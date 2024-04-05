---
layout: default
title: BSAM-RES-04
summary: Sniff de una conexión Bluetooth
description: El sniffing de conexiones Bluetooth consiste en capturar el tráfico de una conexión Bluetooth para analizarlo. Esta técnica puede utilizarse para detectar vulnerabilidades o realizar ataques
parent: Recursos
nav_order: 3
lang: es
page_id: BSAM_resources_04
permalink: recursos/sniff-conexion-bluetooth/
---

# {{ page.summary }}

Esta técnica consiste en capturar tráfico de una conexión Bluetooth usando hardware específico con la capacidad para interceptar paquetes de redes en las que no participan. Se trata de una técnica similar al "modo monitor" en Wi-Fi.

Esta técnica comúnmente captura paquetes de la capa de "Link Layer" de Bluetooth, es decir, paquetes "LMP" en BR/EDR y "LL" en BLE. Dependiendo de la conexión que se esté monitorizando, es posible que los paquetes capturados estén encriptados.

## Sniffers Bluetooth

En la siguiente tabla se lista algún hardware y software que permite realizar esta técnica. Es importante consultar las limitaciones de los proyectos a continuación ya que muchos no permiten una captura fiable de las comunicaciones debido a las técnicas de salto de canal usadas en Bluetooth.

| Hardware                                                                                         | Software                                                                                                           | Modos              |
| :----------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------- | :----------------- |
| Ubertooth                                                                                        | [Ubertooth tools](https://github.com/greatscottgadgets/ubertooth)                                                  | BR* / EDR* / BLE   |
| TI CC1352/CC26x2                                                                                 | [Sniffle](https://github.com/nccgroup/Sniffle)                                                                     | BLE 4.x / BLE 5    |
| nRF51822                                                                                         | [Btlejack](https://github.com/virtualabs/btlejack)                                                                 | BLE 4.x / BLE 5.x* |
| Bluefruit LE sniffer                                                                             | [Btlejack](https://github.com/virtualabs/btlejack)                                                                 | BLE 4.x / BLE 5.x* |
| Micro:Bit                                                                                        | [Btlejack](https://github.com/virtualabs/btlejack)                                                                 | BLE 4.x / BLE 5.x* |
| nRF52840                                                                                         | [nRF Sniffer](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fug_sniffer_ble%2FUG%2Fsniffer_ble%2Fintro.html) | BLE                |
| [PANalyzr](https://spanalytics.com/product/panalyzr/)                                            | -                                                                                                                  | BR / EDR / BLE     |
| [Ellisys Bluetooth Vanguard](https://www.ellisys.com/products/bv1/index.php)                     | -                                                                                                                  | BR / EDR / BLE     |
| [Ellisys Bluetooth Explorer](https://www.ellisys.com/products/bex400/index.php)                  | -                                                                                                                  | BR / EDR / BLE     |
| [TeledyneLecroy Frontline X500](https://teledynelecroy.com/protocolanalyzer/frontline-x500.aspx) | -                                                                                                                  | BR / EDR / BLE     |
| [Sena-UD100](http://www.senanetworks.com/ud100-g03.html)                                         | -                                                                                                                  | BR / EDR / BLE     |

\* Soporte limitado. Consulta el producto o proyecto para más información.
