---
layout: control
title: BSAM-PA-09
summary: Longitud mínima de código PIN
description: Los dispositivos Bluetooth deben evitar eliminar claves de enlace compartidas al recibir nuevas peticiones de emparejamiento. Esto puede ser aprovechado por atacantes para secuestrar las comunicaciones
parent: Emparejamiento
grand_parent: Controles
nav_order: 9
lang: es
page_id: cont_pair_09
permalink: controles/longitud-codigo-pin-bluetooth/
tags:
- BR/EDR
- BLE
---

En el proceso de emparejamiento con Passkey (o número PIN) el dispositivo solicita al usuario o genera automáticamente un número PIN que debe introducirse en ambos extremos emparejados. Además de autenticar que ambos dispositivos pertenecen al mismo usuario, el número PIN se utiliza para generar la clave de enlace que autenticará a los dispositivos en futuras conexiones, por lo que la longitud debe ser adecuada para evitar ataques de fuerza bruta.

Los números PIN de menos de 8 dígitos en BR/EDR o 6 dígitos en BLE no son recomendables por su debilidad ante ataques de fuerza bruta, por lo que, tanto si el dispositivo solicita el PIN al usuario como si lo genera automáticamente, el dispositivo debe verificar que la longitud no es inferior a 8 dígitos en BR/EDR e igual a 6 dígitos en BLE.


## Descripción del proceso

Para comprobar que no se admiten números PIN cortos, se puede probar a realizar un emparejamiento con otro dispositivo. Al introducir un número inferior a 8 dígitos en BR/EDR o 6 dígitos en BLE el dispositivo debe rechazarlo. Si el dispositivo genera el PIN, este debe ser igual o superior a 8 dígitos en BR/EDR e igual a 6 dígitos en BLE.

En caso de que el dispositivo no sea el iniciador, debe rechazar intentos de emparejamiento que especifiquen un número PIN demasiado corto.

Estas pruebas se pueden realizar con dispositivos Bluetooth de prueba que admitan autenticación con PIN o Passkey, o utilizando un PC con las herramientas propias del sistema operativo.

## Recursos relacionados

Algunos recursos relacionados con este control son los siguientes:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-07,BSAM-RES-09' %}

## Caso de ejemplo

Se evalúa la seguridad de las comunicaciones Bluetooth de una unidad radio/infotainment de un coche. Tras verificar los controles {% include ctl_link.md ctl='BSAM-PA-02' %}, {% include ctl_link.md ctl='BSAM-PA-06' %} y {% include ctl_link.md ctl='BSAM-PA-07' %}, se concluye que el dispositivo analizado expone unas capacidades físicas acorde al hardware disponible, en este caso un display y un input YES/NO dado que no existe un teclado completo. Debido a esas capacidades de entrada salida, el método de autenticación más seguro implica la comparación de PIN generado mediante métodos automáticos basados en fuentes de entropía seguras.

Para evaluar la seguridad de la generación de PIN este debe de tener la longitud mostrada de 6 dígitos. Esto es crucial porque los valores no suministrados internamente son completados hasta los 6 dígitos por la _toolbox_ de Bluetooth con __00__  en el lado izquierdo del Pin Code ingresado. Disminuyendo la variabilidad del campo _Random Value_ del comando _Pairing Random_.

Esto facilita el proceso de regeneración de las contraseñas de enlace permitiendo ataques del tipo __store now, decript later__.

El resultado del control será _FAIL_ si el Pin Code generado no tiene 6 dígitos.