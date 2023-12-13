---
layout: control
title: BSAM-AU-03
summary: Desconexión forzada
description: Comprueba si tu dispositivo Bluetooth es vulnerable a ataques de desconexión forzada. Es importante para evitar que un atacante pueda desconectar tu dispositivo de una conexión Bluetooth.
parent: Autenticación
grand_parent: Controles
nav_order: 3
lang: es
page_id: cont_auth_03
permalink: controles/desconexion-forzada-bluetooth/
tags:
- BR/EDR
- BLE
---

Durante una conexión Bluetooth los dispositivos se encargan de mantener la conexión abierta de manera que pueden seguir intercambiando mensajes, estos mensajes tienen un tiempo máximo de respuesta por parte del dispositivo remoto.

## Desconexión BR/EDR
Si se supera el tiempo de espera la conexión se cierra automáticamente. Para el cierre de una conexión por parte de uno de los dispositivos, cualquiera de ellos puede enviar un mensaje _LMP_detach_ con el motivo de la desconexión.

Esta desconexión sólo debería permitirse mediante mensajes provenientes de la misma conexión que se intenta cerrar. Un atacante puede crear una nueva conexión suplantando a otro dispositivo y enviar un mensaje de desconexión, pudiendo desconectar a todos los dispositivos con la dirección MAC del mensaje. Este comportamiento puede ser aprovechado para producir denegaciones de servicio e iniciar otros ataques.

## Desconexión BLE
Si se supera el tiempo de espera el dispositivo local envía una cancelación del envío anterior. Si el comando está relacionado con el proceso de emparejamiento el resultado será la cancelación del emparejamiento con un dato del motivo de la desconexión.

Además de ocurrir una desconexión debido al agotamiento de tiempo para una respuesta en BLE existe la posibilidad de forzar un desemparejamiento en cualquier momento de la conexión mediante el comando _HCI_Disconnect_, el Handle indicará la conexión que se desea desconectar, en función de si la conexión es síncrona o asíncrona el PDU del mensaje será distinto:

| Tipo Conexión   |  Mensaje PDU         | Opcode |
|:----------------|:---------------------|:-------|
| Asíncrona (ACL) | LL_TERMINATE_IND     | 0x02   |
| Síncrona (CIS)  | LL_CIS_TERMINATE_IND | 0x22   |


## Descripción del proceso

Suponiendo el escenario en el que los dispositivos A y B están conectados, se debe probar que el dispositivo C no puede forzar la desconexión de B. Para ello, C debe suplantar la identidad de B e iniciar una conexión con A para enviar inmediatamente un mensaje de desconexión.

Para que el control se cumpla, el proceso debe ser rechazado o el mensaje de desconexión sólo debe desconectar a C, pero no a B.

## Recursos relacionados

Algunos recursos relacionados con este control son los siguientes:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07,BSAM-RES-09' %}

## Caso de ejemplo

Usaremos Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) para realizar la captura de paquetes para su análisis.

Se dispone de unos auriculares Bluetooth los cuales están emparejados con un dispositivo móvil durante la prueba. Estos permiten realizar una nueva conexión con un portátil con la aplicación Scapy, manteniendo la conexión simultanea entre ambos dispositivos.

Para iniciar la conexión el portátil, con la ayuda de la herramienta Scapy, inicia el proceso de conexión con el comando _LE Create Connection_.

![Wireshark LE Create Connection]({{ '/assets/img/bsam-au-03_create_connection.png' | relative_url}})

Los auriculares notifican la aceptación de la conexión con el evento _LE Meta_ con el valor `0x00` (`Success`) del campo _Status_.

![Wireshark Connection complete]({{ '/assets/img/bsam-au-03_connection_complete.png' | relative_url}})

Tras aceptar la conexión se ha solicitado la desconexión procediendo a desconectar a todos los dispositivos que estaban conectados con los auriculares con el comando _Disconnect_.

![Wireshark Disconnect]({{ '/assets/img/bsam-au-03_disconnect.png' | relative_url}})

Se puede comprobar que el portátil se ha desconectado en la captura de Wireshark por el comando _Disconnect Complete_.

![Wireshark Forced Disconnect full process]({{ '/assets/img/bsam-au-03_forced_disconnection.png' | relative_url}})

El auditor debe de comprobar que ya no está emparejado con otros dispositivos.

El resultado del control será _FAIL_ si se desconecta del resto de dispositivos.


## Referencias Externas

* Bluetooth Core V5.3, Vol. 6, Part B, Section 4.5.12 Connection Termination and loss
* Bluetooth Core V5.3, Vol. 6, Part B, Section 5.1.6 ACL Termination Procedure
* Bluetooth Core V5.3, Vol. 6, Part B, Section 5.1.16 Connected Isochronous Stream Termination procedure
* Bluetooth Core V5.3, Vol. 4, Part E, Section 7.1.6 Disconnect Command