---
title: Ubuntu Touch en el Poco X3 NFC, ¿merece la pena?
date: 2026-10-22 10:49:00 +0200  # verano
categories: [Linux, Smartphones]
tags: [ubuntu-touch, linuxphone, etiqueta3]
description: Instalé Ubuntu Touch en mi antiguo Poco X3 NFC, ¿será que Linux en móviles merecerá la pena?
image:
  path: /ruta/a/imagen.jpg
 # lqip: data:image/webp;base64,UklGRiYAAABXRUJQ... # opcional, para carga progresiva
  alt: Descripción alternativa de la imagen
---

Llevo tiempo coqueteando con la idea de instalar y probar Ubuntu Touch, y por suerte, tengo uno de los dispositivos soportados por el proyecto oficial de [UbPorts](https://www.ubuntu-touch.io/), mi veterano Poco X3 NFC. Eché un vistazo en la web del proyecto, y para mi sorpresa, el Poco X3 tiene un muy buen soporte, prácticamente todo (por no decir todo) funciona, así que me puse manos a la obra.

## Pre-requisitos
Antes de instalar realicé una copia de los datos que tengo actuales, no es un telefono que use a dirario, es un dispositivo secundario que utilizo para cacharrear. Realicé copia de seguridad de datos, descargúe la ROM que tenía instalada, CRDroid 12 y la ROM que pide Ubnutu Touch como base, que es [MIUI V12.0.9.0.QJGMIXM](https://xmfirmwareupdater.com/miui/surya/stable/V12.0.9.0.QJGMIXM/).

### Flahseo de MIUI
Si alguno ha tenido o tiene un Xiaomi y ha flasheado una ROM original, conocerá el proceso. Previamente hay que tener el Bootloader desbloqueado, que era mi caso, ya que tenía CRDroid instalada. Desbloquear el Bootloader se ha vuelto un proceso algo complejo en dispositivos nuevos con HyperOS, pero en dispositivos más antiguos con MIUI, el proceso es mas sencillo, la única pega es que hay que esperar 1 semana para poder desbloquearlo. Enlazo una guía de [Mundo Xiaomi](https://www.mundoxiaomi.com/tutoriales/como-desbloquear-bootloader-xiaomi-para-instalar-otras-roms) que lo explica bastante bien.

Desbloqueado el Bootloader, toca flashear la ROM base, para flashearla necesitaremos la herramienta [Xiaomi Mi FlashTool](https://xiaomiflashtool.com/). Antes de abrirla, una vez descargada, hay que crear una carpeta que no viene creada llamada _log_, con ella creada, abrimos el programa, si pide instalar los drivers, instalamos, se cerrará el programa en vez de mostrar una ventana de _Instalación correcta_ es normal, abrimos luego el programa y buscamos la ROM extraida. 
![buscar ROM](<../assets/img/posts/2026-10-22-Ubuntu-touch-Poco-X3/flasheo buscar.png>)

Con la ROM seleccionada, hay que asegurarse de que en la parte inferior de la ventana, no ponga _clean all and lock_ porque tocaría volver a desbloquear el Bootloader, marcamos la opción de _clean all_.
![clean all flash](<../assets/img/posts/2026-10-22-Ubuntu-touch-Poco-X3/clean all flash.png>)