---
layout: default
title: BSAM-RES-07
summary: Envío y recepción de mensajes HCI
description: Envía y recibe mensajes HCI para controlar el controlador Bluetooth y obtener información sobre el dispositivo
parent: Recursos
nav_order: 6
lang: es
page_id: BSAM_resources_07
permalink: recursos/enviar-recibir-mensajes-hci/
---

# {{ page.summary }}

## Host Controller Interface

El dispositivo controlador se comunica con el host a través de mensajes HCI (Host Controller Interface). La interfaz HCI define y limita el control que se puede ejercer desde el host y es la base de cualquier herramienta de escritorio.

HCI transporta datos L2CAP entre host y controlador a través de mensajes de datos (HCI Data Packets), pero también permite interactuar con el controlador a través de otros dos tipos de mensajes:

| Tipo de mensaje | Descripción |
|:----------------|:------------|
| HCI Command     | Contiene una instrucción enviada desde el host hacia el controlador |
| HCI Event       | Contiene un evento enviado desde el controlador hacia el host       |

Los mensajes HCI Command contienen una instrucción para el controlador y son siempre enviados desde el host. Los mensajes HCI Event se envían siempre desde el controlador hacia el host y pueden contener diferentes datos:
- Tras la recepción de una instrucción, el controlador puede responder con un mensaje de estado (Command Status Event).
- Tras completar un proceso solicitado por el host, el controlador puede emitir un mensaje con el resultado del proceso (Command Complete Event).
- El controlador puede emitir un evento con otro tipo de información de forma no solicitada.

Cada HCI Command tiene asignado un Opcode de 2 bytes como identificador unívoco. Los 6 bits más significativos del Opcode indican el Opcode Group Field (OGF) y los 10 menos significativos el Opcode Command Field (OCF): el OGF clasifica los mensajes en grupos y el OCF indica el mensaje dentro del grupo. El siguiente ejemplo muestra el Opcode `0x0401` (`0b0000010000000001`), que corresponde al mensaje HCI Inquiry (OCF `0x0001`) del grupo Link Control (OGF `0x01`):

| OGF      | OCF          |
|:---------|:-------------|
| 0b000001 | 0b0000000001 |

Los eventos se identifican mediante un _event code_ unívoco de 1 byte asignado en el estándar.

El listado completo de los mensajes estándar HCI Command y HCI Event con la descripción de sus parámetros se encuentra en Bluetooth Core Specification v5.3, Volumen 2, Parte E (Host Controller Interface Functional Specification), aunque la organización y el contenido puede variar entre versiones del documento.

Además de los mensajes estándar, los fabricantes de controladores pueden implementar mensajes _vendor-specific_ con funcionalidades extra utilizando el OGF `0x3f`. Algunos fabricantes implementan funcionalidades de depuración a través de este mecanismo que permiten un control más completo del controlador. Algunas tarjetas Broadcom, por ejemplo, permiten recibir los mensajes LMP intercambiados entre los dispositivos.

Referencias:
* Bluetooth Core Specification v5.3, Volumen 2, Parte E (Host Controller Interface Functional Specification)


## Herramientas HCI

El proyecto BlueZ, junto con la pila de Bluetooth usada habitualmente en Linux, distribuye diferentes herramientas que permiten interaccionar con dispositivos Bluetooth a diferentes niveles. `hcitool` y `hciconfig`, en particular, permiten enviar diferentes mensajes HCI, configurar conexiones y consultar parámetros.

A pesar de ser consideradas obsoletas y no contar con soporte, algunas de sus funcionalidades no están cubiertas por las herramientas más modernas que las sustituyen, como `bluetoothctl`.

Ambas herramientas están normalmente disponibles como paquetes en las diferentes distribuciones de Linux. En Arch Linux, ambas se encuentran disponibles en el paquete AUR `bluez-utils-compat`. En Kali Linux, se encuentran en el paquete `bluez`.

Para enviar un mensaje HCI no soportado por ninguna de las herramientas, se puede utilizar la opción `cmd` de `hcitool`, que permite enviar un mensaje HCI Command con Opcode y parámetros arbitrarios:

```bash
hcitool cmd <ogf> <ocf> [parameters]
```

El siguiente ejemplo envía el un mensaje _vendor-specific_ para placas Broadcom que activa el log de depuración con los mensajes LMP intercambiados entre dispositivos:

```bash
hcitool cmd 0x3f 0xf0 0x01
```

Referencias:
* [bluez source](https://github.com/bluez/bluez)
* [hcitool source](https://github.com/bluez/bluez/blob/master/tools/hcitool.c)
* [hciconfig source](https://github.com/bluez/bluez/blob/master/tools/hciconfig.c)

## Bibliotecas y APIs
Además de las herramientas mostradas, los sistemas operativos como Linux ofrecen APIs a través de las cuales interactuar con la pila de Bluetooth y enviar mensajes a distintos niveles.

En Linux, se puede interactuar con HCI a través de sockets, utilizando el protocolo `BTPROTO_HCI` al crear el socket. Se pueden encontrar ejemplos del uso de sockets HCI en las herramientas del repositorio de BlueZ.

En Python, existe la API PyBluez, que permite interactuar con HCI a través de la clase `BluetoothSocket`, sin embargo, se recomienda el uso del módulo Scapy, que facilita el envío de paquetes y es fácilmente extensible.

Muchos de los paquetes HCI no se encuentran implementados y es necesario especificarlos como extensión a las bibliotecas disponibles, por lo que Scapy facilita gran parte de la tarea.

Referencias:
* [btmon source](https://github.com/bluez/bluez/blob/master/tools/btmon-logger.c)
* [bluez tools source](https://github.com/bluez/bluez/tree/master/tools)
* [PyBluez](https://pybluez.readthedocs.io/en/latest/)
* [Scapy](https://scapy.net/?ref=glue)
