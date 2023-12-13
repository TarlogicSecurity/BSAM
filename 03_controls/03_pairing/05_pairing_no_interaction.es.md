---
layout: control
title: BSAM-PA-05
summary: Emparejamiento sin interacción del usuario
description: Evita que tu dispositivo Bluetooth se empareje automáticamente para protegerte de ataques.
parent: Emparejamiento
grand_parent: Controles
nav_order: 5
lang: es
page_id: cont_pair_05
permalink: controles/emparejamiento-sin-intervencion-bluetooth/
tags:
- BR/EDR
- BLE
---

En Bluetooth es necesario establecer una clave de enlace compartida entre dos dispositivos para cifrar las comunicaciones. El procedimiento de emparejamiento "presenta" a ambos dispositivos entre sí y establece la clave compartida. Existen diferentes métodos para establecer la clave compartida y algunos de estos no implementan controles de seguridad para verificar la identidad de los dispositivos involucrados. 

El método _just works_ permite realizar un emparejamiento mediante una clave numérica, que se realiza sin interacción del usuario, y no implementa medidas para asegurarse que los dispositivos son conocidos por el usuario. Todo el material criptográfico que se usa para una conexión _just works_ durante el proceso de emparejamiento es enviado en la banda de Bluetooth sin ningún tipo de cifrado por lo que es posible con un sniffer capturar esta información y suplantar al dispositivo.

Adicionalmente si el modo de emparejamiento es _legacy_ este es susceptible a ataques de tipo _mitm_.


## Descripción del proceso

Se deben comprobar los siguientes dos elementos para verificar que se cumple el control:
 * El campo "Authentication requirements" del mensaje "IO Capability Response" debe mostrar un valor con protección MITM.
 * El dispositivo debe rechazar cualquier intento de emparejamiento mediante el método "just works".

El mensaje "IO Capability Response", que contiene el campo "Authentication Requirements" se puede capturar durante un emparejamiento con una herramienta de usuario o utilizando Scapy para simular un emparejamiento parcial.

Para verificar que no es posible emparejarse utilizando el método "just works", se comprueba qué capacidades tiene el dispositivo y se simula una conexión mediante el método "just works" que debe ser rechazada.

El control sólo se cumple cuando se utiliza un valor para "Authentication Requirements" que incluya protección MITM y no es posible realizar un emparejamiento con "just works".


## Recursos relacionados

Para comprobar este control, pueden ser útiles los siguientes recursos:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-07,BSAM-RES-09' %}


## Caso de ejemplo

Utilizaremos Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) para capturar los paquetes y analizarlos.

Se utiliza la herramienta de configuración de dispositivos Bluetooth del sistema operativo del portátil para iniciar la conexión. En Bluetooth Classic, la conexión entre los dos dispositivos comienza con el comando _IO Capability Request_.

![Wireshark IO Caps Request]({{ '/assets/img/bsam-pa-01_io_cap_request.png' | relative_url}})

Los auriculares responden mediante el comando _IO Capability Request Reply_ con unas _IO Capability_ `0x03` (`NoInputNoOutput`). Esto permite emparejamientos sin intervención del usuario.

![Wireshark IO Caps Request Reply]({{ '/assets/img/bsam-pa-01_io_cap_request_reply.png' | relative_url}})

Se confirma unos mensajes después el emparejamiento mediante el comando _Simple Pairing Complete_.

![Wireshark Simple Pairing]({{ '/assets/img/bsam-pa-05_pairing_no_user_interaction.png' | relative_url}})

Las capacidades de entrada / salida de los auriculares han permitido el emparejamiento sin confirmación ni notificación al usuario.

El resultado del control será _FAIL_ cuando un dispositivo es emparejable sin intervención del usuario.

El mecanismo de emparejamiento _Just Works_ debe evitarse. En este caso, el dispositivo contaba con botones en ambos auriculares y pueden aprovecharse como botón Yes/No, realizando así una confirmación de emparejamiento.

## Referencias externas 

* Bluetooth Core V5.3, Vol. 3, Part C, Section 5.2.2.4 - IO capabilities
* Bluetooth Core V5.3, Vol. 3, Part C, Section 5.2.2.5 - Mapping of input / output capabilities to IO capability
* Bluetooth Core V5.3, Vol. 3, Part C, Section 5.2.2.6 - IO and OOB capability mapping to authentication stage 1 method
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.2 - IO capabilities
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.1 - Selecting key generation method
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.1 Pairing Methods
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.2 LE Legacy pairing - Just Works
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.4 Out Of Band
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.6.2 Aunthentication Stage 1 - Just Works or Numeric Comparison