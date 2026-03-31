---
layout: control
title: BSAM-AP-05
summary: Ataques de repetición (Replay)
description: Comprueba si tu dispositivo es vulnerable a ataques de repetición. Es importante para evitar que se puedan reutilizar paquetes válidos para realizar acciones no autorizadas.
parent: Aplicación
grand_parent: Controles
nav_order: 5
lang: es
page_id: cont_app_05
permalink: controles/ataques-repeticion-bluetooth/
tags:
- BR/EDR
- BLE
---

En un ataque de repetición (o ataque de retransmisión), un atacante intercepta y retransmite un mensaje válido. Esto es posible si no existen mecanismos que detecten transmisiones repetidas o garanticen la frescura del mensaje.

Si una aplicación implementa seguridad personalizada mediante criptografía en la capa de aplicación, estas medidas deben proteger adecuadamente contra ataques de repetición.

## Descripción del proceso

El procedimiento consiste en capturar un paquete o transacción válida de un servicio con medidas de seguridad personalizadas y enviarlo de vuelta para comprobar si realiza las acciones deseadas o si el paquete se ignora.

Este control se considera satisfactorio cuando se verifica que el dispositivo no acepta de forma remota el mismo paquete de actualización por segunda vez.

## Recursos relacionados

Para verificar este control, los siguientes recursos pueden ser útiles:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}