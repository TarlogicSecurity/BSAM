---
layout: default
title: Consideraciones previas
description: Consideraciones previas a la realización de una evaluación de seguridad de dispositivos Bluetooth.
nav_order: 2
has_children: false
lang: es
page_id: prel_index
permalink: consideraciones-previas/
---

# Consideraciones previas a la evaluación de seguridad

Esta sección está dedicada a las consideraciones previas a la realización de una evaluación de seguridad Bluetooth.

Consideraciones generales se enfoca en aquellas consideraciones previas que afectan a todas las evaluaciones de seguridad por igual. Las consideraciones particulares son aquellas que varían entre distintos ejercicios de evaluación.

## Regulaciones de las comunicaciones inalámbricas

Antes de realizar una evaluación de la seguridad de las comunicaciones Bluetooth de un dispositivo es necesario consultar y conocer las normativas que regulan el uso del espacio radioeléctrico en el lugar en el que se va a realizar la auditoría.

Es fundamental tomar las medidas preventivas necesarias para no incumplir la ley, tanto en actividades que impliquen la escucha como la emisión en el espacio radioeléctrico.

Existen limitaciones muy específicas en cuanto a potencias de transmisión y el uso de distintas frecuencias, así como de la emisión de señales que puedan causar interferencias. Si es necesario realizar ensayos fuera de lo estipulado por la ley, será necesario recurrir a elementos mitigadores como jaulas de Faraday para aislar las señales.

## Interacción con dispositivos

No se deben realizar pruebas intrusivas ni interactuar con dispositivos de comunicaciones mediante el envío de paquetes mientras no se identifique claramente que el dispositivo con el que se va a interactuar está claramente definido en el alcance y estamos autorizados para realizar pruebas sobre él.


## Tipos de análisis

Dependiendo de la cantidad de información disponible para abordar una evaluación de seguridad Bluetooth, se definen las siguientes modalidades:

* **Caja negra**: Análisis sin información previa o desde un completo desconocimiento del funcionamiento del dispositivo. Es un enfoque que permite obtener una visión global del estado de la seguridad de un dispositivo. Simula un intento de ataque externo.

* **Caja gris**: En este enfoque se espera que el cliente aporte datos sobre el dispositivo con el objetivo de reducir el tiempo necesario para el proceso de recopilación de información. Esto permite orientar al analista de seguridad y enfocar sus pruebas para la evaluación de controles más adecuados al dispositivo analizado.

* **Caja blanca**: En análisis de caja blanca el alcance de la auditoría está claramente definido. El cliente aporta la máxima información posible para facilitar un proceso de revisión y orientar al analista a los puntos críticos. Es un enfoque adecuado para la revisión de la evolución de un dispositivo en materia de seguridad tras aplicar modificaciones técnicas.


## Autorización de trabajo

Antes de iniciar un análisis de seguridad de un dispositivo, es necesario contar con el permiso, acreditación u orden de trabajo firmado por el propietario del dispositivo.

Esta orden de trabajo debe identificar de forma clara y concisa los siguientes elementos:

* Nombre de la empresa (cliente)
* Persona de contacto
* Teléfono de contacto
* Persona o empresa que realiza el análisis
* Duración temporal

En determinados escenarios, como por ejemplo las revisiones de seguridad en edificios de oficinas, en centros comerciales o en zonas de seguridad donde están ubicados organismos sensibles (agencias gubernamentales, aeropuertos...), puede ser necesario solicitar la autorización de terceros antes de realizar el análisis. 
