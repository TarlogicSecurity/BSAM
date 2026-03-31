---
layout: control
title: BSAM-AP-07
summary: Aplicaciones seguras
description: Comprueba que las apliacaciones que utilizan Bluetooth en tu dispositivo están implementados de forma segura. Es importante para evitar que un mensaje malformado pueda causar una vulnerabilidad
parent: Aplicación
grand_parent: Controles
nav_order: 7
lang: es
page_id: cont_app_07
permalink: controles/aplicaciones-bluetooth-seguras/
tags:
- BR/EDR
- BLE
---

Las aplicaciones utilizan servicios Bluetooth para intercambiar datos entre dispositivos emparejados. Debido a que todos los datos deben considerarse no confiables, las aplicaciones deben implementar controles adecuados para validar y gestionar los datos de forma segura.

## Descripción del proceso

Para comprobar que los servicios bluetooth utilizados por las aplicaciones estan implementados de manera segura se pueden utilizar las siguientes recomendaciones:

* Revisión del código fuente:  Proporciona una visión completa de la implementación y permite validar si se trata de una implementación segura.
* Fuzzing: Permite crear fujos de datos que no son los esperados por la aplicacion y ante problemas de validacion en su contenido puede evidenciar fallos en un dispositivo.
* Ingeniería inversa: En caso de no disponer del codigo fuente, mediante ingenieria inversa se podra evaluar los mecanismos que tratan los datos en busca de problemas de implementacion.

El objetivo final de este control es asegurar que ante entradas de datos aleatorios, la aplicacion es capaz de mantener su integridad y el funcionamiento.