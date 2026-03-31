---
layout: control
title: BSAM-AP-01
summary: Actualización del firmware del controller
description: Comprueba que tu dispositivo Bluetooth tiene un mecanismo para actualizar el firmware del controlador. Es importante para corregir errores y vulnerabilidades de seguridad que se puedan encontrar
parent: Aplicación
grand_parent: Controles
nav_order: 1
lang: es
page_id: cont_app_01
permalink: controles/actualizacion-firmware-controlador-bluetooth/
tags:
- BR/EDR
- BLE
---

Los dispositivos Bluetooth deben proporcionar capacidades de actualización de firmware para sus controladores o componentes de driver. Pueden surgir fallos o vulnerabilidades en el firmware del controlador, y la única forma viable de corregirlos es mediante actualizaciones.

Por este motivo, incorporar un mecanismo de actualización de firmware del controlador es esencial. Sin él, cualquier fallo descubierto sería imposible de corregir sin retirar o reemplazar físicamente los dispositivos.

## Descripción del proceso
El procedimiento consiste en verificar que existe un mecanismo de actualización para actualizar el firmware del controlador en el dispositivo en estudio.

Cada fabricante puede optar por incluir diferentes mecanismos propietarios.

Este control es satisfactorio cuando se verifique que el dispositivo admite actualizaciones de firmware de manera remota.

## Recursos relacionados
Para verificar este control los siguientes recursos pueden ser útiles:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}
