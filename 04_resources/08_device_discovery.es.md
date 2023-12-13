---
layout: default
title: BSAM-RES-08
summary: Descubrimiento de dispositivos
description: El descubrimiento de dispositivos Bluetooth es el proceso de encontrar dispositivos Bluetooth en el área de alcance de otro dispositivo Bluetooth
parent: Recursos
nav_order: 7
lang: es
page_id: BSAM_resources_08
permalink: recursos/device-discovery/
---

# Descubrimiento de dispositivos Bluetooth

El descubrimiento de dispositivos se trata de procedimientos usados para enumerar dispositivos Bluetooth en el rango de comunicaciones de otro dispositivo Bluetooth. Cabe destacar que estos procedimientos son distintos en función de las variantes de Bluetooth usadas.


## Bluetooth LE

El proceso de descubrimiento en Bluetooth LE puede ser totalmente pasivo. Consiste en escuchar en los 3 canales de anuncio a la espera de paquetes de anuncio que provienen de otros dispositivos.

Es posible que muchas herramientas envíen paquetes para completar la información recibida en los paquetes de anuncio. Un ejemplo de solicitud de datos común es la solicitud de nombre tras descubrir un dispositivo en un paquete de anuncio que no expone este dato.


## Bluetooth BR/EDR

En Bluetooth BR/EDR el proceso de descubrimiento es activo. Se ha de enviar una petición de descubrimiento a la que los dispositivos responden. Estas peticiones de descubrimiento pueden solicitar más o menos datos hasta incluir datos opcionales de `RSSI` o datos del fabricante.

Muchas herramientas que realizan descubrimiento en Bluetooth BR/EDR envían peticiones de solicitud de nombre para completar el proceso de descubrimiento.

A bajo nivel, iniciar un descubrimiento consiste en notificar al controlador que se desea realizar un `Inquiry scan` y/o un `Page scan`. Esto se realiza mediante el comando `HCI Write Scan Enable Command` descrito en Bluetooth Core V5.3, Vol. 4, Part E, Section 7.3.18.


## Herramientas de descubrimiento Bluetooth

Prácticamente cualquier dispositivo con soporte para comunicaciones Bluetooth tiene alguna herramienta que permite el descubrimiento de dispositivos Bluetooth por ser un proceso básico para establecer una conexión. Sin embargo, es importante saber que no todas las herramientas solicitan la misma información y siguiendo el mismo procedimiento.

Con el objetivo de conocer qué procedimiento se está usando y para poder extraer la máxima cantidad de información posible se recomienda capturar los procesos de descubrimiento usando la técnica {% include res_link.md res='BSAM-RES-05' %}. Esto permitirá el posterior análisis con herramientas especializadas que muestren todos los datos intercambiados y no solo nombre y MAC como en muchos casos.

Para iniciar los procesos de descubrimiento pueden usarse algunas de las siguientes herramientas:

* Pantalla de ajustes de Bluetooth de Android
* Pantalla de ajustes de Bluetooth de iOS
* Pantalla de ajustes de Bluetooth de Windows
* Pantalla de ajustes de Bluetooth de cualquier distribución de Linux
* bluetoothctl
* Scapy
* [Acrylic Bluetooth LE Analyzer](https://www.acrylicwifi.com/bluetooth-analyzer/)


## Referencias externas

* Bluetooth Core V5.3, Vol. 4, Part E, Section 7.3.18 - Write Scan Enable command
