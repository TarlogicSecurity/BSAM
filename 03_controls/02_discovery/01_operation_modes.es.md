---
layout: control
title: BSAM-DI-01
summary: Modos de operación (BR/EDR y BLE)
description: Comprueba que los modos de operación Bluetooth de tu dispositivo son los necesarios. Es importante para reducir la superficie de ataque y mejorar el consumo de batería.
parent: Descubrimiento
grand_parent: Controles
nav_order: 1
lang: es
page_id: cont_disco_01
permalink: controles/modo-operacion-bluetooth/
tags:
- BR/EDR
- BLE
---


Bluetooth dispone múltiples modos de transporte/operación y un dispositivo puede soportar uno o varios al mismo tiempo. Entre los más comunes destacan BR/EDR (conocido también como Bluetooth Classic) y Bluetooth Low Energy (BLE). AMP es menos común.

Habilitar modos de operación innecesarios aumenta la superficie de ataque al dispositivo. Limitar las capacidades expuestas a lo estrictamente necesario es una buena práctica de seguridad, por lo que es conveniente comprobar que los modos de operación disponibles en el dispositivo coinciden con los requeridos para su correcto funcionamiento.


## Descripción del proceso

Para verificar que los modos de operación son adecuados al uso que se plantea para el dispositivo, es necesario descubrir que modos están habilitados.
Esto se puede realizar de múltiples maneras:
 
* Usando herramientas para la gestión de dispositivos Bluetooth. Con `bluetoothctl` en Linux, se puede iniciar un procedimiento de descubrimiento, primero para BR/EDR y luego para BLE. Esto permite descubrir si un dispositivo se anuncia en los dos modos o en uno de ellos. 
* Mediante frameworks y herramientas de más bajo nivel como puede ser `Scapy` mediante el envío de solicitudes de encuesta (`HCI Inquiry`) y la captura de respuestas para el caso de BR/EDR y para la captura de anuncios (`Advertisments`) para LE.
* Mediante la captura y el análisis del tráfico de una solicitud de características LMP BR/EDR donde se pueden consultar los campos `BR/EDR Not supported`, `LE supported` y `Simultaneous LE and BR/EDR to Same Device Capable` que indican los modos de operación habilitados en este dispositivo.

Si el dispositivo tiene habilitado más de un modo de operación de Bluetooth se deben evaluar los motivos por los que los implementa. Para ello se puede emplear la siguiente tabla que enumera algunas de las restricciones para cada modo de operación.

| Modo              | BR/EDR            | LE                                | AMP                                                                                                       |
|:------------------|:------------------|:----------------------------------|:----------------------------------------------------------------------------------------------------------|
| Velocidad         | Hasta 3 Mb/s[^1]  | Hasta 2 Mb/s[^1]                  | Dependiente del PHY/MAC seleccionado                                                                      |
| Consumo           | Consumo 1W[^2]    | Consumo desde 0.01W a 0.5W[^2]    | Dependiente del PHY/MAC seleccionado                                                                      |
| Topología         | Punto a punto     | Punto a punto, Broadcast, Mesh    | Dependiente del PHY/MAC seleccionado                                                                      |
| Usos contemplados | Audio             | Audio, localización, red          | Aplicaciones especiales donde es necesario un canal físico personalizado o anchos de banda más elevados   |


Se debe evaluar si los usos del dispositivo requieren de cada uno de los modos listados o si es posible que el dispositivo renuncie a alguno de ellos sin un impacto en la funcionalidad.


## Recursos relacionados

Los recursos que pueden ser de utilidad para el reconocimiento de los modos de funcionamiento habilitados:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-08' %}


## Caso de ejemplo

Se analizan unos auriculares inalámbricos. Para ello se hace una captura con `Acrylic Bluetooth LE Analyzer` mientras el ordenador  realiza el descubrimiento de dispositivos.

![Beacon LE con datos acerca de los modos de operación soportados]({{ '/assets/img/bsam-di-01_adv_modes.png' | relative_url}})

En la captura se puede apreciar un paquete de anuncio de Bluetooth LE procedente del dispositivo auditado. Este paquete de anuncio contiene información acerca de los modos de operación soportados en su campo `Advertising Data > Flags`. En estos campos se especifica que existe soporte para la operación simultánea en BR/EDR y LE (Nótese la doble negación en `BR/EDR not supported: false`).

Este dispositivo por tanto puede operar en modo BR/EDR y LE simultáneamente.

Consultando las especificaciones del dispositivo se observa que su uso previsto es la reproducción de audio. Esta funcionalidad está contemplada tanto en BR/EDR como LE. Consultando las características y las especificaciones técnicas se puede encontrar que los auriculares soportan los códecs SBC, AAC y Samsung Scalable Codec. Para cualquiera de los 3 codecs el mayor bitrate soportado es 512Kbps. Descartando la posible compresión del códec utilizado y tomando como válido este valor de bitrate, se determina tanto BR/EDR como LE deberían poder soportar este ancho de banda. Finalmente, los auriculares están pensados para una conexión punto a punto y no para conexiones multipunto por lo que, de nuevo, ambos modos permiten esta funcionalidad.

Del estudio anterior se obtiene la conclusión de que no es necesario habilitar ambos modos simultáneamente. Con habilitar uno de ellos sería suficiente para lograr toda la funcionalidad actual del dispositivo. Por este motivo, estando ambos habilitados simultáneamente se incrementa la superficie de ataque del dispositivo. En este caso particular se recomienda deshabilitar el modo BR/EDR ya que ofrece peor consumo de batería en comparación con el modo LE.

------

[^1]: Valor dependiente de la versión de Bluetooth usada.
[^2]: Valor de consumo para un caso de uso estándar usado como referencia. Este valor depende del escenario, caso de uso y condiciones en las que se desarrolla la transmisión de datos.
