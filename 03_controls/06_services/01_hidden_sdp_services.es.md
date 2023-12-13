---
layout: control
title: BSAM-SE-01
summary: Servicios SDP Bluetooth ocultos
description: Comprueba que tu dispositivo Bluetooth liste todos los servicios disponibles en el SDP y no existan servicios ocultos.
parent: Servicios
grand_parent: Controles
nav_order: 1
lang: es
page_id: cont_services_01
permalink: controles/servicios-sdp-bluetooth-ocultos/
tags:
- BR/EDR
- BLE
---

En Bluetooth, un servicio se refiere a una funcionalidad o característica específica que un dispositivo ofrece a otros dispositivos dentro de una red Bluetooth. Cada servicio se identifica por un Identificador Único Universal (UUID), y puede ser descubierto y utilizado por otros dispositivos Bluetooth que soportan ese servicio en particular.

Existen múltiples formas de descubrir los servicios disponibles en un dispositivo Bluetooth. Uno de los mecanismos para llevar a cabo este descubrimiento es el _Protocolo de Descubrimiento de Servicios_ o _SDP_ y su propósito es descubrir servicios en un dispositivo remoto, obtener información sobre los servicios y sus características, y establecer cómo conectarse a ellos.

Si el _SDP_ está presente, debe estar configurado correctamente y listar todos los servicios que deban estar públicamente disponibles en el dispositivo. No listar un servicio en el SDP no impide que los usuarios accedan a él ni lo asegura.

En el mismo contexto, si un servicio está correctamente configurado no es necesario ocultarlo y configurar correctamente los servicios en el _SDP_ permitirá que el dispositivo cumpla las Especificaciones de Bluetooth, lo que permitirá una mejor interoperatividad entre dispositivos.

## Descripción del proceso

Para comprobar si existen servicios ocultos, es importante llevar a cabo un descubrimiento de _SDP_. Este es un procedimiento estándar de Bluetooth se puede realizar con herramientas estándar del sistema.

Después de obtener la lista de los servicios _SDP_ disponibles se deben realizar formas alternativas de descubrimiento de servicios Bluetooth. Esto puede incluir los siguientes procedimientos:

* Capturar comunicaciones Bluetooth mientras se utiliza el dispositivo para descubrir comunicaciones con servicios previamente desconocidos.
* Realizar escaneos de fuerza bruta de servicios en un dispositivo.
* Realizar escaneos de servicios utilizando diferentes protocolos de descubrimiento de servicios como GATT si están disponibles.

Si hay servicios ocultos al servicio _SDP_ este no estará configurado correctamente.

## Recursos relacionados

Para verificar este control, pueden ser útiles los siguientes recursos:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}


## Caso de ejemplo

Se comprueba la existencia de servicios (_Service_) SDP ocultos en un dispositivo con conectividad BR/EDR. Las pruebas se llevan a cabo con un ordenador. Usando Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) se obtiene una captura de paquetes para su análisis.

Para realizar la comprobación de los _Service_ disponibles se hará uso de la herramienta _sdptool_, que forma parte del paquete __bluez__ en los sistemas operativos GNU-Linux. Se realiza la petición de listar los _Service_ del sistema con sdptool browse.

![sdptool not discovered]({{ '/assets/img/bsam-se-01_sdptool.png' | relative_url}})

En este procedimiento se muestran todos los _Service_ que el servidor de SDP tiene grabados. El protocolo de _Descubrimiento de Servicio_ , sin embargo, no tiene un mecanismo para notificar a los clientes SDP cuando se agregan o eliminan _Service_ del servidor SDP. Debido a esto puede haber _Service_ que no estén listados con el comando `sdptool browse XX:XX:XX:XX:XX:XX`.

Se comprueba si existen servicios ocultos con Scapy, o una herramienta similar, se genera una petición de lectura de los atributos de los _Service_ SDP. Para ello se tendrán que comprobar todas las posibilidades disponibles en cada campo:

* _ServiceRecordHandle_
* _MaximumAttributeByteCount_
* _AttributeIDList_
* _ContinuationState_

Para cada petición se obtiene una respuesta que permite comprobar si la petición realizada es correcta o no.

Cotejando las respuestas correctas a la petición de atributos de los servicios con los listados por el servidor SDP se hallarán los _Service_ ocultos.

El resultado del control es _FAIL_ cuando se localizan diferentes servicios en las peticiones de lectura y en el listado del servidor SDP.
