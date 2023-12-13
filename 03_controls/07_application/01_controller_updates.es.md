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

Los dispositivos bluetooth pueden contar con capacidades de actualizacion del firmware del controlador.

Siempre existe la posibilidad de que se encuentren errores o vulnerabilidades de seguridad en el firmware del controlador de un dispositivo. El único mecanismo que se puede utilizar para solucionar estos errores y vulnerabilidades de seguridad es mediante actualizaciones de firmware.

Por las razones mencionadas anteriormente es fundamental incluir un mecanismo de actualización del firmware del controlador en un dispositivo. De lo contrario, si se encuentra un problema, será inviable sin tener que retirar todos los dispositivos en uso.

## Descripción del proceso
El procedimiento consiste en verificar que existe un mecanismo de actualización para actualizar el firmware del controlador en el dispositivo en estudio.

Cada fabricante puede optar por incluir diferentes mecanismos propietarios.

Este control es satisfactorio cuando se verifique que el dispositivo admite actualizaciones de firmware de manera remota.

## Recursos relacionados
Para verificar este control los siguientes recursos pueden ser útiles:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}
