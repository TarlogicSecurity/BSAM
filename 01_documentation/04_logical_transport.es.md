---
layout: default
title: 'Transporte lógico: ACL y SCO'
description: Transporte lógico Bluetooth -  ACL y SCO, protocolos para la transmisión de datos y voz
parent: Documentación
nav_order: 3
lang: es
page_id: doc_logical_transport
permalink: documentacion/logical-transport/
---

# {{ page.title }}

Los protocolos de la capa de transporte lógico se usan para establecer y gestionar conexiones entre hosts de Bluetooth.

Las capas de transporte lógico son las capas de comunicación más bajas disponibles para un host y su implementación corresponde al stack de Bluetooth del host.

Existen dos protocolos de transporte lógico disponibles en función de las características de una conexión: ACL y SCO.

![Bluetooth logical transport protocols]({{ '/assets/img/doc_logical_transport.png' | relative_url}})


## ACL (Asynchronous Connection-Less)

El protocolo ACL es asíncrono y no está orientado a conexión.

Asíncrono se refiere a que este protocolo está pensado para intercambiar cantidades de datos irregulares a lo largo del tiempo. Las capas superiores que usen este protocolo necesitarán enviar datos que varían según factores externos como el uso del dispositivo, la interacción el mismo o la disponibilidad de datos...

No orientado a conexión se refiere a la capacidad para establecer enlaces "unicast" y "multicast", es decir, con uno o varios dispositivos simultáneamente.

El protocolo ACL usará el ancho de banda disponible en cada momento. Esto significa que la velocidad del enlace es dependiente de otras conexiones e intercambios de información, reduciendo la velocidad cuando múltiples dispositivos solicitan el uso del enlace al mismo tiempo. Esto también significa que habrá momentos en los que ACL proveerá anchos de banda muy grandes permitiendo el intercambio de grandes cantidades de información.

Este es el protocolo por defecto para el transporte lógico en Bluetooth.


## SCO (Synchronous Connection-Oriented)

El protocolo SCO es síncrono y orientado a conexión.

Síncrono se refiere a que este protocolo está pensado para transmitir un ancho de banda fijo y constante. Las capas superiores que usen este protocolo de enlace tendrán la capacidad para enviar y recibir cantidades de datos fijas en el tiempo.

Orientado a conexión se refiere a que este enlace solo permite conexiones "unicast", es decir, con un único dispositivo.

En la práctica, SCO se implementa como un slot ACL de tiempo fijo, por este motivo, SCO prioriza el intercambio de información con baja latencia.

Las características de este transporte lógico lo hacen ideal para la transmisión de datos y voz como en el caso de las llamadas de voz. Estos streams de audio y datos no requieren una calidad excelente por lo que el ancho de banda no ha de ser muy grande, pero si requieren baja latencia y una transmisión de datos consistente e invariable.