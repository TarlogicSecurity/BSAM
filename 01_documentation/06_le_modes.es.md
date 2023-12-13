---
layout: default
title: Modo de conexión en Bluetooth LE
description: Listado de los modos de conexión y procedimientos entre dispositivos Bluetooth Low Energy
parent: Documentación
nav_order: 10
lang: es
page_id: doc_le_connection
permalink: documentacion/modo-conexion-bluetooth-le/
---

# {{ page.title }}

Según __Bluetooth Core V5.3, Vol. 3, Parte C, Sección 10, titulada "ASPECTOS DE SEGURIDAD - TRANSPORTE FÍSICO LE"__, los modos y procedimientos se definen de la misma manera tanto para las conexiones asíncronas _ACL_ como para las conexiones síncronas _CIS_. Esta sección tiene como objetivo establecer cómo se emparejarán los dispositivos BLE en términos de seguridad. Es importante señalar que cada modo y procedimiento viene con requisitos específicos que no se detallan aquí, por lo que será esencial consultar la sección mencionada del estándar para obtener información detallada.


## Modos de conexión

Hay disponibles 5 modos de conexión que se subdividen en niveles.

* _LE security Mode 1_:

    - _Level 1_: No Security (sin seguridad ni cifrado)
    - _Level 2_: emparejado sin autenticar con cifrado
    - _Level 3_: emparejado autenticado con cifrado
    - _Level 4_: emprajeado autenticado con _LE Secure Connection pairing_ usando una clave segura de 128-bits 

* _LE Security Mode 2_:

    - _Level 1_: emparejado sin autenticar con firma de datos
    - _Level 2_: emprajeado autenticado con firma de datos

* _Mixed security Mode_:

    - Se trata de las configuraciones de seguridad en función de cada tipo de modo de seguridad y configuración admitida en cada dispositivo

* _Secure Connections Only Mode_:

   - Sólo se admitirán conexiones seguras y autenticadas

* _LE Security Mode 3_:

    - _Level 1_: No Security
    - _Level 2_: uso de un código de emisión no autenticado
    - _Level 3_: uso de un código de emisión autenticado

## Procedimientos

Los procedimientos no son excluyentes de ningún modo concreto pero es necesario un cierto procedimiento para realizar acceder a un modo de seguridad en Bluetooth LE.

* _Authentication procedure_

    - El procedimiento de autenticación cubre el _LE Security Mode 1_ y se efectuará únicamente tras haber sido establecida la conexión.
    - La autenticación en _LE Security Mode 1_ se logra habilitando la encriptación.

* _Data Signing_

    - El firmado de datos se utiliza para transferir datos autenticados entre dos dispositivos en una comunicación no cifrada.
    - Cuando se solicita el _LE Security Mode 2_ se deberán de firmar los datos de la conxión

* _Authorization procedure_

    - Un servicio puede requerir autorización antes de permitir el acceso, que es una confirmación por parte del usuario para continuar con el procedimiento.
    - La autenticación no necesariamente proporciona autorización. La autorización puede ser concedida mediante la confirmación del usuario después de una autenticación exitosa.

* _Encryption procedure_

    - Un dispositivo Central cifra la conexión usando _Encryption Session Setup_ para proporcionar integridad y confidencialidad.
    - Un dispositivo Periférico puedría cifrar la conexión con el comando _Security Request_
