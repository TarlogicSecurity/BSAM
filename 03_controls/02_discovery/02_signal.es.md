---
layout: control
title: BSAM-DI-02
summary: Potencia de señal adecuada
description: Comprueba el alcance de la señal Bluetooth de tu dispositivo. Es importante para reducir la superficie de ataque y evitar que un atacante pueda acceder a él desde una distancia mayor a la necesaria.
parent: Descubrimiento
grand_parent: Controles
nav_order: 2
lang: es
page_id: cont_disco_02
permalink: controles/potencia-senal-bluetooth/
tags:
- BR/EDR
- BLE
---


Un dispositivo Bluetooth con una potencia de señal suficiente para la interacción a larga distancia incrementa su exposición. Por esta razón, la potencia de señal debe limitarse para cubrir únicamente el área necesaria para su uso previsto.

El rango efectivo de comunicación del dispositivo depende de las condiciones ambientales, fuentes de ruido y características de la antena, ya que estos factores afectan la relación señal-ruido (SNR) y, en consecuencia, la distancia a la que es posible establecer conexiones.

Para garantizar evaluaciones fiables, el equipo utilizado y las condiciones ambientales durante la medición deben documentarse para permitir comparaciones consistentes con pruebas futuras.

## Descripción del proceso

Para comprobar el área a la que llega la señal se deben realizar diferentes medidas:
* Medir la intensidad recibida (RSSI) a diferentes distancias del dispositivo emisor, sin obstáculos y anotando las características del dispositivo receptor.
* Realizar pruebas de descubrimiento a diferentes distancias del dispositivo emisor, sin obstáculos y anotando las características del dispositivo receptor, hasta dejar de detectarlo.
* Realizar pruebas de conexión a diferentes distancias del dispositivo emisor, sin obstáculos y anotando las características del dispositivo receptor, hasta dejar de detectarlo.

La medida de la intensidad (RSSI) es indicada en los eventos HCI por el controlador Bluetooth cuando se recibe un mensaje de un dispositivo remoto. Aparece, por ejemplo, en los mensajes de anuncio emitidos en Bluetooth LE o en las respuestas de `inquiry` en BR/EDR, por lo que se pueden utilizar los mensajes de descubrimiento para tomar la medida. También se puede solicitar el RSSI para un dispositivo con el mensaje `HCI_Read_RSSI`.

El parámetro RSSI recibido a una misma distancia varía mucho entre dispositivos receptores, fabricantes y entornos. Además, el RSSI por sí solo tampoco determina la calidad de la detección, ya que hay que tener en cuenta la relación señal/ruido.

En general, sin embargo, el límite inferior es cercano al valor -120, a partir del cual el dispositivo dejará de ser detectado. De esta forma, detectando el punto en el que se recibe un RSSI próximo o inferior a -120, se puede aproximar la distancia máxima a la que se puede interaccionar con el dispositivo.

Utilizando el mismo método de realizar un descubrimiento del dispositivo, se puede medir la distancia a partir de la cual deja de detectarse y utilizar esa distancia como aproximación.

También se pueden llevar a cabo pruebas de conexión a diferentes distancias, hasta que no sea posible y anotar la distancia máxima, tomándola como referencia.

## Recursos relacionados

Los recursos que pueden ser de utilidad para estimar la distancia máxima a la que es posible comunicarse con el dispositivo:

{% include res_table.md resources='BSAM-RES-08' %}

Otros recursos que pueden ayudar a la extracción de datos de intensidad de señal son:

{% include res_table.md resources='BSAM-RES-05,BSAM-RES-07' %}

## Caso de ejemplo

Usaremos Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) para realizar la captura de paquetes para su análisis. 

Al abrir Wireshark el ordenador comienza el escaneo de dispositivos descubribles. Los `beacons` proporcionan información sobre la disponibilidad del dispositivo que lo emite. Cuando estos datos son recibidos, el controlador agrega el campo RSSI, que representa la intensidad de la señal con la que se la recibido, en dBm.

![Wireshark LE Meta RSSI]({{ '/assets/img/bsam-di-02_rssi.png' | relative_url}})

Unos auriculares Bluetooth que emiten anuncios `beacons` Bluetooth LE y responden a mensajes de `Inquiry` BR/EDR se pueden detectarse a 15 metros de distancia, con un RSSI recibido de alrededor de -115 en el punto más alejado, utilizando como equipo de recepción el controlador Bluetooth Cypress CYW920819WCD2 con la antena integrada del propio kit de evaluación.
Con cualquier aplicación tanto de ordenador o teléfono se podrá medir la intensidad de señal con la que emite un dispositivo.

Es relevante para el estudio saber que no se pueden establecer conexiones fiables a más de 10 metros del dispositivo, por tanto, se considera un radio de 10 metros, aproximadamente, para la interacción y de 15 metros para la detección.

Considerando que se trata de unos auriculares utilizados habitualmente en espacios cerrados pequeños (dentro de una misma habitación) o con dispositivos cercanos, se considera un área adecuada a su uso.
