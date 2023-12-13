---
layout: default
title: 'Capa de enlace de datos: L2CAP'
description: L2CAP es un protocolo de capa de enlace de datos de Bluetooth para multiplexar, segmentar y abstraer datos
parent: Documentación
nav_order: 4
lang: es
page_id: doc_data_link_layer
permalink: documentacion/data-link-layer/
---

# {{ page.title }}

El Protocolo de Control de Enlace Lógico y Adaptación (L2CAP) reside en la capa de enlace de datos. L2CAP proporciona servicios de datos orientados a la conexión y sin conexión a los protocolos de la capa superior.

El objetivo de este protocolo es añadir capacidad de multiplexación de protocolos, operación de segmentación y reensamblaje y abstracciones de grupo.

Es importante tener en cuenta que la especificación L2CAP sólo está definida para transportes lógicos ACL y no está prevista la compatibilidad con SCO.

![Protocolos de la capa de enlace de datos Bluetooth]({{ '/assets/img/doc_data_link_layer.png' | relative_url}})

## Estructura de paquetes L2CAP

La estructura general de los paquetes L2CAP se define como una trama _basic frame_ or _b-frame_.

![Estructura de la trama b- L2CAP]({{ '/assets/img/doc_l2cap_bframe.png' | relative_url}})

L2CAP proporciona diferentes canales (CID - Identificador de canal) donde se pueden multiplexar diferentes protocolos o servicios. Algunos de los CID más comunes son 0x0001 para el canal de señalización, `0x0002` para el canal sin conexión o `0x0006` para el SMP (Security Manager Protocol).

En particular, el canal sin conexión define una estructura de paquete extendida llamada _group frame_ o _g-frame_ que proporciona una segunda capa para la multiplexación de servicios llamada multiplexor de protocolo/servicio ( _Protocol/Service Multiplexer_ )  o _PSM_.

![Estructura de la g-frame L2CAP]({{ '/assets/img/doc_l2cap_gframe.png' | relative_url}})


Los servicios PSM se clasifican en dos rangos. El primero es asignado por el Bluetooth SIG, mientras que el segundo es libre de ser asignado por los proveedores. Los servicios PSM asignados por el Bluetooth SIG se pueden encontrar en el documento de números asignados por Bluetooth en la Sección 2.5. Los servicios PSM se describen con más detalle en la especificación Bluetooth Core V5.3 Vol 3 Parte A Sección 4.2.