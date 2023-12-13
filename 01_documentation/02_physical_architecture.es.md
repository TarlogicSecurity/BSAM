---
layout: default
title: Arquitectura física
description: Arquitectura física de Bluetooth - Host y Controller, dos elementos esenciales para la comunicación
parent: Documentación
nav_order: 1
lang: es
page_id: doc_physical_architecture
permalink: documentacion/physical-architecture/
---

# Arquitectura física de Bluetooth

La arquitectura física de Bluetooth está dividida en dos elementos principales: _Host_ y _Controller_.

![Representación de la arquitectura física de Bluetooth]({{ '/assets/img/doc_physical_architecture_host_controller.png' | relative_url}})


## Controlador Bluetooth

El controlador Bluetooth (_Controller_) es un _"chip"_, o conjunto de ellos, con capacidad para transmitir y recibir ondas de radio, y está encargado de realizar las tareas de más bajo nivel de las comunicaciones Bluetooth.

Un controller puede tener soporte para una o más tecnologías Bluetooth. Existen controllers que sólo soportan _Bluetooth LE_, otros que soportan _Bluetooth BR/EDR_ y otros que soportan operación combinada en ambos modos.

El controller, además de tener el hardware para convertir señales de radio en bits y viceversa, también tiene múltiples responsabilidades a la hora crear, mantener y cerrar conexiones. Las capas inferiores del estándar de Bluetooth están implementadas en el firmware de en estos controladores y estos tienen capacidad de procesamiento de paquetes sin necesidad de que estos paquetes lleguen al Host.


## Host

El término _Host_ se refiere al hardware que hace uso de un _Controller_ para poder comunicarse vía Bluetooth. Este _Host_ ha de ejecutar un software o _stack_ que provee una abstracción y permite a las aplicaciones interactuar con dispositivos Bluetooth de manera independiente del hardware del _Controller_.

El software ejecutado en el _Host_ es responsable de funciones relacionadas con los procesos de descubrimiento y emparejamiento. Algunas de estas funciones son enumerar los dispositivos cercanos, controlar si un dispositivo debe ser descubrible o no, enumerar las capacidades del _Host_ para decidir qué métodos de emparejamiento están disponibles, la intervención durante el proceso de emparejamiento para permitir al usuario confirmar o denegar esta acción o el almacenamiento de claves de emparejamiento en un lugar seguro.


## Host Controller Interface o HCI

_Host Controller Interface_ o _HCI_ se refiere al protocolo utilizado para comunicar un _Host_ con un _Controller_. El protocolo _HCI_ suele delimitar la capacidad que un _Host_ tiene para modificar el comportamiento de un _Controller_. Superar esta barrera implica la modificación del _Controller_ o la modificación del firmware del _Controller_.
