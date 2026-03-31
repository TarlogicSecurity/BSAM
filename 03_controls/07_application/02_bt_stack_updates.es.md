---
layout: control
title: BSAM-AP-02
summary: Actualización del stack de Bluetooth
description: Comprueba que tu dispositivo Bluetooth admite actualizaciones de la pila Bluetooth para corregir errores y vulnerabilidades.
parent: Aplicación
grand_parent: Controles
nav_order: 2
lang: es
page_id: cont_app_02
permalink: controles/actualizacion-stack-bluetooth/
tags:
- BR/EDR
- BLE
---

El estándar Bluetooth evoluciona con el tiempo, introduciendo nuevas versiones con capacidades ampliadas y mejoras de seguridad. Por ello, los dispositivos deben incluir mecanismos para actualizar la pila Bluetooth, que forma parte del firmware del dispositivo.

Pueden surgir vulnerabilidades en la pila durante la vida útil del dispositivo y las actualizaciones software son la forma de corregirlas. Sin un mecanismo de actualización, cualquier problema identificado requeriría la retirada de todos los dispositivos.

## Descripción del proceso

El procedimiento consiste en verificar que existe un mecanismo de actualización para actualizar la pila de Bluetooth del controlador en el dispositivo en estudio.

Cada fabricante puede optar por incluir diferentes mecanismos propietarios.

Este control es satisfactorio cuando se verifique que el dispositivo admite actualizaciones de la pila de bluetooth de manera remota.

## Recursos relacionados
Para verificar este control los siguientes recursos pueden ser útiles:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}