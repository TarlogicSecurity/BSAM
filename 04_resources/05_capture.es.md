---
layout: default
title: BSAM-RES-05
summary: Captura de una conexión Bluetooth
description: Captura de una conexión Bluetooth entre un dispositivo y su controlador para analizar el tráfico Bluetooth
parent: Recursos
nav_order: 4
lang: es
page_id: BSAM_resources_05
permalink: recursos/capturar-conexion-bluetooth/
---

# {{ page.summary }}

Esta técnica consiste en la captura de una conexión con el mismo dispositivo con el que se realiza la conexión Bluetooth. En resumen, el proceso consiste en capturar los paquetes intercambiados entre el "Host" y el "Controller" de un dispositivo mientras este se conecta a otro. El protocolo usado para intercambiar mensajes entre el controlador y el "Host" es HCI (Host Controller Interface). Todas las herramientas listadas a continuación sirven para capturar y/o almacenar paquetes del protocolo HCI en uno u otro formato.

Dado que esta técnica solo captura el protocolo HCI, muchos de los paquetes de la capa "Link Layer" intercambiados entre controladores no estarán presentes en la captura. Existen mecanismos de depuración mediante los cuales es posible capturar estos paquetes, pero solo están disponibles en hardware específico. Consulte {% include res_link.md res='BSAM-RES-06' %} para más información.

Esta técnica requiere herramientas y procedimientos que dependen del sistema operativo en uso. A continuación, se listan algunas alternativas para los sistemas operativos más comunes.


## Captura Bluetooth en Linux

En Linux existen distintas alternativas para capturar paquetes de conexiones establecidas con nuestra máquina. Casi todas las máquinas Linux con soporte para Bluetooth tienen la pila `BlueZ` instalada por lo que la herramienta `btmon` es una buena alternativa para hacer capturas sin necesidad de instalar muchas dependencias. `Wireshark` es una de las alternativas más atractivas ya que cuenta con la posibilidad de diseccionar paquetes gráficamente. Para los casos en los que es necesario interactuar de manera programática con los paquetes o realizar capturas automatizadas, `Scapy` es una buena alternativa.

Referencias:

* [btmon](https://man.archlinux.org/man/extra/bluez-utils/btmon.1.en)
* [Wireshark](https://wiki.wireshark.org/Bluetooth)
* [Scapy](https://scapy.readthedocs.io/en/latest/layers/bluetooth.html)


## Captura Bluetooth en Windows

Windows también ofrece una herramienta nativa para el volcado de paquetes de Bluetooth conocida como `BTVS` o `Bluetooth Virtual Sniffer` con capacidad para volcar en formato `Wireshark` para su posterior análisis. `Wireshark` también soporta hacer un volcado de paquetes Bluetooth directamente en Windows.

Referencias:

* [Wireshark](https://wiki.wireshark.org/Bluetooth)
* [Bluetooth Virtual Sniffer](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs)


## Captura Bluetooth en MacOS, iOS, tvOS & watchOS

En las plataformas de Apple, el único método soportado para el volcado de paquetes Bluetooth es la herramienta de depuración `PacketLogger`. Es compatible con todas las plataformas de Apple y permite exportar las capturas posteriormente a un formato compatible con `Wireshark`.

Referencias:

* [PacketLogger](https://developer.apple.com/bug-reporting/profiles-and-logs/?name=bluetooth)


## Captura Bluetooth en Android

Android soporta de manera nativa la exportación de logs de comunicaciones mediante un mecanismo conocido como `btsnoop` o `registro de Bluetooth HCI`. Existen scripts de Frida como `Frida BLEMon` que permiten instrumentar llamadas del API de Bluetooth para generar volcados de comunicaciones Bluetooth.

La obtención de los registros `btsnoop` difiere para cada dispositivo Android y para cada fabricante, aunque algunos pasos pueden ser comunes.

### Opciones para desarrolladores

En la mayoría de dispositivos Android, el `registro de Bluetooth HCI` es una funcionalidad para desarrolladores, por lo que hay que activar las opciones para desarrolladores para poder utilizarlo.

Para ello, se accede a `Ajustes > Información del teléfono` y se pulsa sobre `Número de compilación` repetidamente hasta que aparezca una notificación indicando que las opciones para desarrolladores están activadas.

### Obtención de registros btsnoop en OxygenOS 11.1.2.2

Con las opciones para desarrolladores activadas, se accede a `Ajustes > Sistema > Opciones para desarrolladores` y se pulsa sobre `Habilitar registro de Bluetooth HCI` para habilitar el registro. Luego, pausa y vuelve a iniciar la función Bluetooth del teléfono. A partir de ahora, todos los mensajes HCI se registran en archivos de log mientras la opción `Habilitar registro de Bluetooth HCI` permanezca habilitada.

Para extraer los archivos de log del teléfono, se accede a `Ajustes > Sistema > Opciones para desarrolladores > Obtener registros` y se inicia la captura `Bluetooth Exception`, pulsando sobre `NOT REBOOT` para evitar reiniciar el terminal. Este paso sólo sirve para extraer los logs previamente generados, no es necesario repetir las pruebas con la captura de `Bluetooth Exception` activa.

Después de unos segundos, se detiene la captura de `Bluetooth Exception` y se espera a que el informe sea generado. Cuando haya terminado, se pulsa sobre `SHARE` y se selecciona la carpeta `btsnoop`. Luego, se pulsa sobre el menú `(...)` y sobre `Share` para enviar la carpeta `btsnoop`, que contiene todos los `registros de Bluetooth HCI`, a alguna otra ubicación donde analizarlos.

Referencias:

* [Btsnoop.log](https://source.android.com/docs/core/connect/bluetooth/verifying_debugging?hl=es-419#debugging-with-logs)
* [Frida BLEMon](https://github.com/optiv/blemon)
