---
layout: control
title: BSAM-PA-01
summary: Modo de emparejamiento por defecto
description: Comprueba que tu dispositivo Bluetooth no esté en modo de emparejamiento por defecto. Evita que un atacante pueda emparejarse con tu dispositivo sin tu intervención.
parent: Emparejamiento
grand_parent: Controles
nav_order: 1
lang: es
page_id: cont_pair_01
permalink: controles/dispositivo-bluetooth-modo-emparejable/
tags:
- BR/EDR
- BLE
---

El emparejamiento es el paso inicial en la comunicación Bluetooth, donde se establece la confianza para permitir futuras conexiones. Un dispositivo puede operar en estado emparejable o no emparejable, solo cuando está en estado emparejable procesará solicitudes de emparejamiento entrantes. Si está en estado no emparejable, ignorará dichas solicitudes, evitando intentos de emparejamiento no deseados o no autorizados.

Los dispositivos que responden automáticamente a solicitudes de emparejamiento sin intervención del usuario pueden exponer información excesiva, aumentando el riesgo de suplantación e incrementando la superficie de ataque. Por lo tanto, el emparejamiento debe restringirse a situaciones estrictamente necesarias, y el dispositivo debe requerir interacción física del usuario, como pulsar un botón para iniciar el proceso.


## Descripción del proceso

Se ha de probar que únicamente es posible emparejarse con el dispositivo al cambiar su estado a emparejable. El cambio de modo a emparejable ha de requerir la intervención del usuario para habilitarse. El modo emparejable debe estar habilitado durante un tiempo limitado, hasta que se realice un emparejamiento o el propio usuario desactive el estado manualmente.


## Recursos relacionados

Para comprobar si el dispositivo es emparejable se puede iniciar un proceso de emparejamiento con herramientas de usuario o utilizando bibliotecas de paquetes como Scapy. Del apartado de recursos, pueden ser útiles los siguientes:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07' %}


## Caso de ejemplo

Unos auriculares Bluetooth son descubribles y emparejables después de encenderlos. Durante ese tiempo, otro dispositivo no emparejado puede acceder a información sobre estos auriculares mediante peticiones de emparejamiento sin que el usuario sea notificado.

Usaremos Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) para realizar la captura de paquetes para su análisis. 

El portátil realiza una nueva conexión solicitando las capacidades de entrada/salida 'IO Capability' con el comando _IO Capabilitiy Request_:

![Wireshark IO Caps Request]({{ '/assets/img/bsam-pa-01_io_cap_request.png' | relative_url}})

Los auriculares en ese momento permiten la conexión y responden con un sus 'IO Capability' con el comando _IO Capability Request Reply_.

![Wireshark IO Caps Request Reply]({{ '/assets/img/bsam-pa-01_io_cap_request_reply.png' | relative_url}})

El procedimiento culmina con el establecimiento de la conexión notificado mediante el comando _Simple Pairing Complete_.

![Wireshark Simple Pairing]({{ '/assets/img/bsam-pa-01_simple_pairing_complete.png' | relative_url}})

El resultado del control será _FAIL_ cuando un dispositivo es emparejable por defecto.
