---
layout: default
title: 'Capa de enlace de banda base: LMP y LL'
description: LMP y LL - Protocolos de capa de enlace de Bluetooth para establecer y gestionar conexiones entre controladores
parent: Documentación
nav_order: 2
lang: es
page_id: doc_baseband_link_layer
permalink: documentacion/baseband-link-layer/
---

# {{ page.title }}

Los protocolos de capa de enlace tienen como función negociar y controlar todos los aspectos de la operación de conexiones entre dos controladores Bluetooth.

Los protocolos de capa de enlace se gestionan e implementan en el controlador Bluetooth y el 'Host' no tiene conocimiento acerca de que paquetes de capa de enlace intercambia el controlador.

Los controladores Bluetooth pueden soportar distintos modos de operación como Bluetooth LE y/o BR/EDR. Dependiendo de los modos de operación soportados, se utilizarán distintos protocolos de capa de enlace.

![Protocolos de capa de enlace de Bluetooth]({{ '/assets/img/doc_baseband_link_layer.png' | relative_url}})


## LMP (Link Manager Protocol)

El Link Manager Protocol (LMP) es el protocolo de capa de enlace usado en conexiones entre controladores que se establecen en modos BR (Basic Rate) y EDR (Extended Data Rate), más comúnmente conocido como "Bluetooth Classic".

La especificación completa del Link Manager Protocol (LMP) se encuentra en el documento Bluetooth Core Specification V5.3, Vol. 2, Part C.


## LL (Link Layer)

El Link Layer (LL) es el protocolo usado para establecer y gestionar conexiones entre controllers BLE (Bluetooth Low Energy).

La especificación del Link Layer (LL) se encuentra en el documento Bluetooth Core Specification V5.3, Vol. 6, Part B.
