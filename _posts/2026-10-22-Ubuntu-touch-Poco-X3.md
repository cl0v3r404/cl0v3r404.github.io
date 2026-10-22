---
title: Ubuntu Touch en el Poco X3 NFC, ¿merece la pena?
date: 2026-10-22 18:49:00 +0200  # verano
categories: [Linux, Smartphones]
tags: [ubuntu-touch, linuxphone]
author: cl0v3r
description: Instalé Ubuntu Touch en mi antiguo Poco X3 NFC, ¿será que Linux en móviles merecerá la pena?
image:
  path: assets/img/posts/2026-10-22-Ubuntu-touch-Poco-X3/portada.jpg
  alt: Ubuntu Touch en Poco X3 NFC
---

Llevo tiempo coqueteando con la idea de instalar y probar Ubuntu Touch, y por suerte, tengo uno de los dispositivos soportados por el proyecto oficial de [UBports](https://www.ubuntu-touch.io/), mi veterano Poco X3 NFC. Eché un vistazo en la web del proyecto, y para mi sorpresa, el Poco X3 tiene un muy buen soporte, prácticamente todo (por no decir todo) funciona, así que me puse manos a la obra.

## Prerrequisitos.
Antes de instalar realicé una copia de los datos que tengo actuales, no es un teléfono que use a diario, es un dispositivo secundario que utilizo para cacharrear. Realicé copia de seguridad de datos, descargué la ROM que tenía instalada, CRDroid 12 y la ROM que pide Ubuntu Touch como base, que es [MIUI V12.0.9.0.QJGMIXM](https://xmfirmwareupdater.com/miui/surya/stable/V12.0.9.0.QJGMIXM/).

### Flasheo de MIUI.
Si alguno ha tenido o tiene un Xiaomi y ha flasheado una ROM original, conocerá el proceso. Previamente hay que tener el Bootloader desbloqueado, que era mi caso, ya que tenía CRDroid instalada. Desbloquear el Bootloader se ha vuelto un proceso algo complejo en dispositivos nuevos con HyperOS, pero en dispositivos más antiguos con MIUI, el proceso es más sencillo, la única pega es que hay que esperar 1 semana para poder desbloquearlo. Enlazo una guía de [Mundo Xiaomi](https://www.mundoxiaomi.com/tutoriales/como-desbloquear-bootloader-xiaomi-para-instalar-otras-roms) que lo explica bastante bien.

Desbloqueado el Bootloader, toca flashear la ROM base, para flashearla necesitaremos la herramienta [Xiaomi Mi FlashTool](https://xiaomiflashtool.com/). Antes de abrirla, una vez descargada, hay que crear una carpeta que no viene creada llamada _log_, con ella creada, abrimos el programa, si pide instalar los drivers, instalamos, se cerrará el programa en vez de mostrar una ventana de _Instalación correcta_ es normal, abrimos luego el programa y buscamos la ROM extraída.
![buscar ROM](<../assets/img/posts/2026-10-22-Ubuntu-touch-Poco-X3/flasheo buscar.png>)

Con la ROM seleccionada, hay que asegurarse de que en la parte inferior de la ventana, no ponga _clean all and lock_ porque tocaría volver a desbloquear el Bootloader, marcamos la opción de _clean all_.
![clean all flash](<../assets/img/posts/2026-10-22-Ubuntu-touch-Poco-X3/clean all flash.png>)

## Flasheo de Ubuntu Touch.
Con la ROM de MIUI ya flasheada, podemos instalar Ubuntu Touch. Tan solo hay que descargar el [instalador](https://devices.ubuntu-touch.io/device/surya/#installerDownload) desde la web. Está para cualquier plataforma, Windows, Linux y macOS. Al descargar y abrir el instalador, hay dos formas de hacer que instale el sistema, conectándolo, teniendo previamente los ajustes de desarrollador habilitados y la depuración USB, o seleccionándolo de la lista.
![UBports installer](<../assets/img/posts/2026-10-22-Ubuntu-touch-Poco-X3/Instalador UBPorts.png>)

Es tan sencillo, que una vez se seleccione el dispositivo o lo detecte, seguir las instrucciones del instalador. Él se encargará de descargar e instalar el sistema.

## Mi experiencia.
La experiencia del sistema se siente bien, los menús son parecidos a los de Android, tienen una estética muy consistente, pero los gestos para navegar por el sistema son algo extraños y lleva algo de rato acostumbrarse.

Yo instalé la versión anterior, la 24.04.1 (porque soy así de boludo) en vez de la última 24.04.2, pero me ha servido para experimentar el proceso de actualización. Lo instalé por la noche, y no fui capaz de descargar la actualización, tardaba mucho, se cortaba la descarga o no la encontraba, pero al día siguiente se descargó rápido y la encontró al momento. El proceso de actualización es bastante sencillo, como si actualizáramos un móvil Android cualquiera, se reiniciará cuando esté lista para instalarse.

### Lo bueno.

- No pensaba que esto que fuese a decir sería un punto positivo, pero lo ha sido. En la versión anterior, 24.04.1, los bordes de la pantalla cortaba parte del reloj haciendo que fuese poco legible la hora, algunos elementos de la barra de estado se tapaban con el notch de la pantalla, al igual que los del centro de control. Con la 24.04.2 eso se ha solucionado, ahora los bordes de la pantalla se respetan, el notch lo evita quedando todo a la vista sin que este tape nada.

- El sistema se siente fluido y bien hecho, no falla nada, lector de huellas, NFC, GPS, SIM, llamadas de voz, prácticamente todo lo básico funciona.

- La mayoría de la gente cuando mencionas la palabra "Linux" se les viene a la cabeza las palabras "difícil, hackers y técnico" pero Ubuntu Touch demuestra que no tienes que ser un Hacker o un experto en informática para poder usar el sistema. Todo tiene interfaz gráfica, todo lo mueves con el dedo, no usas la terminal para absolutamente nada, a pesar de que viene una ya instalada. Las apps se descargan desde una App Store llamada _OpenStore_ con solo pulsar un botón.

### Lo malo.
Como todo en la vida tiene su lado negativo, y Ubuntu Touch no iba a ser menos.

- Creo que este es el principal problema de este sistema, la falta de apps. No tenemos un cliente oficial de WhatsApp, si bien existe uno, pero se utiliza como dispositivo vinculado, lo que te obliga a tener otro dispositivo en el que tener WhatsApp instalado. También existen clientes alternativos a muchas apps diarias, pero muchas de estas, bien son accesos directos a las webs oficiales, como la de los bancos (sí, me sorprendí al ver que existían apps de banco, pero realmente son accesos directos a las webs), YouTube, YouTube Music y otras tantas. También hay clientes de apps escritos nativamente para el sistema, y muchas son de buena calidad, pero muchas fallan, como es el caso del cliente de Bitwarden, Sealed, que en mi caso fue imposible iniciar sesión con la cuenta. Hay otras que funcionan bien, no vamos a decir que todas son malas.

## Conclusión. ¿Merece la pena Ubuntu Touch?
Aquí depende de cada usuario en valorar si merece la pena instalarlo o no. Como dispositivo secundario que solo vas a usar de forma ocasional y que te pueda servir para mantener contacto con lo básico, para hacer un "detox digital" puede servirte. Funciona lo básico para mantenerte en contacto, llamadas de voz, SMS, GPS, Mail... Pero si planteas usarlo como dispositivo principal o dispositivo secundario donde puedas necesitar apps tipo bancarias, no es la mejor opción. Como he dicho, depende de cada uno el valorar si merece la pena.

En mi caso personal, Ubuntu Touch no es lo que voy buscando, ya que muchas apps de las que uso, como Bitwarden, que para mí es básico, no funcionan. Podría usarlo como un dispositivo básico, sí, pero este Poco X3 me ha salvado cuando no he tenido mi Galaxy S23, por lo que desprenderme de Android no es la mejor de las ideas. Si fuese otro dispositivo más antiguo que apenas merezca la pena a día de hoy usar como dispositivo de emergencia, lo dejaría instalado porque le puede dar una segunda vida bastante interesante, y el proyecto avanza lento, pero con buen pie. En este Poco X3 volveré a CRDroid ya que es la ROM que mejor me ha funcionado.