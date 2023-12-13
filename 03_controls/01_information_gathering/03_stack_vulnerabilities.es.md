---
layout: control
title: BSAM-IG-03
summary: Vulnerabilidades de la pila Bluetooth del host
description: Cómo identificar vulnerabilidades en la pila de Bluetooth del host para garantizar la seguridad de tu dispositivo.
parent: Recopilación de información
grand_parent: Controles
nav_order: 3
lang: es
page_id: cont_info_03
permalink: controles/vulnerabilidades-pila-bluetooth-host/
tags:
- BR/EDR
- BLE
---


Los protocolos de las capas altas del estándar están implementados en la pila del host (BlueZ, drivers de Bluetooth en Windows…). Si se han introducido vulnerabilidades en esta pila, es posible que las vulnerabilidades afecten al dispositivo al completo, por lo que pueden ser críticas para su seguridad.

Al igual que con las vulnerabilidades en el controlador, se ha de verificar qué vulnerabilidades de pila afectan al dispositivo analizado.

## Descripción del proceso

En primer lugar, se debe identificar cuál es la pila del host en uso. La identificación de la pila del host depende del tipo de dispositivo:

* En dispositivos con sistema operativo, la pila de Bluetooth suele estar implementada como un driver. En el caso de sistemas Unix, este suele ser BlueZ, mientras que en Windows se utiliza Bluetooth Driver Stack.

* En dispositivos con firmware propio, la pila de Bluetooth puede ser una implementación propia o tratarse de una biblioteca pública.

La implementación de la pila de Bluetooth utilizada y su versión puede no ser conocida por el cliente. Tampoco suele exponerse de manera directa, por lo que puede ser necesario un trabajo de ingeniería inversa sobre el firmware del dispositivo para identificar qué implementación de la pila de Bluetooth se utiliza.

La enumeración de vulnerabilidades generalmente puede realizarse con búsquedas en bases de datos de vulnerabilidades o en motores de búsqueda de propósito general.

## Recursos relacionados

Para la enumeración de vulnerabilidades pueden ser de interés los siguientes recursos:

{% include res_table.md resources='BSAM-RES-03' %}

## Caso de ejemplo

Durante el análisis de una unidad multimedia de un vehículo, se observa que el sistema operativo ejecutado es Android. Interactuando con el dispositivo, en la sección de `Ajustes > Información del dispositivo` se identifica que la versión de Android ejecutada es Android 8.0.

Desde Android 4.2 en adelante, el sistema operativo incluye su propio stack de Bluetooth. Se realiza una búsqueda en bases de datos especializadas y se localizan los siguientes CVEs como posibles vulnerabilidades para nuestro dispositivo:

* [https://nvd.nist.gov/vuln/detail/CVE-2017-0785](https://nvd.nist.gov/vuln/detail/CVE-2017-0785)
* [https://nvd.nist.gov/vuln/detail/CVE-2017-0781](https://nvd.nist.gov/vuln/detail/CVE-2017-0781)
* [https://nvd.nist.gov/vuln/detail/CVE-2017-13262](https://nvd.nist.gov/vuln/detail/CVE-2017-13262)
* [https://nvd.nist.gov/vuln/detail/CVE-2017-13261](https://nvd.nist.gov/vuln/detail/CVE-2017-13261)
* [https://nvd.nist.gov/vuln/detail/CVE-2017-13260](https://nvd.nist.gov/vuln/detail/CVE-2017-13260)
* [https://nvd.nist.gov/vuln/detail/CVE-2017-13258](https://nvd.nist.gov/vuln/detail/CVE-2017-13258)

Para los CVEs enumerados, se encuentran los siguientes exploits:

* [https://www.exploit-db.com/exploits/44555](https://www.exploit-db.com/exploits/44555)
* [https://www.exploit-db.com/exploits/44554](https://www.exploit-db.com/exploits/44554)
* [https://www.exploit-db.com/exploits/44327](https://www.exploit-db.com/exploits/44327)
* [https://www.exploit-db.com/exploits/44326](https://www.exploit-db.com/exploits/44326)

Tras la ejecución de los exploits, se verifica que algunos de ellos siguen teniendo aplicación sobre el dispositivo. Se emite la recomendación de actualizar el dispositivo a una versión de Android más reciente.
