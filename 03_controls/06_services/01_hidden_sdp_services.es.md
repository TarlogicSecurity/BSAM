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

En Bluetooth, un servicio representa una funcionalidad específica ofrecida por un dispositivo, identificada mediante un (UUID). Los servicios pueden ser descubiertos y utilizados por otros dispositivos Bluetooth que los soporten.

Bluetooth proporciona múltiples mecanismos para descubrir servicios disponibles, uno de ellos es el Protocolo de Descubrimiento de Servicios (SDP). SDP permite identificar servicios remotos, obtener sus características y determinar cómo conectarse a ellos.

Para identificar servicios ocultos o no documentados, debe realizarse un descubrimiento SDP utilizando herramientas estándar. Tras obtener la lista de servicios anunciados mediante SDP, se deben aplicar técnicas adicionales para verificar que todos los servicios detectados coinciden con los anunciados. La presencia de un servicio no listado en SDP indica un servicio oculto, pero no garantiza seguridad, ya que el acceso sigue siendo posible.

Si un servicio está adecuadamente protegido, ocultarlo no es necesario. Mantener listados SDP precisos mejora la interoperabilidad y cumple con las especificaciones Bluetooth.

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
