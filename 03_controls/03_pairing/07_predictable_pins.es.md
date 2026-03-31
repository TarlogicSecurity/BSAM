---
layout: control
title: BSAM-PA-07
summary: Código PIN bluetooth predecible
description: Comprueba que tu dispositivo Bluetooth no genere códigos PIN predecibles. Es importante para evitar que un atacante pueda descifrar el código PIN y tomar el control del proceso de emparejamiento
parent: Emparejamiento
grand_parent: Controles
nav_order: 7
lang: es
page_id: cont_pair_07
permalink: controles/codigo-pin-predecible-bluetooth/
tags:
- BR/EDR
- BLE
---

Otro método de emparejamiento utiliza un PIN aleatorio generado por el dispositivo, que el usuario introduce en el segundo dispositivo. Como la clave depende de este PIN, su imprevisibilidad es fundamental.

La fuente usada para generar PIN debe proporcionar suficiente entropía. Deben evitarse generadores pseudo aleatorios rudimentarios como contadores incrementales o fragmentos de timestamps. En su lugar, deben emplearse generadores criptográficamente seguros para garantizar que los atacantes no puedan predecir el PIN.

## Descripción del proceso

Para comprobar la validez del control se deben realizar múltiples emparejamientos para recopilar datos que permitan detectar patrones en los números PIN generados.

Algunos patrones fácilmente identificables son los siguientes:
* Números PIN consecutivos: el código PIN se incrementa tras cada emparejamiento.
* Números PIN repetidos: uso de generadores pseudoaleatorios con un número limitado de resultados y que, llegado a cierto umbral, comienza a repetir valores.

Es recomendable el uso de herramientas estadísticas o suites de tests para fuentes criptográficas. Algunas de las herramientas y tests más completos se listan a continuación:
* DieHard statistical test
* NIST Cryptographic Algorithm Validation Program Testing: Random Number Generators

El control se cumple si no se puede determinar un patrón en los números PIN recibidos y si las fuentes de entropía para la generación de códigos PIN es fuerte frente a los tests mencionados.


## Recursos relacionados

Para comprobar este control, pueden ser útiles los siguientes recursos:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-07' %}


## Caso de ejemplo

Se evalúa la seguridad de las comunicaciones Bluetooth de una unidad radio/infotainment de un coche. Tras verificar el control {% include ctl_link.md ctl='BSAM-PA-02' %}, se concluye que el dispositivo analizado expone unas capacidades físicas acorde al hardware disponible, en este caso un display y un input YES/NO dado que no existe un teclado completo. Debido a esas capacidades de entrada salida, el método de autenticación más seguro implica la comparación de PIN generado mediante métodos automáticos basados en fuentes de entropía seguras.

Para evaluar la seguridad de la generación de PIN del sistema se inician múltiples procesos de emparejamiento con un teléfono móvil capturándolos mediante la técnica {% include res_link.md res='BSAM-RES-05' %}. Durante este proceso se evidencia que el dispositivo siempre indica un pin incremental en una unidad con el valor `0000`, `0001`, `0002` y así sucesivamente.

Por lo tanto, este dispositivo no supera el control ya que la generación de códigos PIN es predecible por lo que es posible hacerse con el control de un proceso de emparejamiento. 
