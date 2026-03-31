---
layout: control
title: BSAM-PA-03
summary: Seguridad de los canales OOB en Bluetooth
description: Comprueba que tu dispositivo Bluetooth utilice un canal OOB seguro para el emparejamiento. Es importante para evitar que un atacante pueda interceptar el tráfico y comprometer el emparejamiento
parent: Emparejamiento
grand_parent: Controles
nav_order: 3
lang: es
page_id: cont_pair_03
permalink: controles/canales-oob-bluetooth/
tags:
- BR/EDR
- BLE
---

En Bluetooth, es necesario establecer una clave compartida entre dos dispositivos para autenticarlos y cifrar las comunicaciones. El emparejamiento presenta ambos dispositivos entre sí y genera esta clave mediante distintos métodos. Algunos dispositivos emplean datos fuera de banda (OOB), intercambiados mediante tecnologías externas como NFC o Wi‑Fi.

A través de un mecanismo OOB, cada dispositivo intercambia los datos de identificación necesarios para la autenticación, funcionando de forma similar a un intercambio manual de PIN. Una vez que estos datos se intercambian, el emparejamiento se considera protegido contra ataques MitM, siempre que el canal OOB sea seguro. No obstante, si el método OOB no está adecuadamente protegido frente a escuchas, un atacante podría capturar los datos intercambiados y comprometer el emparejamiento. El método de verificación para este control depende del mecanismo OOB específico utilizado.


## Descripción

Debe realizarse una captura de los datos de anuncio/emparejamiento de un dispositivo para verificar que un dispositivo desea usar OOB data para su emparejamiento.

Si este es el caso, es necesario analizar el canal externo que se va a usar para el intercambio de los datos. Durante este análisis se ha de certificar que el canal usado es confidencial.

## Caso de ejemplo

Un dispositivo puede compartir con otro mediante un procedimiento fuera de banda (OOB) los datos necesarios para el establecimiento de una conexión. Estos datos tienen que tener el formato que ha pre-fijado el estándar de Bluetooth.

Usaremos Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) para realizar la captura de paquetes para su análisis. 

Cuando se negocia una conexión el comando _Pairing Request_ o _Pairing Response_ tienen un campo llamado _OOB data_, y puede presentar dos valores:

| Valor | Nombre            | Descripción                                                                                           |
|:------|:------------------|:------------------------------------------------------------------------------------------------------|
| 0x00  | Data Not Present  | No hay datos OOB disponibles                                                                          |
| 0x01  | Data Present      | Hay datos OOB disponibles y que deben ser considerados en el proceso de comunicación o emparejamiento.|


![Wireshark OOB data not present]({{ '/assets/img/bsam-pa-03_oob_data_no_present.png' | relative_url}})

Cuando el dispositivo tenga valor `0x01` (`Data Present`) se ha de auditar el mecanismo de intercambio de datos OOB para garantizar que se realiza de manera segura, por lo general a traves de wifi o RFID.


## Recursos relacionados

Para comprobar este control, pueden ser útiles los siguientes recursos:

{% include res_table.md resources='BSAM-RES-08' %}
