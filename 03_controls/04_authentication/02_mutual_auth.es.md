---
layout: control
title: BSAM-AU-02
summary: Autenticación mutua
description: Comprueba si tu dispositivo Bluetooth admite la autenticación mutua. Es importante para evitar ataques de suplantación de identidad, en los que un dispositivo malicioso puede hacerse pasar por otro dispositivo.

parent: Autenticación
grand_parent: Controles
nav_order: 2
lang: es
page_id: cont_auth_02
permalink: controles/autenticacion-mutua-bluetooth/
tags:
- BR/EDR
---

Durante el proceso de autenticación Bluetooth, no es necesario que los dos dispositivos involucrados comprueben la identidad el otro. Esto puede dar pie a ataques de suplantación de identidad, en los que un dispositivo malicioso puede hacerse pasar por otro dispositivo.

El proceso de autenticación se puede realizar mediante los métodos:
 * `Legacy Authentication`: Realiza autenticación unilateralmente, de maestro a esclavo, y puede permitir que el maestro de una comunicación no sea autenticado. Se debe evitar ya que permite realizar ataques de suplantación de identidad.
 * `Secure Authentication`: Exige la autenticación de ambas partes de la comunicación, evitando que alguna de ellas sea suplantada por un tercer dispositivo.


## Descripción del proceso

Para comprobar si el dispositivo admite `Legacy Authentication`, es necesario modificar las capacidades LMP del controlador Bluetooth. Las capacidades LMP (LMP features) indican las funcionalidades que soporta el controlador Bluetooth. En concreto, las capacidades "Secure Connections (Host Support)" y "Secure Connections (Controller Support)" indican si el host y el controlador soportan conexiones tipo Secure Connection, que requieren el método de autenticación `Secure Authentication`.

Para comprobar si el dispositivo puede ser forzado a reducir la seguridad de la autenticación a `Legacy Authentication`, se establecen los bits correspondientes a las capacidades "Secure Connections (Host Support)" y "Secure Connections (Controller Support)" a 0 en nuestro dispositivo.

Dependiendo del fabricante del controlador, esto será posible utilizando un mensaje específico de HCI. En el caso de la placa de desarrollo CYW920819EVB-02, el fabricante, Broadcom, permite escrituras en memoria RAM a través de un mensaje HCI, y, gracias a la PoC de la vulnerabilidad [BIAS](https://github.com/francozappa/bias), se conoce la localización en memoria de las cadenas de bits correspondientes a las LMP features, por lo que se pueden sobrescribir asegurando siempre que el dispositivo indique que no se soportan las conexiones de tipo "Secure Connection".

Tras modificar las capacidades del controlador, se inicia una conexión con el dispositivo auditado. Si el dispositivo inicia la autenticación mediante `Legacy Authentication` el control no se cumple.


## Recursos relacionados

Para comprobar este control, pueden ser útiles los siguientes recursos:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07,BSAM-RES-09' %}

## Descripción del proceso

Usaremos Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) para realizar la captura de paquetes para su análisis.

Para habilitar la autenticación mutua entre dos dispositivos uno Central y otro Periférico se tiene que cumplir que el host del dispositivo admita conexiones seguras. Esto se hace indicando un valor `0x01` (`Secure_Connections_Host_Support is ‘enabled’. Host supports Secure Connections.`) en el campo _Secure\_Connections\_Host\_Support_ del comando _HCI\_Write\_Secure\_Connections\_Host\_Support_.

Este paquete deberá de ser localizado en las capturas de Wireshark, durante el proceso de emparejamiento, entre el dispositivo Central y el Periférico para verificar su valor. La no existencia del mismo ya indica que los dispositivos no configuraron la conexión segura.

El resultado del control será _FAIL_ si no se encuentra el comando _HCI\_Write\_Secure\_Connections\_Host\_Support_ o si se encuentra con un valor del campo _Secure\_Connections\_Host\_Support_ distinto a `0x01`.
