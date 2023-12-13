---
layout: control
title: BSAM-IG-04
summary: Vulnerabilidades conocidas en estándar Bluetooth
description: Cómo identificar las vulnerabilidades del estándar de Bluetooth para garantizar la seguridad de tu dispositivo.
parent: Recopilación de información
grand_parent: Controles
nav_order: 4
lang: es
page_id: cont_info_04
permalink: controles/vulnerabilidades-estandar-bluetooth/
tags:
- BR/EDR
- BLE
---


A lo largo de la historia del estándar de Bluetooth se han identificado problemas de seguridad y que han dado lugar a vulnerabilidades. Los dispositivos con capacidades Bluetooth raramente implementan unicamente la última versión del estándar, sino que por compatibilidad proporcionan soporte a versiones anteriores que pueden tener problemas de seguridad no resueltos.

Al igual que con las vulnerabilidades en el controlador y en la pila, se ha de verificar qué vulnerabilidades del estándar afectan al dispositivo analizado.

También ha de tenerse en cuenta que las especificaciones de Bluetooth tienen un ciclo de vida anunciado por el _Bluetooth SIG_. Es importante que la version de la especificación de Bluetooth utilizada vaya a tener soporte durante el ciclo de vida del producto ya que, de aparecer nuevas vulnerabilidades en el estándar, estas no podrán ser solucionadas.


## Descripción del proceso

En primer lugar se debe  identificarse cuál es la versión del estándar de Bluetooth soportada por el dispositivo. La versión implementada del estándar puede ser obtenida de diferentes fuentes:

* Inspección de los paquetes de una conexión en la capa `Link Layer` o capa de enlace (`LMP` o `LL`) en búsqueda de paquetes `LMP_VERSION_REQ`, `LMP_VERSION_RES` y `LL_VERSION_IND`.
* Solicitud de datos de versión al dispositivo a través del mandato HCI `Read remote version`.
* Inspección de información del dispositivo: los dispositivos como los teléfonos inteligentes suelen especificar la versión de Bluetooth entre sus características.
* Búsqueda para obtener la información pública del dispositivo: los modelos de dispositivo, en ocasiones, indican la versión de Bluetooth que implementan.
* Solicitud de datos al fabricante.

Ha de tenerse en cuenta que la información provista en Internet puede estar desactualizada y conviene contrastarlo, siempre que sea posible, con la información que proporcione el dispositivo.

La enumeración de vulnerabilidades generalmente puede realizarse con búsquedas en bases de datos de vulnerabilidades o en motores de búsqueda de propósito general.

La comprobación del _Status_ de la versión de la especificación para verificar su ciclo de vida puede consultarse comúnmente en la [lista de documentos del Bluetooth SIG](https://www.bluetooth.com/specifications/specs/?types=specs-docs&keyword=core&filter=).

Listado de las especificaciones más comunes y su estado en Mayo de 2023:

| Versión                   | Status    | Fecha de obsolescencia (Deprecation)  | Fecha de retirada (Withdwawal)    |
|:--------------------------|:----------|:--------------------------------------|:----------------------------------|
| Core Specification 4.0    | Withdrawn | 28/01/2019                            | 01/02/2022                        |
| Core Specification 4.1    | Withdrawn | 28/01/2019                            | 01/02/2023                        |
| Core Specification 4.2    | Adopted   | 01/02/2026                            | 01/02/2031                        |
| Core Specification 5.0    | Adopted   | 01/02/2027                            | 01/02/2032                        |
| Core Specification 5.1    | Adopted   | 01/02/2029                            | 01/02/2034                        |
| Core Specification 5.2    | Adopted   | 01/02/2030                            | 01/02/2035                        |
| Core Specification 5.3    | Adopted   | 01/02/2032                            | 01/02/2037                        |
| Core Specification 5.4    | Adopted   | 01/02/2033                            | 01/02/2038                        |


## Recursos relacionados

A continuación se listan recursos que pueden ser de utilidad para la obtención de la versión de Bluetooth soportada por un dispositivo bluetooth:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07' %}

Para la enumeración de vulnerabilidades pueden ser de interés los siguientes recursos:

{% include res_table.md resources='BSAM-RES-03' %}


## Caso de ejemplo

Se captura una conexión establecida entre el PC y un dispositivo usando la técnica {% include res_link.md res='BSAM-RES-05' %}. Esta captura se ha realizado con los modos de depuración de una tarjeta Cypress habilitados siguiendo la técnica {% include res_link.md res='BSAM-RES-06' %}. De este modo, se capturan paquetes de la capa `Link Layer`, en este caso de BR/EDR.

Entre los paquetes intercambiados se encuentra un paquete `LMP_VERSION_RES` como se muestra a continuación.

![Paquete LMP_VERSION_RES diseccionado en Wireshark]({{ '/assets/img/bsam-ig-04_lmp-version-res.png' | relative_url}})

El paquete diseccionado cuenta con un campo `VersNr` con valor `0x0b`. Consultando el documento Bluetooth Assigned Numbers Rev. 2022-12-20 en la sección Section 2.1 (Core specification versions), se concluye que el dispositivo analizado soporta Bluetooth v5.2. 

![Tabla de nombres de la especificación de core de Bluetooth]({{ '/assets/img/bsam-ig-04_core-ver.png' | relative_url}})

Tras una búsqueda siguiendo la técnica {% include res_link.md res='BSAM-RES-03' %} se encuentran tres vulnerabilidades en esta versión del estándar de Bluetooth:

* [BIAS](https://francozappa.github.io/publication/bias/paper.pdf)
* [KNOB](https://knobattack.com/#vulnerable)
* [BlueTrust](https://www.tarlogic.com/es/blog/bluetrust-vulnerabilidad-bluetooth/)

Para cada una de las vulnerabilidades encontradas se ejecuta una prueba y se comprueba que el dispositivo analizado no acepta claves de baja entropía por lo que se considera no vulnerable a KNOB. Sin embargo, el dispositivo permite la autenticación de dispositivos con identidades clonadas por lo que se considera vulnerable a BIAS y a BlueTrust. Se notifica este problema de seguridad al fabricante del controlador para obtener una actualización de firmware que permita solucionar las vulnerabilidades encontradas.


## Referencias externas

* Bluetooth Core V5.3, Vol. 6, Part B, Section 2.4.2.13 - LL_VERSION_IND
* Bluetooth Core V5.3, Vol. 2, Part C, Section 4.3.3 - LMP version
* Bluetooth Core V5.3, Vol. 4, Part E, Section 7.1.23 - Read Remote Version Information command
* Bluetooth Core V5.3, Vol. 4, Part E, Section 7.7.12 - Read Remote Version Information Complete event
* Bluetooth Assigned Numbers Rev. 2022-12-20, Section 2.1 - Core specification versions
