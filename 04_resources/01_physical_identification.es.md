---
layout: default
title: BSAM-RES-01
summary: Identificación física del controlador
description: Identifica el controlador Bluetooth de tu dispositivo desmontándolo o buscando información sobre él en línea.
parent: Recursos
nav_order: 0
lang: es
page_id: BSAM_resources_01
permalink: recursos/identificar-controlador-bluetooth-fisicamente/
---

# {{ page.summary }}

En ocasiones, es posible identificar el controlador de Bluetooth de un dispositivo desmontando el dispositivo y analizando las placas de circuitos impresos del mismo.

Para identificar el controlador de Bluetooth se pueden seguir diversas estrategias.

Una posible estrategia es tratar de localizar antenas externas o en la propia PCB. Los chips Bluetooth y, posiblemente otros chips de comunicación se encontrarán físicamente cerca de estas antenas. Un ejemplo de esto se puede identificar en la siguiente imagen donde se reconoce una antena PCB a la izquierda de la foto (pista de cobre serpenteante solo conectada en un extremo). Siguiendo la pista, tras pasar por algunos componentes de tamaño muy reducido acaba conectada a un chip marcado como `CSR BC417`, un controlador Bluetooth.

![Antena PCB a la izquierda cerca de un chip bluetooth CSR BC417]({{ '/assets/img/bsam-res-01_bc417.png' | relative_url}})

Otra alternativa es la búsqueda de la serigrafía de todos los chips presentes en una placa de circuito impreso en motores de búsqueda o en la web de los distintos fabricantes de chips encontrados.

Esta búsqueda también es útil para verificar que el chip candidato, encontrado mediante la primera estrategia, realmente se trata de un controlador Bluetooth.

## Recomendaciones

- Es recomendable fotografiar o grabar en video el proceso de desmontaje. En caso de duda en el proceso de montaje se puede consultar el video del desmontaje.

- Es interesante realizar fotografías detalle de todas las placas de circuitos impresos y de los chips que contiene el dispositivo. Estas imágenes pueden ser utilizadas para otros análisis o para posterior documentación en el informe.
