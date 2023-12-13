---
layout: default
title: BSAM-RES-10
summary: Gestión de claves de enlace en el host
description: La gestión de claves de enlace en el host permite controlar las respuestas del controlador a los intentos de conexión Bluetooth
parent: Recursos
nav_order: 9
lang: es
page_id: BSAM_resources_10
permalink: recursos/gestion-claves-enlace/
---

# {{ page.summary }}

El stack de software del host, entre otras muchas funciones, es responsable de la gestión de claves de enlace.

En función de la presencia de claves de enlace para la conexión con otro dispositivo, el host es capaz de controlar (a través del protocolo HCI) algunas respuestas que el controlador hace ante un intento de conexión entrante o saliente. Aunque existen alternativas a esta técnica, como la implementación de un stack de host propio, resulta más sencillo instalar o borrar claves de otro dispositivo en el host a través de APIs para ello.

Esta técnica es dependiente del stack del host en el que se intenta realizar. A continuación se detalla la técnica para el stack BlueZ de sistemas Linux.


## Claves de enlace en BlueZ

La pila de Bluetooth BlueZ expone diferentes APIs para interaccionar con ella. La management (MGMT) API, descrita en el documento [mgmt-api.txt](https://github.com/bluez/bluez/blob/master/doc/mgmt-api.txt), permite manipular las claves de enlace, entre otras tareas.

Como se indica en el documento, para utilizar la API se debe abrir un Bluetooth Management socket y seguir el formato de mensajes.

El comando [Load Link Keys](https://github.com/bluez/bluez/blob/8c452c2ec1739efe581273bacd738e5294d0ca0f/doc/mgmt-api.txt#L788), en concreto, permite modificar las claves almacenadas en la base de datos: se le pasa un array de claves para almacenar. Un array vacío elimina todas las claves de la base de datos.

Para facilitar el uso de esta API, BlueZ provee de la herramienta [btmgmt](https://github.com/bluez/bluez/blob/master/tools/btmgmt.c). Aunque no implementa todos los comandos especificados en la API, sí implementa muchos y puede servir como base. El comando Load Link Keys no se encuentra implementado en la herramienta, pero existe la opción de línea de comandos y no se requiere mucho esfuerzo para añadir la funcionalidad.
