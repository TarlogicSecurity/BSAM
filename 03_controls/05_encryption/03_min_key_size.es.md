---
layout: control
title: BSAM-EN-03
summary: Tamaño mínimo de clave de cifrado
description: Comprueba que tu dispositivo Bluetooth utilice un tamaño de clave de cifrado mínimo. Es importante para evitar que un atacante pueda descifrar tus comunicaciones mediante un ataque de fuerza bruta.
parent: Cifrado
grand_parent: Controles
nav_order: 3
lang: es
page_id: cont_enc_03
permalink: controles/tamano-minimo-clave-cifrado-bluetooth/
tags:
- BR/EDR
- BLE
---

Las claves temporales de cifrado de una conexión de Bluetooth pueden tener distintos niveles de entropía. Para decidir como de segura es una clave temporal de cifrado de una conexión Bluetooth, durante el inicio del proceso de cifrado se negocia el número de bytes de entropía que tendrá.

Claves pequeñas, por debajo de 7 bytes, pueden ser descubiertas mediante ataques de fuerza bruta, vulnerando la confidencialidad de las comunicaciones.


## Descripción del proceso

Para la verificación del tamaño mínimo de clave que acepta el dispositivo ha de realizarse un intento de conexión cifrada imponiendo una limitación de tamaño máximo de clave. Existe una [prueba de concepto del ataque KNOB](https://github.com/francozappa/knob) que permite esto.

De esta manera, cuando se inicie el proceso de negociación de entropía de clave, intentaremos negociar 1 byte de entropía a lo que el dispositivo deberá responder con su tamaño de clave mínimo.

En algunos casos, será necesario realizar múltiples intentos de conexión con distintos tamaños máximos de clave para averiguar el tamaño mínimo de clave que permite ya en vez de negociar un tamaño de clave, cierra la conexión.

Se recomienda forzar el uso del máximo tamaño de clave soportado por el estándar Bluetooth: 16 bytes.

Este control se considerará satisfactorio si el tamaño mínimo de clave de cifrado es de al menos 7 bytes.

## Recursos relacionados

Para comprobar este control, pueden ser útiles los siguientes recursos:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07,BSAM-RES-09' %}

## Caso de ejemplo

Se va a comprobar la longitud de la clave de cifrado del emparejamiento entre un ordenador y unos auriculares Bluetooth. Usaremos Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) para realizar la captura de paquetes para su análisis. La conexión se solicita a través de la aplicación de gestión de dispositivos Bluetooth integrada en el sistema operativo.

En la captura de Wireshark, se identifica el proceso de emparejamiento con un comando _Pairing Request_, seguido por el comando _Pairing Response_, y se confirma el emparejamiento con el comando _Pairing Confirm_.

![Wireshark Pairing Process]({{ '/assets/img/bsam-en-02_pairing_process.png' | relative_url}})

En los comandos _Pairing Request_ y _Pairing Response_ hay un campo llamado _Maximum Encryption Key Size_, este indica la longitud máxima de la clave de cifrado en bytes. Este campo debería de tener una longitud de entre 7 y 16 bytes según el estándar de Bluetooth.

En el comando _Pairing Request_ del portátil se comparte en el campo _Maximum Encryption Key Size_ con un valor `16` bytes.

![Wireshark Pairing Request]({{ '/assets/img/bsam-en-02_pairing_request.png' | relative_url}})

En el comando _Pairing Request_ de los auriculares se comparte en el campo _Maximum Encryption Key Size_ un valor de `16` bytes.

![Wireshark Pairing Response]({{ '/assets/img/bsam-en-02_pairing_response.png' | relative_url}})

Los comandos _Pairing Request_ y _Secure Connections_ deberían tener siempre valor de _Maximum Encryption Key Size_ superior a 6 para pasar el control. Aún así se recomienda no utilizar valores inferiores a 16.

Si algún dispositivo solicita una longitud de clave inferior a 7 el otro dispositivo que participa en el emparejamiento deberá de cancelar la conexión.

El resultado del control será _FAIL_ si alguno de los dos dispositivos presenta un campo _Maximum Encryption Key Size_ con valor inferior a 7, si esto ocurriera y se establece la conexión entre ambos dispositivos no superarán este control.