---
layout: control
title: BSAM-SE-02
summary: Servicios GATT Bluetooth ocultos
description: Comprueba que tu dispositivo Bluetooth liste todos los servicios disponibles en GATT. Es importante para evitar que un atacante pueda ocultar servicios y acceder a ellos sin tu conocimiento o consentimiento
parent: Servicios
grand_parent: Controles
nav_order: 2
lang: es
page_id: cont_services_02
permalink: controles/servicios-gatt-bluetooth-ocultos/
tags:
- BR/EDR
- BLE
---

En Bluetooth, un servicio se refiere a una funcionalidad o característica específica que un dispositivo ofrece a otros dispositivos dentro de una red Bluetooth. Cada servicio se identifica por un Identificador Único Universal (UUID), y puede ser descubierto y utilizado por otros dispositivos Bluetooth que soportan ese servicio en particular.

En Bluetooth existen múltiples formas de descubrir los servicios disponibles en un dispositivo. Uno de los mecanismos para realizar este descubrimiento es el _Perfil de Atributos Genéricos_ o _GATT_ y permite la comunicación entre dispositivos mediante el intercambio de atributos (datos organizados en un formato específico).

Es obligatorio que _GATT_ esté presente, es importante que se configure correctamente y que liste todos los servicios disponibles en el dispositivo. No listar un servicio en _GATT_ no impide que los usuarios lo accedan y no lo asegura.

De igual manera, si un servicio está correctamente asegurado, no es necesario ocultarlo y configurar _GATT_ correctamente para listarlo estará en conformidad con las Especificaciones de Bluetooth, lo que permitirá una mejor interoperabilidad entre dispositivos.

## Descripción del proceso

Para verificar si existen servicios ocultos, es importante realizar un descubrimiento de _GATT_. Este es un procedimiento estándar de Bluetooth que se puede realizar con las herramientas estándar del sistema.

Después de tener una lista de servicios disponibles en _GATT_, es necesario realizar formas alternativas de descubrir servicios Bluetooth. Esto puede incluir los siguientes procedimientos:

* Capturar comunicaciones Bluetooth mientras se opera el dispositivo para descubrir comunicaciones con servicios previamente desconocidos.
* Realizar escaneos de fuerza bruta de servicios en un dispositivo.
* Realizar escaneos de servicios utilizando diferentes protocolos de descubrimiento de servicios, como SDP si está disponible.

Si hay servicios ocultos al servicio _GATT_, no está configurado correctamente.

## Recursos relacionados

Para verificar este control, los siguientes recursos pueden ser útiles:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}

## Caso de ejemplo

Se va a comprobar la existencia de servicios GATT ocultos en un dispositivo con conectividad BLE. Las pruebas se llevarán a cabo con un ordenador. Usaremos Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) para realizar la captura de paquetes para su análisis. 

Para este control será necesario contar con [python 3](https://www.python.org/downloads/windows/) instalado.
Desde un terminal será necesario instalar los paquetes asyncio y bleak con el asistente de instalación de python _pip_.

`pip install asyncio`

`pip install bleak`

Para realizar la búsqueda de los servicios _Service_ expuestos por GATT se usará el script de bleak: [service_explorer.py](https://github.com/hbldh/bleak/blob/develop/examples/service_explorer.py).

Para ejecutarlo es necesario conocer la dirección MAC del dispositivo objetivo y escribir el comando:

`python service_explorer.py --address XX:XX:XX:XX:XX:XX >> gatt_explorer.txt`

Se dispone de un archivo (_gatt\_explorer.txt_) con las trazas generadas por el script _service_explorer.py_ que se ordena para facilitar su lectura.

![gatt_explorer.txt ordenado]({{ '/assets/img/bsam-se-02_gatt_profiler.png' | relative_url}})

Para cada servicio (_Service_) hay una o varias características (_Characteristic_) que contiene uno o varios descriptores (_Descriptor_) y un valor (_Value_). Los servicios, características y descriptores tienen dos campos adicionales: el _Handle_ y el _UUID_. 

El controlador de Bluetooth internamente lista los servicios GATT mediante el campo _Handle_ o _UUID_. El estándar ha limitado, para un dispositivo, el número de servicios, características y descriptores a 65.535 (0xFFFF), equivalente a recorrer desde el valor 1 hasta el 65.535 el _Handle_. A cada _Handle_ le corresponde un _UUID_ y viceversa.

Para comprobar si existen servicios ocultos, se realizará una petición de lectura con Scapy, o alguna herramienta similar como gatttool, para cada _Handle_, en función de la respuesta obtenida para cada consulta existirá o no el _Handle_. Los posibles casos de respuesta a una petición de lectura , según el protocolo ATT, permitirán saber si existe o no un _Handle_.

| Respuesta de lectura | Existe el _Handle_ |
|---------------|---------|
| Respuesta afirmativa a la lectura | SI |
| Error por Autorización insuficiente | SI |
| Error por Autenticación insuficiente | SI |
| Error por longitud de clave de cifrado demasiado corta | SI |
| Error por cifrado insuficiente | SI |
| Error por _Handle_ invalido | NO |
| Error por lectura no permitida | SI |

Una vez se tiene la salida de las dos herramientas, se comparan los resultados y si existen _Handle_ que no aparecen listados con el script de bleak, quiere decir que es un servicio oculto.

El resultado del control será _FAIL_ cuando se encuentran más _Handle_ existentes que los descubiertos en la exploración del GATT.
