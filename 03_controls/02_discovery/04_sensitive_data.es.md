---
layout: control
title: BSAM-DI-04
summary: Exposición de datos sensibles
description: Los dispositivos Bluetooth pueden exponer datos sensibles en los mensajes de descubrimiento. Analiza estos datos para evitar que se exponga información personal.
parent: Descubrimiento
grand_parent: Controles
nav_order: 4
lang: es
page_id: cont_disco_04
permalink: controles/exposicion-datos-bluetooth/
tags:
- BR/EDR
- BLE
---


Dentro de los paquetes de _advertising_ en BLE, y en algunas respuestas extendidas de _inquiry_ en BR/EDR, pueden estar presentes datos adicionales incorporados por el fabricante.

Algunos de los mensajes de descubrimiento contienen datos específicos del fabricante (_Manufacturer Specific Data_) que pueden contener información sensible y deben ser analizados con detenimiento.

Estos datos deben analizarse en busca de información sensible y verificar que no se están exponiendo datos innecesarios.


## Descripción del proceso

Capturando los mensajes emitidos durante la fase de descubrimiento, mediante `beacons` LE o en las respuestas de `inquiry` en BR/EDR, se pueden encontrar datos de interés, como los servicios que expone el dispositivo, el nombre o datos del fabricante.

En los `beacons` LE se exponen datos de fabricante en los campos `AdvData`, mientras que en BR/EDR, se exponen en los mensajes `Extended Inquiry Result`. En ambos casos el campo es de tipo `Manufacturer Specific Data`, con id `0xFF`, y se indica un identificador del fabricante que puede ser consultado en el documento _Bluetooth Assigned Numbers_.

El contenido de los datos de tipo _Manufacturer Specific Data_ se encuentra en un formato específico del fabricante que puede no ser público, por lo que puede requerir un esfuerzo de ingeniería inversa para ser decodificado.

Algunos formatos populares para este campo son los siguientes:

| Eddystone | <https://github.com/google/eddystone> |
| Microsoft | <https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cdp/77b446d0-8cea-4821-ad21-fabdf4d9a569?redirectedfrom=MSDN> |
| iBeacon   | <https://en.wikipedia.org/wiki/IBeacon> |


## Recursos relacionados

Para obtener los `beacons` de Bluetooth LE o los mensajes de `Extended Inquiry Response` pueden ser útiles los siguientes recursos:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-08' %}


## Caso de ejemplo

Unos auriculares, durante el descubrimiento, emiten dos `beacons` LE diferentes y responden a peticiones de `inquiry` con un mensaje de `Extended Inquiry Result`. Usando Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) se realiza la captura de paquetes para su análisis.

Al abrir Wireshark el ordenador comienza el escaneo de dispositivos descubribles. Los `beacons` del dispositivo publican información en diferentes formatos. El primero indica como ID del fabricante Samsung y se puede leer parte del nombre del dispositivo:

![Beacon con datos de Samsung]({{ '/assets/img/bsam-di-04_samsung_beacon.png' | relative_url}})

En el segundo se indica un ID de fabricante de Microsoft:

![Beacon con datos de Microsoft]({{ '/assets/img/bsam-di-04_microsoft_beacon.png' | relative_url}})

El mensaje `Extended Inquiry Result` contiene diferentes datos con el ID de fabricante de Samsung, además de dos listados de `UUIDs` correspondientes a los servicios del dispositivo:

![Extended Inquiry Result con datos de Samsung]({{ '/assets/img/bsam-di-04_samsung_eir.png' | relative_url}})

En los tres casos se encuentran datos de fabricante con una codificación desconocida, pero muestran datos sobre el nombre del dispositivo (Buds2) y su utilidad ([Headphones]).

Los servicios expuestos, por otro lado, indican 5 servicios con UUIDs no estándar que exponen servicios propios del fabricante. Esta información puede ser útil al auditor en posteriores fases del análisis y es preferible evitar exponer servicios innecesariamente.

Dado que esta información debería ser accesible exclusivamente en un contexto más limitado es interesante resaltar en el informe posibles soluciones a esta evaluación. Se emite una recomendación de sólo hacer accesible el nombre y los servicios del dispositivo durante el modo de emparejamiento o para otro dispositivo autenticado. Ninguno de los servicios debería estar accesible para dispositivos no autenticados.
