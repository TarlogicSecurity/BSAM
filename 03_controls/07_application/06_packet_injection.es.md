---
layout: control
title: BSAM-AP-06
summary: Ataques de inyección de paquetes Bluetooth
description: Aplica métodos de cifrado seguros en la aplicación Bluetooth para prevenir ataques de suplantación e inyección de paquetes.
parent: Aplicación
grand_parent: Controles
nav_order: 6
lang: es
page_id: cont_app_06
permalink: controles/inyeccion-paquetes-bluetooth/
tags:
- BR/EDR
- BLE
---

Un ataque de inyección de paquetes consiste en el envío deliberado de paquetes de datos alterados o fabricados con el objetivo de manipular o perturbar las operaciones normales de sus dispositivos conectados. Esto es posible cuando no hay ningún tipo de comprobación de que el paquete tiene un formato correcto o es enviado por un dispositivo legítimo.

Si una aplicación requiere métodos de seguridad personalizados y decide utilizar la criptografía para un servicio en particular, los métodos de seguridad a nivel de la capa de aplicación deben ser adecuados para prevenir ataques de suplantación e inyección de paquetes.

No cumplir con este control puede significar que, a pesar de los esfuerzos por utilizar medidas de seguridad a nivel de aplicación, estos puedan eludirse.


## Descripción del proceso

El procedimiento consiste en estudiar y analizar si los métodos de cifrado seleccionados son seguros contra ataques de suplantación e inyección de paquetes.

Este control se considera satisfactorio cuando se verifica que el mecanismo de generación de un paquete válido no es tan trivial que permita elaborar nuevos paquetes sin conocer la clave de cifrado del paquete.


## Recursos relacionados

Para verificar este control, los siguientes recursos pueden ser útiles:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}

## Caso de ejemplo

Durante una auditoría se encuentra un dispositivo con cifrado personalizado a nivel de aplicación. En particular, un usuario puede establecer una contraseña secreta en el dispositivo que cifra el tráfico a nivel de aplicación.

Los paquetes de datos intercambiados con el dispositivo incluyen una PDU que contiene un número de _secuencia_ para evitar ataques de repetición, como se describe en {% include ctl_link.md ctl='BSAM-AP-05' %}. Este número se incrementa cada vez que se envía un paquete y se verifica que el nuevo número sea mayor que el último registro en el dispositivo. La PDU también contiene un campo CRC32 para verificar la integridad del paquete.

Los paquetes están cifrados usando un cifrado de flujo. Esto significa que se genera un flujo pseudoaleatorio y se realiza una operación XOR con el paquete original.

En este escenario, un atacante puede capturar un mensaje cifrado c para un texto plano desconocido _m_. El atacante puede calcular un mensaje cifrado _c'_ = _c_ ⊕ (Δ, CRC(Δ)) que se descifrará correctamente en un mensaje _m'_ = _m_ ⊕ Δ . Si el atacante utiliza los cambios Δ para modificar la parte del número de _secuencia_ del mensaje, puede reinyectar paquetes en el dispositivo sin conocer su contenido.
