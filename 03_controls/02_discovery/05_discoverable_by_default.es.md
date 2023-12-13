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

Los dispositivos Bluetooth tienen la capacidad de ser descubribles en todo momento, ya sea porque se anuncian activamente mediante el envío de paquetes de _advertising_ o porque ante consultas de tipo _inquiry_ responden indicando que se encuentran disponibles. Esto es así por comodidad de cara al usuario, de manera que los dispositivos se conectan automáticamente cuando estén cerca. 
Es una buena práctica que este descubrimiento no este activo por defecto y que lo esté únicamente bajo demanda del usuario, de manera que pueda activar y desactivar este estado para no ser detectado e identificado cuando no es necesario, aun penalizando la usabilidad y experiencia de usuario.


Los dispositivos descubribles por defecto permiten la extracción de parte de la información necesaria para ser suplantados (MAC, Nombre, Versión de Bluetooth soportada...), además de otra información relevante. Se recomienda mantener el dispositivo en estado _no descubrible_ mientras no sea necesario realizar el proceso de emparejamiento o la conexión.

Los dispositivos con controles de entrada, como botones y teclados, o elementos similares, deben permitir cambiar el estado de descubribilidad mediante estos controles.

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
