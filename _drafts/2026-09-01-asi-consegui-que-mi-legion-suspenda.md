---
title: Así conseguí que mi Lenovo Legion despierte de la suspensión con Linux
date: 2026-09-01 13:20:00 +0200
categories: [Tutorial, Linux]
tags: 
  - suspension
  - Ubuntu
  - Nvidia 
description: Tras varios intentos por instalar Linux en mi Lenovo Legion 5, por fin conseguí solucionar los problemas de suspensión.
image:
  path: /assets/img/posts/YYYY-MM-DD-nombre-tutorial/portada.png
  alt: Descripción de la imagen
toc: true
---

Como todo buen informático siempre que le sea posible, va a instalar Linux en su equipo, y con mi Lenovo Legion 5 82RB no iba a ser menos. Todo esto viene desde hace casi un año, cuando decidí que era momento de migrar de Windows a Linux. En aquel y este momento utilizo el equipo para estudiar, hacer tareas, virtualizar... vamos lo que viene siendo un uso de cualquier estudiante de ASIR. Con Windows sentía el portatil como limitado y quería liberar todo el potencial y que mejor forma de hacer que con Linux, así que me dispuse a instalarlo, elegí en aquel inicio Fedora 43, lo cual para un equipo moderno es lo ideal. Cuando creía que ya lo tenía listo para la vuelta a clases (lo hice durante las vacaciones de Navidad), al bajar la tapa y al volver a abrirla, me encontré con que el portatil no se despertaba, pantalla negra, no respondía, la unica manera era forzando el reinicio "a botonazo".

Me puse manos a la obra en investigar el por que, si era a causa de un problema de configuración o algun parametro faltante en el kernel. Recorrí practicamente todo internet, consulté con la IA, pregunté en subreddits de Linux y especificos de Fedora y ninguna de las soluciones parecía funcionar por lo que lo dejé de lado hasta inicios de este verano. Ya con más tiempo volví a instalar Linux, pero esta vez cambié de distribución, la elegida esta vez fué OpenSUSE Tumbleweed, seguí las guías oficiales de instalación de los drivers de NVidia y me dispuse a realizar algunas pruebas.

## Especificaciones técnicas del equipo.

Antes de pasar con las pruebas quiero plasmar las especificaciones del equipo por si alguien con un equipo similar tiene estos problemas. 

- Lenovo Legion 5 15IAH7H (82RB)
- Intel Core i7-12700H
- iGPU Intel Iris Xe
- dGPU Nvidia RTX 3060 Laptop
- BIOS LENOVO J2CN61WW

## Las pruebas que aclararon el problema.

La primera prueba que hice consistió en cambiar desde la BIOS el tipo de graficos, es decir, cambiarlo de modo "Hybrid" a modo "Dedicated" y probar la suspensión. Cuando lo probé, el equipo se despertó perfectamente, sin tener ningun tipo de problema ni retardo raro. Por lo que descarté que fuese alguna mala configuración del driver de Nvidia.

Volví a cambiar los graficos a "Hybrid", ya que no me permite la BIOS cambiar a un modo de solo integrados, y utilicé EnvyControl para desactivar de forma completa la RTX, ya que con parametros del kernel no se desactivaba la RTX. Al desactivar la RTX, asegurandome de que no fuese detectada por el sistema, probé la suspensión. Para mi sorpresa funcionó perfectamente. Por lo que el panorama era tal que así

| Graficos Discrete desde BIOS | ✅ |
| Graficos iGPU con EnvyControl | ✅ |
| Graficos Hibridos | ❌ |

Con estas pruebas descarté de que fuese un problema con el driver de Nvidia o que faltase algún módulo en el kernel. Era una incompatibilidad con los gráficos hibridos.

Realizando busquedas para ver si alguien con un equipo similar a este solucionó los problemas, muchos de los que encontré usaban el equipo siempre enchufado o para gaming, por lo que poner los graficos en "Discrete" fue la solución. Pero yo quería tener mi equipo funcional en cualquier terreno, ya que en ocasiones necesito usarlo sin el cargador. Volví a dejarlo con Windows ya que necesitaba el equipo, hasta ahora, que volví a intentarlo.

## La prueba de la verdad

Para este intento, utilicé Ubuntu 26.04.1 (que además esta recien salida al momento de redactar esto), ya que Ubuntu siempre se ha caracterizado por tener una mejor integración y soporte con controladores de Nvidia. Empecé a consultar con ChatGPT, le pasé logs, y mas o menos pensabamos que era un conflicto de Gnome con Nvidia y Wayland, por lo que la siguiente prueba fue instalar Linux Mint 22.3, que es base Ubuntu 24.04. Aquí, en Linux Mint me di cuenta de que ofrecía varias versiones del driver de Nvidia, 580-open, 595-open y 610-open. Decidí probar con una versión antigua, la 580-open, ya que en Ubuntu 26 la ultima version que se instala es la 595.84. Instalé la versión 580.173.02 y probé, aqui el comportamiento fue distinto, en vez de quedarse la pantalla negra, aqui la pantalla enviaba señal viendose la pantalla de bloqueo de Cinnamon, pero el sistema se quedaba bloqueado, no respondía a nada.

Forcé el reinicio y probé con la siguiente versión, 595.84 y el resultado fue exactamente identico, sin cambio alguno. Tras volver a forzar otro reinicio probé con la siguiente versión, 610.43.02, que el comportamiento fué el mismo. Estas pruebas las realicé con el ultimo kernel de Mint, 7.0.0-30-generic (que es el mismo que Utiliza Ubuntu).

La IA, investigó y pensó que era una regresión en el kernel, por lo que me recomendó instalar una versión anterior de este, concretamente la 6.17, instalé la versión y probé con el mismo resultado. Hasta este momento el panorama estaba así.

| Driver | Kernel | Resultado |
| 580.173.02 | 7.0.0-30 | Suspende / despierta pero sistema congelado |
| 595.48 | 7.0.0-30 | Suspende / despierta pero sistema congelado |
| 610.43.02 | 7.0.0-30 | Suspende / despierta pero sistema congelado |
| 610.43.02 | 6.17.0-42 | Suspende / despierta pero sistema congelado |

Esto descartó que se tratase de una regresión tanto en el Kernel como en el Driver. ChatGPT analizó logs de mi equipo al intentar salir de la suspensión pero se cortaban en que la suspensión era exitosa, por lo que se llegó a hacer una teoría de que las tablas ACPI de este equipo fuesen demasiado complejas y no manejasen bien en Linux la suspensión con los gráficos hibridos.

Empecé a buscar en internet la ultima versión disponible de Nvidia en Fedora, que es la más puntera en este aspecto, con la idea de instalar Fedora para realizar pruebas con unos drivers más recientes y busqué el changelog de esa ultima versión de drivers. Me encontré con que el ultimo driver, 610.57.04, traía correcciones para el [Runtime D3 (RTD3)](https://www.nvidia.com/en-us/drivers/details/274513/). Así que le pregunté a ChatGPT cómo podía instalar el driver descargandolo desde la web de nvidia, a lo que para mi sorpresa y comodidad, vió que estaba esta misma versión en el repositorio "proposed" esperando a pasar al respositorio de "updates", lo cual facilitaba mucho más la instalación del driver, así no tenía que gestionar por mi cuenta dependencias, ni eliminar la versión anterior instalada. También encontré en la web de [Slimbook](https://slimbook.com/blog/tutoriales-2/post/arreglar-suspension-con-el-driver-de-nvidia-en-ubuntu-y-reducir-el-consumo-energetico-21) un post que hablaba de un arreglo para portatiles con graficos hibridos agregando a grub el parametro ``pcie_aspm=off``, lo probé con la versión del driver 610.43.02 y no cambió el resultado. Por lo que tocaba agregar el repositorio.

### Ubuntu 24.04

```bash
sudo add-apt-repository "deb http://archive.ubuntu.com/ubuntu noble-proposed multiverse"
sudo apt update
```

### Ubuntu 26.04

```bash
sudo add-apt-repository "deb http://archive.ubuntu.com/ubuntu resolute-proposed multiverse"
sudo apt update
```

Tras actualizar, compruebo de que aparezca disponible en los repositorios para instalarlo

```bash
apt-cache policy nvidia-driver-610-open
```

y si se ha agregado el repositorio correctamente deberíamos ver algo como esto

```bash
610.57.04-0ubuntu0.24.04.3 100 # Dependiendo de tu versión la parte final puede cambiar
        100 http://es.archive.ubuntu.com/ubuntu noble-proposed/multiverse amd64 Packages
```

Como veo que aparece, antes de instalar, hago una simulación para ver de que no haya paquetes rotos ni nada parecido

```bash
sudo apt-get -s -t noble-proposed install nvidia-driver-610-open
```
Si la simulación sale todo correcto y no hay errores de paquetes rotos, quito la ``-s`` para instalarlo. Importante usar ``-t noble-proposed`` o ``-t resolute-proposed`` dependiendo de la versión, para especificar que queremos instalarlo desde este repositorio, ya que al tener baja prioridad, va a tomarlo desde el repositorio principal, y en el principal está el actualmente instalado y no va a hacer nada.

Tras instalarlo y reiniciar, probé de nuevo la suspensión, sin el parametro, el cual me dió el mismo resultado. En el siguiente inicio agregué de nuevo el parametro (no lo he mencionado, pero lo ponía de forma temporal para no modificar grub) y dejé el equipo sin usar mientras seguía investigando, se suspendió como era de esperar. Pero esta vez al despertar, se despertó correctamente, manteniendo los graficos hibridos. Por lo que encontré algo muy valioso y dejaba este problema de la siguiente manera

| Etapa | Prueba | Resultado |
| 1 | Graficos Hybrid | ❌ |
| 2 | Solo iGPU | ✅ |
| 3 | Solo Nvidia | ✅ |
| 4 | Nvidia 580 + Kernel 7.0 | ❌ |
| 5 | Nvidia 595 + Kernel 7.0 | ❌ |
| 6 | Nvidia 610.43 + Kernel 7.0 | ❌ |
| 7 | Nvidia 610.43 + Kernel 6.17 | ❌ |
| 8 | Nvidia 610.57.04 + Kernel 7.0 | ❌ |
| 8 | Nvidia 610.57.04 + Kernel 7.0 + ``pcie_aspm=off``| ✅ |

Probé varios ciclos de suspensión y despertar para que fuese fiable y 3 ciclos de 3 funcionaron, por lo que dí por concluida la investigación y por solucionado el problema. Así que decidí instalar Ubuntu 24.04, ya que prefiero usar Wayland antes que X11, he aplicado este proceso y está funcionando como un campeón.

