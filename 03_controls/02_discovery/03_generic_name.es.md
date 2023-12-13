---
layout: control
title: BSAM-DI-03
summary: Nombre del dispositivo genérico
description: Evita usar nombres de dispositivos Bluetooth que revelen información personal o del dispositivo.
parent: Descubrimiento
grand_parent: Controles
nav_order: 3
lang: es
page_id: cont_disco_03
permalink: controles/nombre-dispositivo-bluetooth/
tags:
- BR/EDR
- BLE
---

Los dispositivos Bluetooth pueden enviar de manera pública, y sin necesidad de autenticación o autorización, su nombre y otros datos relacionados.

El nombre Bluetooth de un dispositivo puede dar información sobre el tipo de dispositivo, su usuario, incluir la propia MAC o un ID único. Es recomendable que el dispositivo tenga un nombre genérico y desvele la mínima información necesaria.

El conocer a quien pertenece un dispositivo Bluetooth permite ataques dirigidos y deriva en un problema de privacidad al poder identificar un dispositivo de manera univoca en un momento y lugar concreto.

## Descripción del proceso

El nombre del dispositivo puede aparecer durante el descubrimiento, en los anuncios BLE o en las respuestas de `Inquiry`, o solicitándolo con un mensaje `HCI_Remote_Name_Request`.

En el primer caso, se puede encontrar el nombre simplemente durante el proceso de descubrimiento, por lo que cualquier aplicación Bluetooth será de utilidad para mostrarlo. Sin embargo, algunos dispositivos no envían el nombre en los paquetes de descubrimiento, por lo que será necesario solicitarlo activamente.

Para este segundo caso, se establece una conexión con el dispositivo y se solicita el nombre con el mandato HCI `HCI_Remote_Name_Request`.

El nombre obtenido de esta forma no debe contener datos que indiquen el propósito del dispositivo ni datos personales de su usuario.

## Recursos relacionados

Para obtener el nombre pueden ser útiles los siguientes recursos:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-07,BSAM-RES-08' %}

## Caso de ejemplo
Usaremos Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) para realizar la captura de paquetes para su análisis. 

Al abrir Wireshark se reciben los paquetes de anuncio de los dispositivos cercanos. Alguno dispositivos pueden enviar datos en sus paquetes de anuncio, como el valor del _device name_ `L600474`.

![Wireshark LE Meta Device Name]({{ '/assets/img/bsam-di-03_august_device_name.png' | relative_url}})

El nombre indica a un posible atacante la existencia de este dispositivo y puede proporcionar incluso el modelo del mismo.