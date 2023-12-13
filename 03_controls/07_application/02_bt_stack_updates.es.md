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

El estándar de bluetooth evoluciona y cada cierto tiempo aparece una nueva versión disponible por lo que los dispositivos puede contar con mecanismos de actualización de este estándar de bluetooth por una pila de bluetooth con nuevas capacidades y que arregle problemas de seguridad.

Durante la vida útil de un dispositivo que utiliza tecnología Bluetooth es posible que se encuentren errores o vulnerabilidades de seguridad en la pila Bluetooth. El único mecanismo que se puede utilizar para solucionar estas vulnerabilidas de seguridad es mediante actualizaciones de pila de Bluetooth (una parte del firmware del dispositivo).

Por las razones mencionadas anteriormente es fundamental incluir un mecanismo de actualización de la pila de Bluetooth o del firmware que incluye esta en un dispositivo. De lo contrario, si se encuentra un problema, será inviable solucionarlo sin tener que retirar todos los dispositivos en uso.

## Descripción del proceso

El procedimiento consiste en verificar que existe un mecanismo de actualización para actualizar la pila de Bluetooth del controlador en el dispositivo en estudio.

Cada fabricante puede optar por incluir diferentes mecanismos propietarios.

Este control es satisfactorio cuando se verifique que el dispositivo admite actualizaciones de la pila de bluetooth de manera remota.

## Recursos relacionados
Para verificar este control los siguientes recursos pueden ser útiles:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}