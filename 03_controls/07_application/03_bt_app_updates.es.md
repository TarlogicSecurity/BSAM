---
layout: control
title: BSAM-AP-03
summary: Actualización de la aplicación
description: Comprueba que tu dispositivo Bluetooth tiene un mecanismo para actualizar el software de la aplicación. Es importante para corregir errores y vulnerabilidades de seguridad que se puedan encontrar
parent: Aplicación
grand_parent: Controles
nav_order: 3
lang: es
page_id: cont_app_03
permalink: controles/actualizacion-software-bluetooth/
tags:
- BR/EDR
- BLE
---

Los dispositivos con capacidades Bluetooth requieren de herramientas y aplicaciones que hagan uso de estas comunicaciones, si se quiere dotar de nuevas funcionalidades o corregir problemas, estos dispositivos han de contar con un mecanismo de actualización.

En la vida útil de un dispositivo existe la posibilidd de que se encuentren errores o vulnerabilidades de seguridad en las aplicaciones software. Estas aplicaciones pueden ser entendidas como aplicaciones en dispositivos como los smartphones pero también aplicaciones que consumen los recursos Bluetooth en sistemas embedidos menos potentes como las aplicaciones realizadas en plataformas de hardware y software libre. El único mecanismo que se puede utilizar para solucionar estos errores y vulnerabilidades de seguridad es mediante actualizaciones de las aplicaciones software.

Por las razones mencionadas anteriormente es fundamental incluir un mecanismo de actualización del software que consume los recursos del controlador Bluetooth en un dispositivo. De lo contrario, si se encuentra un problema, será inviable solucionarlo sin tener que retirar todos los dispositivos en uso.

## Descripción del proceso

El procedimiento consiste en verificar que existe un mecanismo de actualización para actualizar el software del controlador en el dispositivo en estudio.

Cada fabricante puede optar por incluir diferentes mecanismos propietarios.

Este control es satisfactorio cuando se verifique que el dispositivo admite actualizaciones del software de manera remota.

## Recursos relacionados
Para verificar este control los siguientes recursos pueden ser útiles:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}