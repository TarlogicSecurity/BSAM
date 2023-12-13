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

Uno de los métodos utilizados durante el emparejamiento para generar la clave compartida entre los dispositivos consiste en compartir un código PIN generado aleatoriamente por uno de los dispositivos. A partir de este número PIN, que el usuario debe introducir, se genera la clave de enlace, por lo que es fundamental evitar que el número generado sea predecible. En algunos casos, como las radios de los automóviles o en los Apple TV, el PIN puede ser un número fijo de 4 dígitos.

La aleatoriedad de las semillas utilizadas en los procedimientos criptográficos para generar claves de enlace en Bluetooth es de suma importancia. Cuando se emplea un parámetro fijo, como el código PIN, se disminuye la entropía de esta entrada, lo que conlleva a una reducción en el nivel de seguridad y, por ende, debilita la protección de las claves de enlace del dispositivo. Esto se debe a que las claves de enlace pueden ser potencialmente derivadas a partir de este código PIN fijo, lo que compromete la seguridad del sistema.


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