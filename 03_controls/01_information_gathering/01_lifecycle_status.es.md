---
layout: control
title: BSAM-IG-01
summary: Ciclo de vida del controlador Bluetooth
description: Cómo comprobar el ciclo de vida de tu controlador Bluetooth para garantizar su seguridad.
parent: Recopilación de información
grand_parent: Controles
nav_order: 1
lang: es
page_id: cont_info_01
permalink: controles/ciclo-vida-controlador-bluetooth/
tags:
- BR/EDR
- BLE
---

Un controlador Bluetooth, cuando se encuentra al final de su ciclo de vida de producto, no recibirá actualizaciones por parte del fabricante, por lo que la identificación de vulnerabilidades en su firmware o en cualquier otro componente provocará que la vulnerabilidad sea permanente en el dispositivo y no existirá una manera oficial de solucionarla.

Por ello, es importante comprobar en qué punto del ciclo de vida se encuentra el controlador Bluetooth del dispositivo, ya que esto puede indicar si será necesario cambiar el dispositivo a corto, medio o largo plazo.


## Descripción del proceso

Tras identificar el controlador bluetooth, para saber en qué punto del ciclo de vida se encuentra, será necesario consultar la documentación del fabricante.

Se puede encontrar una lista de controladores que ya han alcanzado su final de soporte en el anexo de [Controladores Bluetooth sin soporte]({% link 05_annex/01_eol_controllers.es.md %}). Alternativamente, si el dispositivo no se encuentra listado, será necesario realizar una consulta en la página del fabricante, en distintas webs de proveedores o en distintos motores de búsqueda. 
Además de los motores de búsqueda comunes y la página del fabricante, se recomienda realizar búsquedas en los siguientes proveedores/fabricantes:

| Proveedor | URL                                                                       |
|:----------|:--------------------------------------------------------------------------|
| Mouser    | <https://www.mouser.es/>                                                  |
| RS        | <https://es.rs-online.com/web/>                                           |
| Farnell   | <https://es.farnell.com/>                                                 |
| Octopart  | <https://octopart.com/>                                                   |
| Digikey   | <https://www.digikey.es/>                                                 |
| TME       | <https://www.tme.eu/>                                                     |
| Element14 | <https://sg.element14.com/>                                               |
| Avnet     | <https://www.avnet.com/>                                                  |
| Microchip | <https://www.microchip.com/product-change-notifications/>                 |
| Infineon  | <https://www.infineon.com/cms/en/product/search/discontinued-products/>   |
| Broadcom  | <https://www.broadcom.com/products/wireless/wireless-lan-bluetooth>       |
| Qualcomm  | <https://www.qualcomm.com/wireless-connectivity?function=all>             |
| uBlox     | <https://www.u-blox.com/en/short-range-radio-modules>                     |

## Recursos relacionados

Los recursos que pueden ser de utilidad para la identificación del modelo de controlador en el dispositivo se pueden encontrar en la siguiente tabla:

{% include res_table.md resources='BSAM-RES-01,BSAM-RES-02' %}

## Caso de ejemplo

Tras el desmontaje de un dispositivo Bluetooth ({% include res_link.md res='BSAM-RES-01' %}) se identifica un controlador `nRF51822`. Una búsqueda en el proveedor Mouser indica que este controlador no está recomendado para nuevos diseños debido a que el fabricante ha emitido esta recomendación por haber llegado a su final de ciclo de vida y existir productos que lo reemplazan.

![El proveedor Mouser indicando que el controlador no se recomienda para nuevos diseños]({{ '/assets/img/bsam-ig-01_nrnd.png' | relative_url}})

Este mismo fabricante muestra una nota informativa en su página web relativa a una [vulnerabilidad afectando a todos los dispositivos de la familia nRF51](https://infocenter.nordicsemi.com/pdf/in_119_v1.0.pdf). De encontrarse el dispositivo en ese momento fuera del periodo de soporte, esa vulnerabilidad no habría sido parcheada, quedando el dispositivo expuesto y vulnerable.

Esto resulta importante ya que es posible que el controlador no cuente con soporte para realizar correcciones frente a fallos de seguridad en un plazo más corto que el soporte que ha de darse al dispositivo bajo estudio.
