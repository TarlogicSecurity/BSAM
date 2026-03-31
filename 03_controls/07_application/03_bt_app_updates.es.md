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

Los dispositivos con capacidades Bluetooth dependen de software de capa de aplicación que gestiona y consume los servicios Bluetooth. Para agregar nuevas funciones o corregir problemas, estos dispositivos requieren un mecanismo de actualización de software.

Como pueden surgir fallos o vulnerabilidades en esta capa a lo largo del tiempo, las actualizaciones son esenciales. Sin ellas, resolver los problemas identificados sería imposible sin retirar los dispositivos.

## Descripción del proceso

El procedimiento consiste en verificar que existe un mecanismo de actualización para actualizar el software del controlador en el dispositivo en estudio.

Cada fabricante puede optar por incluir diferentes mecanismos propietarios.

Este control es satisfactorio cuando se verifique que el dispositivo admite actualizaciones del software de manera remota.

## Recursos relacionados
Para verificar este control los siguientes recursos pueden ser útiles:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}