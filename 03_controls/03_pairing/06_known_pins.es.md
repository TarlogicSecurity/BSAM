---
layout: control
title: BSAM-PA-06
summary: Código PIN conocidos
description: Comprueba que tu dispositivo Bluetooth no utiliza un código PIN fijo. Es importante para evitar que un atacante pueda descifrar el código PIN y tomar el control del proceso de emparejamiento
parent: Emparejamiento
grand_parent: Controles
nav_order: 6
lang: es
page_id: cont_pair_06
permalink: controles/codigo-pin-conocido-bluetooth/
tags:
- BR/EDR
- BLE
---

Uno de los métodos de emparejamiento utilizados para generar la clave compartida requiere compartir un código PIN aleatorio, que el usuario debe introducir. Dado que la clave depende de este PIN, es esencial que no sea predecible.

La aleatoriedad de las semillas empleadas en procedimientos criptográficos es crítica para asegurar las claves Bluetooth. En algunos dispositivos, el PIN puede ser un número fijo de 4 dígitos. Cuando se emplean parámetros fijos como PIN estáticos, la entropía disminuye y se reduce la seguridad, comprometiendo la fortaleza de las claves derivadas.


## Descripción del proceso

Para comprobar la validez del control se deben realizar múltiples emparejamientos para recopilar datos que permitan detectar que los números PIN son fijos.

La tara a realizar consiste en comprobar que no se cumple:

* Número PIN constante: se usa el mismo número PIN para cada emparejamiento, lo que no es seguro para garantizar la protección contra MITM.

El control se cumple si no se repiten los números PIN recibidos.

## Caso de ejemplo

Para evaluar la seguridad de la generación de PIN del sistema se inician múltiples procesos de emparejamiento con un teléfono móvil capturándose mediante la técnica {% include res_link.md res='BSAM-RES-05' %}. Durante este proceso se evidencia que el dispositivo siempre indica el mismo número pin con el valor `0000`.

Por lo tanto, este dispositivo no supera el control ya que la generación de códigos PIN es predecible por lo que es posible hacerse con el control de un proceso de emparejamiento. 

## Recursos relacionados

Para comprobar este control, pueden ser útiles los siguientes recursos:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-07' %}