---
title: Así migré de Promox a OpenMediaVault
date: YYYY-MM-DD HH:MM:SS +0200
categories:
  - HomeLab
tags:
  - omv
  - proxmox
  - docker
description: Preparativos antes de un cambio de arquitectura
image:
  path: /ruta/a/imagen.jpg
  alt: Descripción alternativa de la imagen
---
Como todo cambio de arquitectura requiere un proceso de preparación, y en esta publicación mostraré que hice para preparar esa migración. Para que no sea muy extenso, lo dividiré en varias partes, así también separo la parte de la preparación, copias de seguridad, etc, de la parte de configuración. En este post trataré el tema de los preparativos, que he hecho para preparar el cambio y como proceder, así que sin más preámbulo empezamos.

## Preparación del entorno, LXCs a Docker

Para facilitar la transición y evitar re-configurar todos y cada uno de los servicios que tengo desplegados, he decidido migrar cada uno de los LXC a Docker, así desplegar los contenedores en OMV cuando esté preparado será un proceso más rápido y me evito horas o incluso días de configurar estos, o pelearme con configuraciones que no tengo documentadas (fallo por mi parte). 

### Paso 1: Localización de las carpetas

Aunque pueda sonar algo complicado cambiar una instalación "baremetal" a contenerizada, es sencillo, basta con localizar las rutas de las carpetas de configuración, bases de datos o datos almacenados por las aplicaciones (en caso de que no los estemos almacenando en un almacenamiento externo o seguro), varía según el servicio, es recomendable leer la documentación de cada uno de los servicios que se tengan desplegados para encontrar las carpetas. Algunos tienen en su demonio de SystemD rutas configuradas, por lo que se pueden comprobar desde estos mismos, tomaré de ejemplo mi migración de calibre web. Todos los comandos que estoy poniendo, los he ejecutado desde la consola de Proxmox.

```sh
pct exec <lxcid> -- systemctl list-units --type=service | grep -i calibre
pct exec <lxcid> -- systemctl cat calibre-web
```

Con estos comandos podemos obtener información del directorio de trabajo `WorkingDirectory=/opt/calibre-web` y del directorio desde donde se ejecuta `ExecStart=/opt/calibre-web/.venv/bin/python /opt/calibre-web/cps.py`. Si es como en mi caso, que en calibre web aparecían 3 bases de datos, con comparar las fechas tenemos la que verdaderamente está en uso. Luego tocaría localizar la ruta de almacenamiento, donde, en este caso subo los libros, podemos obtenerla si no hemos configurado nada nosotros, pero si configuramos nosotros la ruta exacta, la tendremos localizada seguro.

```sh
pct exec <lxcid> -- find / -iname 'metadata.db' 2>/dev/null
```

### Paso 2: establecer clave SSH en LXC destino

Una vez tengamos todos los directorios importantes localizados, procedemos a crear una conexión entre el LXC origen y destino, para ello lo que hice fue usar claves ssh, primero para hacerlo más fácil y rápido, y si, como yo, que desplegué el LXC con los script de *community scripts* estos LXC no tendrán contraseña (a menos que la establecieses), por lo que conectarse con ssh normal, no sería posible. 

```sh
# Creamos el archivo SSH
pct exec <lxcorigen> -- bash -c "test -f /root/.ssh/id_ed25519 || ssh-keygen -t ed25519 -N '' -f /root/.ssh/id_ed25519"

# Extraemos la clave publica hacia el Host
pct exec <lxcorigen> -- cat /root/.ssh/id_ed25519.pub > /tmp/id_calibre.pub

# Mandamos la clave al LXC destino
pct push <lxcdestino> /tmp/id_calibre.pub /root/.ssh/authorized_keys_calibre

# Agregamos la clave a las autorizadas y borramos el archivo temporal
pct exec <lxcdestino> -- bash -c "cat /root/.ssh/authorized_keys_calibre >> /root/.ssh/authorized_keys && rm /root/.ssh/authorized_keys_calibre"

# Asignamos permisos
pct exec <lxcdestino> -- chmod 600 /root/.ssh/authorized_keys

#Comprobamos que funcione la clave
pct exec <lxcorigen> -- ssh -i /root/.ssh/id_ed25519 root@<IP_LXC_106> "echo ok"
```

### Paso 3: Copiar las carpetas con rsync

Si la clave funciona, nos devolverá un OK, por lo que podemos pasar a copiar las carpetas, para ello he usado `rsync` para no cambiarle permisos a las carpetas en la operación de moverlas. Si el LXC no sabemos si tiene `rsync` instalado, hay un comando que he usado, que nos comprueba si esta presente y si no lo instala. Esto hay que hacerlo en ambos LXC.

```sh
pct exec <lxcorigen> -- which rsync || pct exec <lxcorigen> -- apt install -y rsync

pct exec <lxcdestino> -- which rsync || pct exec <lxcdestino> -- apt install -y rsync
```

Una vez tengamos `rsync` instalado o nos hayamos asegurado de que exista creamos las carpetas necesarias, como estoy usando calibre web de ejemplo, he tenido que crear la carpeta `config` en una ruta designada para almacenar los datos de las aplicaciones. 

```sh
pct exec <lxcdestino> -- mkdir -p /mnt/docker/calibre-web/config
```

Ahora, para mover los archivos con total seguridad, detenemos el servicio y procedemos a copiarlo con `rsync`.

```sh
pct exec <lxcorigen> -- systemctl stop calibre-web

pct exec <lxcorigen> -- rsync -avzP --numeric-ids /opt/calibre-web/app.db root@<IP_LXC_DESTINO>:/mnt/docker/calibre-web/config/app.db
```

Cuando la copia termine es bueno asegurarse de que los permisos no hayan cambiado

```sh
pct exec <lxcdestino> -- ls -la "/mnt/mp/calibre-web/Calibre WEB" | head -5   
# nobody:nogroup, pero con permisos de lectura para "otros" (r-x) — accesible igual
pct exec <lxcdestino> -- ls -la /mnt/docker/calibre-web/config/app.db          
# 1000:1000, coincide con el propietario original — sin necesidad de chown
```

### Paso 4: Creación del docker-compose con las rutas correspondientes

Si al compararlos con el LXC original, hay algún permiso cambiado lo arreglamos con `chown`. Si todo está OK, procedemos a crear el `docker-compose` apuntando en este a las rutas de configuración donde hemos guardado nuestra configuración existente y biblioteca.

```
    volumes:
      - "/mnt/docker/calibre-web/config:/config"
      - "/mnt/mp/calibre-web/Calibre WEB:/books"
```
_Se pone con comillas dobles para que podamos poner espacios en la ruta_

Desplegamos el compose y una vez desplegado, lo probamos, y si ha ido bien, deberíamos poder acceder con nuestro usuario. Puede ser que la primera vez tengamos que entrar con el usuario para volver a apuntar a nuestra biblioteca, pero una vez apuntemos a ella, veremos que todo esta en orden. 

El procedimiento ha sido prácticamente igual para todos, depende también del servicio que quieras migrar y de los archivos que requieras migrar. Importante apuntar a las carpetas donde hemos guardado toda nuestra configuración migrada. 


## Copia de los datos de Docker

De que serviría todo esto, si no copiamos nuestra carpeta de las configuraciones de las aplicaciones para llevárnoslas. Recomiendo hacer la copia de esto a un almacenamiento externo, HDD, USB, NAS, Nube, GitHub, el que sea de vuestra preferencia. Yo los voy a copiar a una carpeta en el NAS para luego moverlas a la carpeta destinada a Docker. 

También recomiendo sacar los `docker-compose` con sus `.env` para no tener que batallar luego con configuraciones o rebuscar en el repositorio, de esta manera será mas fácil y rápido, lo que se debe tener en cuenta, que una vez migre a OMV los compose, tengo que cambiar las rutas del almacenamiento. 

## El adiós a Proxmox

Teniendo ya todo copiado y asegurándome de que todo los contenedores funcionen en Docker, habiendo respaldado las configuraciones y los compose files, puedo, oficialmente, apagar Proxmox para empezar a sustituirlo por OMV. 

En el siguiente post, lo retomaré desde la instalación de OMV, que usaré la ISO oficial por rapidez, abarcaremos algunas configuraciones básica que haré para dejar OMV listo para funcionar. 

> [Advertencia]
> Como último, decir que he utilizado Claude durante el proceso, para solucionar problemas que iban saliendo, buscar en la documentación oficial más rápidamente y revisar logs por si se me escapa algo. algunos comandos fueron proporcionados por Claude y fueron comprobados por mi parte, por lo que se pueden usar de forma segura.

