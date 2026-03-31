---
layout: control
title: BSAM-DI-05
summary: Descubrimiento de dispositivo
description: Comprueba que tu dispositivo Bluetooth no sea descubrible por defecto para reducir la superficie de ataque.
parent: Descubrimiento
grand_parent: Controles
nav_order: 5
lang: es
page_id: cont_disco_05
permalink: controles/descubrimiento-dispositivos-bluetooth/
tags:
- BR/EDR
- BLE
---

Los dispositivos Bluetooth pueden permanecer en modo visible anunciándose continuamente o respondiendo a solicitudes de descubrimiento, facilitando conexiones automáticas para comodidad del usuario. Sin embargo, la visibilidad no debe estar habilitada por defecto, solo debe activarse cuando sea estrictamente necesario. Permitir que el usuario controle manualmente este estado ayuda a evitar detecciones innecesarias, incluso si afecta a la usabilidad.

Los dispositivos que permanecen visibles exponen información como la dirección MAC, el nombre del dispositivo y la versión de Bluetooth soportada, lo que puede utilizarse para rastreo, suplantación u otros ataques dirigidos. Por lo tanto, la visibilidad debe permanecer desactivada excepto cuando sea necesaria para el emparejamiento o conexión.

Los dispositivos equipados con controles de entrada (por ejemplo, botones o teclados) deben proporcionar un medio para activar o desactivar la visibilidad mediante estos controles.

## Descripción del proceso

Para cumplir el requisito se debe validar que sólo es posible descubrir el dispositivo al cambiar el estado a descubrible y sólo durante un tiempo limitado o hasta que se establezca una conexión o se desactive el estado manualmente.

## Recursos relacionados

Para comprobar la descubribilidad del dispositivo se pueden obtener los `beacons` de Bluetooth LE o los mensajes de `Extended Inquiry Response` mediante los siguientes recursos:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-07,BSAM-RES-08' %}

## Caso de ejemplo

Se encienden unos auriculares Bluetooth para llevar a cabo este análisis. Al abrir Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) es posible capturar los paquetes para su posterior análisis. Los `beacons` ofrecen visibilidad sobre el dispositivo que los emite, en este caso, los auriculares.

![Wireshark BR/EDR Extended inquiry result]({{'/assets/img/bsam-di-05_extended_inquiry_result.png' | relative_url}})

El resultado del control será _FAIL_ cuando un dispositivo es descubrible por defecto.

Se puede mejorar este comportamiento haciendo que los auriculares no fuesen descubribles a menos que el usuario forzase ese modo. Para lograr la conexión automática de los auriculares, deberían ser estos los que tratasen de iniciar una conexión con el dispositivo móvil u ordenador al que estaban conectados previamente.
