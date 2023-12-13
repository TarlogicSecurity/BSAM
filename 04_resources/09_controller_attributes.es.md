---
layout: default
title: BSAM-RES-09
summary: Cambiar los atributos de un controlador
description: Cambiar los atributos de un controlador Bluetooth puede permitir hacerse pasar por otro dispositivo o simular diferentes escenarios
parent: Recursos
nav_order: 8
lang: es
page_id: BSAM_resources_09
permalink: recursos/cambiar-atributos-controlador/
---

# {{ page.summary }}

Muchas de las funciones y procedimientos del estándar Bluetooth dependen de parámetros específicos de cada dispositivo, como, por ejemplo: el nombre, la dirección, las capacidades de entrada y salida (si tiene pantalla o teclado), la versión del estándar Bluetooth que soporta, la versión del firmware del controlador, etc. A estos parámetros se los denomina, de forma genérica, atributos del dispositivo, y muchos de ellos son relevantes durante el establecimiento de una conexión, para identificar o autenticar al dispositivo frente a otros.

Ser capaz de establecer los atributos del dispositivo es una habilidad necesaria para, entre otras cosas, suplantar a un dispositivo conocido frente a otros o simular diferentes escenarios.

Para ello, los distintos atributos de un controlador pueden ser modificados. Algunos de ellos pueden ser modificados a través de mecanismos provistos por el estándar de Bluetooth. Otros han de ser modificados usando mecanismos propios del fabricante o incluso mediante la modificación del firmware que ejecutan.


## Mecanismos estándar

Se trata de mecanismos soportados por el estándar Bluetooth y que deberían ser compatibles con todos los adaptadores Bluetooth del mercado. Normalmente estos procedimientos se hacen mediante el envío de paquetes HCI.


### Modificación del nombre

El `Local Name` es el nombre que el controlador expone a otros dispositivos. Este parámetro puede ser modificado a través de comandos HCI estándar. Más concretamente esto se debe hacer a través de un `HCI Write Local Name Command`. Este comando se encuentra documentado en el Bluetooth Core V5.3, Vol. 4, Part E, Section 7.3.11.


### Modificación de la clase de dispositivo

El `Device Class` puede ser modificado mediante el comando `HCI Write Class of Device Command` documentado en  el Bluetooth Core V5.3, Vol. 4, Part E, Section 7.3.26.


## Mecanismos propios de fabricante

### Broadcom / Cypress

Los fabricantes Broadcom y Cypress implementan muchos comandos HCI no estándar que permiten la modificación de atributos de sus controladores. Estos comandos no se encuentran documentados oficialmente aunque a continuación se listan algunos de los conocidos que pueden ser de utilidad:

| Comando       | Opcode    | Parametros        |
|:--------------|:----------|:------------------|
| Write Address | 0xfc01    | Dirección MAC     |
| Write RAM     | 0xfc4c    | Dirección, Datos  |

A través de estos comandos se pueden sobrescribir regiones de memoria que contienen datos como la versión de LMP soportada, la identificación del fabricante, los _features_ y _extended features_ de nuestro controlador.


## Referencias externas

* Bluetooth Core V5.3, Vol. 4, Part E, Section 7.3.11 - Write Local Name command
* Bluetooth Core V5.3, Vol. 4, Part E, Section 7.3.26 - Write Class of Device command
