---
layout: control
title: BSAM-PA-08
summary: Borrado de claves de enlace bluetooth
description: Los dispositivos Bluetooth deben evitar eliminar claves de enlace compartidas al recibir nuevas peticiones de emparejamiento. Esto puede ser aprovechado por atacantes para secuestrar las comunicaciones.
parent: Emparejamiento
grand_parent: Controles
nav_order: 8
lang: es
page_id: cont_pair_08
permalink: controles/borrado-claves-enlace-bluetooth/
tags:
- BR/EDR
- BLE
---

Una vez realizado el emparejamiento entre dos dispositivos, estos almacenan las claves de enlace generadas en este proceso, de manera que las puedan utilizar en el futuro para establecer nuevas conexiones.
Los dispositivos Bluetooth deben evitar eliminar claves de enlace compartidas al recibir nuevas peticiones de emparejamiento. Esto puede ser aprovechado por atacantes para secuestrar las comunicaciones.

Existen pequeñas diferencias para este caso entre Bluetooth clásico y baja energía:

* _Bluetooth Classic_ (BR/EDR) en las funciones de autenticación y cifrado, Bluetooth utiliza una clave compartida, creada durante el emparejamiento, que es almacenada en una base de datos en cada dispositivo. Algunos dispositivos Bluetooth eliminan esta clave compartida al recibir una nueva petición de emparejamiento aparentemente proveniente del dispositivo original.

* _Bluetooth Low Energy_ (BLE) tras el emparejamiento en fase 2 se distribuyen las claves LTK y STK pero únicamente se almacén las claves LTK para ser reutilizadas en futuras reconexiones. No se describe en el estándar el tamaño y las características de la base de datos de seguridad y es posible que un dispositivo local (iniciador) cuya dirección MAC es conocida inicie un nuevo emparejamiento y esta clave sea borrada del dispositivo remoto (respondedor).

En cualquiera de los dos casos esto permite que un atacante pueda forzar el desemparejamiento entre dos dispositivos y aproveche la situación para generar un nuevo emparejamiento borrando así la clave conocida entre los dos dispositivos legítimos. 

Esta situación también le permite la captura de un nuevo emparejamiento legítimo entre ambos dispositivos con el objetivo de obtener la clave de una conexión, haciendo uso de la fuerza bruta del código PIN, con menor entropía que otras partes del cifrado de Bluetooth.

Esta es una forma de secuestrar las comunicaciones entre dos dispositivos que se denomina Bluesnarfing. Para prevenirlo, se debe restringir la eliminación de la clave compartida únicamente cuando ambos dispositivos están debidamente vinculados en una conexión autenticada.


## Descripción del proceso

Consideraremos que A y B son dos dispositivos emparejados entre sí y que C es un dispositivo usado para la verificación del control. Para comprobar si C puede desemparejar a B de A maliciosamente, se pueden probar diferentes métodos:

* Suplantar al dispositivo B y realizar un emparejamiento con A
* Suplantar al dispositivo B e iniciar una conexión autenticada con A sin tener clave de enlace
* Suplantar al dispositivo B e iniciar una conexión autenticada con A con una clave de enlace falsa

En el primer caso, si A no cumple este control, es posible que A acepte el nuevo emparejamiento y borre la clave antigua.

En el segundo caso, al intentar iniciar la autenticación, el controlador de C enviará el error "key or pin missing". Algunos dispositivos, al recibir este error, eliminan la clave anterior y pasan a estar desemparejados.

En el tercer caso, C fallará la autenticación con A. Algunos dispositivos, al recibir una autenticación fallida, eliminan la clave compartida y pasan a estar desemparejados.

En cualquiera de los casos, A no debería eliminar la clave compartida, para evitar que un tercero pueda desemparejar ambos dispositivos y tratar posteriormente de capturar el enlace.


## Recursos relacionados

Algunos recursos relacionados con este control son los siguientes:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07,BSAM-RES-09,BSAM-RES-10' %}

## Caso de ejemplo

Usaremos Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) para realizar la captura de paquetes para su análisis. 

Tenemos un ordenador con Scapy instalado, y unos auriculares Bluetooth emparejados con un teléfono móvil.

El ordenador está suplantando con la herramienta _scapy_ al teléfono móvil y trata de recuperar una conexión previa con los auriculares y que este se la denegará mediante el comando _Link Key Request Negative Reply_.

![Wireshark Link Key Request Negative Reply]({{ '/assets/img/bsam-pa-01_link_key_request_negative_reply.png' | relative_url}})

El ordenador realiza una nueva conexión a los auriculares solicitando las capacidades de entrada/salida _IO Capability_ con el comando _IO Capabilitiy Request_.

![Wireshark IO Caps Request]({{ '/assets/img/bsam-pa-01_io_cap_request.png' | relative_url}})

Tras compartir las _IO Capabilities_ de ambos dispositivos se establece una conexión, notificada mediante el comando _Simple Pairing Complete_.

![Wireshark Simple Pairing]({{ '/assets/img/bsam-pa-01_simple_pairing_complete.png' | relative_url}})

Tras el establecimiento comienza un proceso de negociación de clave entre los dos controladores de Bluetooth que no es posible registrarlos en Wireshark, pero que finaliza por la notificación de una nueva clave de enlace con el comando _HCI\_Link\_Key\_Notification_.

Para comprobar que los auriculares fueron sensibles a este ataque se intenta conectar desde el teléfono móvil y si responden con el comando _Link Key Request Negative Reply_ quiere decir que ha sido posible borrar la clave de enlace y por lo tanto un _FAIL_ par este control.


## Recursos externos

* Bluetooth Core V5.3, Vol. 3, Part H, Appendix B.1 DATABASE LOOKUP