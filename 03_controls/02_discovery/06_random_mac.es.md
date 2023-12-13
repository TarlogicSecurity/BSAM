---
layout: control
title: BSAM-DI-06
summary: Uso de direcciones MAC aleatorias
description: Comprobar que el dispositivo utiliza una MAC aleatoria en los mensajes de descubrimiento
parent: Descubrimiento
grand_parent: Controles
nav_order: 6
lang: es
page_id: cont_disco_06
permalink: controles/mac-aleatoria-bluetooth/
tags:
- BR/EDR
- BLE
---

Una dirección MAC (Media Access Control) es un identificador único asignado a la interfaz de red de un dispositivo para su identificación en una red de dispositivo. Estas direcciones son utilizadas en las redes de comunicaciones que usan los protocolos de la familia IEEE 802, entre el que se incluye Bluetooth (IEEE 802.15.1).

En Bluetooth, existen dos tipos de direcciones MAC:

* _MAC Pública_: Esta dirección es la identificación real y fija del dispositivo, la cual no puede modificarse.
* _MAC Aleatoria_: Esta dirección es generada de forma aleatoria que muestra el dispositivo y es posible cambiarla.

Dentro del grupo de _MACs Aleatorias_ tenemos dos subgrupos: las que permiten la conexión al dispositivo y las que no lo permiten.

Las _MAC Aleatorias_ que si permiten la conexión y acceso a un conjunto de _Servicios_ Bluetooth obligatoriamente no tienen que resolver la conexión contra la _MAC Pública_. Aunque es posible calcular la _MAC Pública_ a partir de una _MAC Aleatoria_.

La generación de _MACs Aleatorias_ durante el descubrimiento de los dispositivos es fundamental para prevenir ataques como `BlueTrust`.

__Nota__: Existe una similitud entre la _MAC real_ en Wi-Fi y la _MAC Pública_ en Bluetooth, al igual que entre la _MAC aleatoria_ en Wi-Fi y la _MAC Aleatoria_ en Bluetooth.

## Descripción del proceso

Durante el proceso de descubrimiento se debe de verificar que el dispositivo está generando una dirección aleatoria mediante la comprobación del campo  Address_Type[i] del mensaje HCI_LE_Extended_Advertising_Report. Puede tomar los valores:

| Valor | Descripción del parámetro |
|:------|:--------------------------|
| 0x00 | Dirección de Dispositivo Pública |
| 0x01 | Dirección de Dispositivo Aleatoria |
| 0x02 | Dirección Identidad Pública (corresponde a la _Dirección Privada Resoluble_) |
| 0x03 | Dirección Identidad Aleatoria (estática) (corresponde a la _Dirección Privada Resoluble_) |
| 0xFF | Dirección no suministrada (descubrimiento anónimo) |
| Todos los demás valores | Reservados para usos futuros |

Este control se considera satisfactorio cuando se verifica que el valor del campo es 1, 2, 3. 

## Example case

Abriendo Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (bvts.exe -Mode Wireshark) se pueden leer los mensajes de descubrimiento de los dispositivos cercanos, una de las etiquetas que se muestran en los paquetes de anuncio es el tipo de dirección MAC, indicando con el campo _Address Type_, en este caso con valor  `0x00` (`Public Device Address`). 

![Wireshark LE Meta Address Type]({{ '/assets/img/bsam-di-06_august.png' | relative_url}})

El tipo de dirección MAC se especifica según el estándar de Bluetooth, en términos generales, el valor obtenido estará en el rango de 0x00 a 0x03.

El resultado del control será _FAIL_ cuando su valor sea 0x00.

## Referencias Externas

* Bluetooth Core V5.3, Vol. 3, Part C, Section 10.8 Random Device Address
* Bluetooth Core V5.3, Vol. 1, Part A, Section 2.1.1.3 Security Manager Protocol
* Bluetooth Core V5.3, Vol. 4, Part H, Section 7.7.65.13 LE Extended Advertising Report event
