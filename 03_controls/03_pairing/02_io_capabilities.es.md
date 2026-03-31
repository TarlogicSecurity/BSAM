---
layout: control
title: BSAM-PA-02
summary: Capacidades de entrada y salida
description: Asegúrate de que tu dispositivo Bluetooth declare las máximas capacidades de I/O para mejorar la seguridad.
parent: Emparejamiento
grand_parent: Controles
nav_order: 2
lang: es
page_id: cont_pair_02
permalink: controles/capacidades-entrada-salida-bluetooth/
tags:
- BR/EDR
- BLE
---

Al comenzar el emparejamiento, ambos dispositivos intercambian la información necesaria para establecer una clave compartida que asegure futuras conexiones. Entre los datos intercambiados se encuentran las capacidades de entrada y salida (IO capabilities), que indican si un dispositivo puede mostrar información en pantalla, aceptar pulsaciones simples o permitir al usuario introducir un código mediante un teclado. Estas capacidades ayudan a determinar el método de autenticación más seguro y adecuado, según lo que cada dispositivo puede hacer.

Los dispositivos con mayores capacidades de entrada y salida deben utilizarlas para habilitar modos de emparejamiento más seguros, ya que parte de la seguridad se basa en la verificación por parte del usuario para confirmar que los dispositivos que se emparejan son efectivamente los previstos. Esta verificación, normalmente mediante confirmación de PIN, protege contra ataques MitM.

Los dispositivos con capacidades IO limitadas no pueden realizar esta verificación y pueden quedar restringidos a métodos inseguros como "Just Works", que no requiere interacción del usuario ni protege contra MitM. Por ello, los dispositivos deben declarar las capacidades IO más altas disponibles y evitar declarar ninguna.

## Descripción del proceso

La información sobre capacidades de entrada y salida soportadas se intercambia entre los dispositivos al realizar el emparejamiento, por lo que esta información se debe capturar durante este proceso.

Para ello, se puede iniciar un emparejamiento con una herramienta de usuario, como bluetoothctl, y capturar la conexión con Wireshark. Otro método consiste en utilizar una librería de envío de paquetes Bluetooth para simular el inicio de un emparejamiento y terminar la conexión cuando se reciba el mensaje `IO Capabilities Response`, que contiene la información necesaria.

El mensaje contiene el campo `IO Capability` con uno de los siguientes valores:

| Valor | Nombre          | Descripción                                                                                     |
|:------|:----------------|:-------------------------------------------------------------------------------------------------
| 0x00  | DisplayOnly     | Es capaz de mostrar información en una pantalla, pero no puede recibir entradas​.                |
| 0x01  | DisplayYesNo    | Puede mostrar información y/o preguntas de sí/no, permitiendo una interacción limitada​​.         |
| 0x02  | KeyboardOnly    | Puede recibir entrada a través de un teclado (Ej: introducir un PIN durante el emparejamiento)​. |
| 0x03  | NoInputNoOutput | No tiene medios para mostrar información ni recibir entrada de por ejemplo teclados o botones.  |
| 0x04  | KeyboardDisplay | Puede recibir entrada a través de un teclado y es capaz de mostrar información en una pantalla  |

Los valores se utilizan para determinar qué método de emparejamiento se utilizará con el dispositivo, por lo que debe ser el valor con mayores capacidades posibles. Se debe comprobar que las capacidades expuestas por software son las más altas que el hardware del dispositivo permite.


## Recursos relacionados

Para descubrir los valores de "IO Capability" y otros relacionados, se puede forzar un emparejamiento y capturar los resultados:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07' %}


## Caso de ejemplo
Usaremos Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode Wireshark) para realizar la captura de paquetes para el análisis del emparejamiento entre un portátil y los auriculares Bluetooth Samsung Galaxy Buds 2.

En Bluetooth Classic podemos encontrar el campo _IO Capabilitiy_ en los comandos _IO Capability Request_ y _IO Capabilitiy Response_.
El siguiente ejemplo ilustra un comando _IO Capability Response_ con un campo _IO Capability_ de valor `0x03` (`NoInputNoOutput`).

![Wireshark Pairing IO Capabilities]({{ 'assets/img/bsam-pa-02_pairing_io_caps.png' | relative_url}})

En el caso de Bluetooth LE encontramos el campo _IO Capability_ en los comandos _Pairing Request_ y _Pairing Response_. 
En el ejemplo dado se muestra un comando _Pairing Request_ con unas _IO Capability_ de valor `0x03` (`NoInputNoOutput`).

![Wireshark Pairing IO Capabilities BLE]({{ '/assets/img/bsam-pa-02_pairing_io_caps_le.png' | relative_url}})

El resultado del control será _FAIL_ cuando un dispositivo no tiene el valor `0x04` (`KeyboardDisplay`).

El auditor deberá de revisar que las características declaradas sean las mismas que se encuentran presentes en el dispositivo físico.

## Referencias externas 

* Bluetooth Core V5.3, Vol. 3, Part C, Section 5.2.2.4 - IO capabilities
* Bluetooth Core V5.3, Vol. 3, Part C, Section 5.2.2.5 - Mapping of input / output capabilities to IO capability
* Bluetooth Core V5.3, Vol. 3, Part C, Section 5.2.2.6 - IO and OOB capability mapping to authentication stage 1 method
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.2 - IO capabilities
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.1 - Selecting key generation method
