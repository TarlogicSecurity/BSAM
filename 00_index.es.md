---
title: Metodología de evaluación de seguridad de Bluetooth
description: La metodología BSAM es una guia para la evaluación de seguridad en dispositivos con capacidades Bluetooth.
image: /assets/img/BSAM.png
layout: home
nav_order: 0
lang: es
page_id: index
permalink: /
---

![BSAM - Bluetooth security assessment methodology logo]({{ '/assets/img/BSAM.png' | relative_url}})

# BSAM: Metodología para la evaluación de seguridad Bluetooth

**BSAM** es el acrónimo de _Bluetooth Security Assesment Methodology_ (**Metodología de evaluación de seguridad de dispositivos Bluetooth**). BSAM es una metodología de abierta y colaborativa desarrollada para estandarizar la evaluación de seguridad de dispositivos que hacen uso de la tecnología bluetooth.

La metodología BSAM define todos los [controles de seguridad Bluetooth]({% link 03_controls/00_index.es.md %}) necesarios para que fabricantes, investigadores de seguridad, desarrolladores de software, aficionados y profesionales de la ciberseguridad dispongan de una guía con la que realizar evaluaciones de seguridad en dispositivos con comunicaciones Bluetooth.

BSAM es un pilar fundamental para la evaluación de la seguridad de las comunicaciones en dispositivos IoT con capacidades Bluetooth y Bluetooth LE y para ayudar a planificar **pruebas de pentesting**.


## Impacto de Bluetooth en la seguridad

El estandar Bluetooth se encuentra en constante evolución y la tecnología Bluetooth está hoy en día integrada en todo tipo de dispositivos.

* **Dispositivos personales:** Portátiles, teléfonos inteligentes o tabletas utilizan Bluetooth para conectarse a dispositivos de entrada como teclados, auriculares, relojes, Gafas VR o consolas de juegos.
* **Dispositivos de transporte:** Sistemas de entretenimiento a bordo y control de vehículos, vehículos autónomos o sistemas de control de trafico.
* **Dispositivos domésticos:** Cerraduras inteligentes, altavoces inteligentes, bombillas inteligentes, termostatos inteligentes, electrodomésticos y otros sensores domóticos.
* **Dispositivos de salud y fitness:** Monitores de actividad física, monitores de frecuencia cardíaca y dispositivos médicos para recopilar y transmitir datos sobre la salud y el estado físico de los usuarios.
* **Dispositivos industriales:** Robots industriales, sensores y controladores que utilizan Bluetooth para comunicarse entre sí.


Bluetooth se usa ampliamente en dispositivos que afectan la ciberseguridad, la seguridad y la privacidad. La complejidad del estándar Bluetooth y la variabilidad en su configuración dificultan la implementación de medidas de seguridad efectivas. BSAM estandariza las pruebas de **seguridad Bluetooth** para garantizar que los dispositivos Bluetooth sean seguros y privados.

## Estructura de la metodología BSAM

Para familiarizarse con el estándar de Bluetooth, puede consultar la sección [Documentación]({% link 01_documentation/00_index.es.md %}). Esta sección contiene información básica acerca de dónde obtener detalles técnicos adicionales o resúmenes de la tecnología que se va a analizar.

Antes de iniciar una auditoría de seguridad, es importante conocer la lista de [consideraciones previas]({% link 02_preliminary_considerations/00_index.es.md %}) de la metodología para comprender las ventajas e inconvenientes del análisis propuesto, así como para asegurarse de que en todo momento se cumple con la normativa vigente.

La metodología propuesta está compuesta por múltiples [controles]({% link 03_controls/00_index.es.md %}) que han de evaluarse individualmente para verificar que el dispositivo es seguro desde el punto de vista de las comunicaciones Bluetooth.

Los controles de esta metodología se centran en las comprobaciones que han de realizarse, pero no en cómo han de realizarse. Para facilitar el trabajo, se enumeran algunos [recursos]({% link 04_resources/00_index.es.md %}) que permiten la ejecución de los controles.


## Estado de la guia de seguridad Bluetooth

El proyecto BSAM tiene como objetivo crear una completa **guia de seguridad Bluetooth** y por eso está en continuo desarrollo. ¡Esta metodología Bluetooth no se considera terminada y puede recibir modificaciones que afecten a su contenido o estructura!

A continuación, se listan algunas de las tareas en progreso:
* Expandir la sección de documentación
* Expandir la sección de recursos
* Relacionar los controles con estándares y normativas de seguridad como: GSMA IoT Security Assessment , IEC 62443 ciberguridad industrial, ETSI EN 303645.

Esta metodología ha sido desarrollada por [Tarlogic](https://www.tarlogic.com/es/) con licencia [CC BY](https://creativecommons.org/licenses/by/4.0/).

<a href="https://github.com/TarlogicSecurity/BSAM" class="btn btn-primary">Ver en GitHub</a>