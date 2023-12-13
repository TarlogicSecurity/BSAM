---
layout: control
title: BSAM-SE-03
summary: Control de acceso a servicios Bluetooth
description: Verifica el control de acceso de los servicios Bluetooth de tu dispositivo para evitar accesos no autorizados.
parent: Servicios
grand_parent: Controles
nav_order: 3
lang: es
page_id: cont_services_03
permalink: controles/control-acceso-servicio-bluetooth/
tags:
- BR/EDR
- BLE
---

Los servicios de Bluetooth tienen una serie de permisos de manera que se puede limitar la manera de interactuar con ellos, si hace falta autenticacion previa o si se pueden leer o escribir.

Para cada servicio Bluetooth expuesto en un dispositivo es necesario verificar si se ha implementado correctamente el control de acceso. 

Esto se debe a que el estándar está enfocado a la interconectividad y por tanto hay servicios de perfiles en los que prima la interconectividad y deben de permitir el acceso sin autenticación como puede ser el caso del servicio del perfil GATT. 

Un dispositivo, debido al intercambio de características (_Feature Exchange_), podría haber necesitado cifrado para establecer la conexión y acceder al perfil GATT. Sin embargo, dado que existen varios tipos de cifrado, es posible que no brinde un nivel de seguridad suficiente para acceder a un _Servicio_ que requiera una mayor protección. Del mismo modo, puede ocurrir que, para acceder a ciertas funcionalidades expuestas en un servicio desde el perfil de atributos general, sea necesario incluso elevar el nivel de cifrado, pasando de la _Phase 2_ a la _Phase 3_ del emparejamiento, con el fin de acceder a dicho _Servicio_.

Por ello es importante verificar la cifrado configurada para cada nivel de seguridad implementado en el dispositivo.


## Descripción del proceso

Para realizar esta verificación, es necesario elaborar una lista de los dispositivos disponibles {% include ctl_link.md ctl='BSAM-SE-01' %} y {% include ctl_link.md ctl='BSAM-SE-02' %}.

Para cada servicio expuesto, es necesario realizar una comprobación sobre qué recursos se pueden acceder y modificar. Esto requiere cierta adaptación para cumplir con los protocolos que exponen el servicio. Algunos ejemplos ilustran esta necesidad:

* Para cualquier servicio GATT se debe verificar si los atributos se pueden leer, escribir o notificar según nuestro nivel de seguridad y autenticación. Para cada resultado se debe considerar si ese acceso es adecuado o no según los datos que expone.
* Para cada RFCOMM, se debe verificar si un usuario puede leer o escribir en ese servicio para cada nivel de seguridad disponible en ese servicio. Se debe evaluar si los datos expuestos son adecuados para el nivel de seguridad y autenticación.
* Para el protocolo SDP, se debe verificar si es correcto obtener acceso al servicio sin autenticación y/o cifrado a nivel de Bluetooth.
* Es importante verificar qué funcionalidades del protocolo SMP se pueden acceder sin autenticación y/o cifrado a nivel de Bluetooth.

## Recursos relacionados

Para verificar este control, los siguientes recursos pueden ser útiles:

{% include res_table.md resources='BSAM-SE-01,BSAM-SE-02' %}

Otra documentación relacionada:

| ID               | Descripción                                                        |
|:-----------------|:-------------------------------------------------------------------|
| Documentación    | [Modo de conexión en Bluetooth LE]({% link 01_documentation/06_le_modes.es.md %})                                   |

## Caso de ejemplo

Se va a comprobar la correcta configuración (lectura / escritura) de los servicios GATT en un dispositivo con conectividad BLE. Las pruebas se llevarán a cabo con un ordenador que se conectará (sin autorización, sin autenticación y sin cifrado) al un dispositivo. Usaremos Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) para realizar la captura de paquetes para su análisis. 

El perfil GATT describe la forma en que se intercambian datos con los perfiles (_Profile_). En él, se definen los _Profile_, que agrupan un conjunto de servicios (_Service_). Estos _Service_, a su vez, están compuestos por características (_Characteristic_), y estas _Characteristic_ poseen descriptores (_Descriptor_).

Los permisos para leer, escribir o realizar acciones específicas con cada servicio y característica depende de la autorización, autenticación y cifrado. Estos requisitos (autorización, autenticación y cifrado) son específicos de cada dispositivo y los fijó el desarrollador del mismo.

El controlador de Bluetooth internamente lista los servicios GATT mediante el campo _Handle_ o _UUID_. El estándar ha limitado, para un dispositivo, el número de servicios, características y descriptores a 65.535 (0xFFFF), equivalente a recorrer desde el valor 1 hasta el 65.535 (0xFFFF) el _Handle_. A cada _Handle_ le corresponde un _UUID_ y viceversa. Hay una lista de UUIDs asociados a una función concreta en el documento del SIG de Bluetooth [Assigned Numbers](https://www.bluetooth.com/specifications/assigned-numbers/).

Se conecta el ordenador a el dispositivo y se realiza una petición de Lectura y Escritura para cada _Handle_ con Scapy, o alguna herramienta similar.Los posibles casos de respuesta a una petición de lectura / escritura, según el protocolo ATT, permitirán saber la configuración de cada _Handle_.

Las respuestas a una petición de lectura pueden ser:

| Respuesta | Descripción |
|---------------|---------|
| Respuesta afirmativa a la lectura | El _Handle_ es legible sin permisos |
| Error por Autorización insuficiente | El _Handle_ necesita autorización para ser leído |
| Error por Autenticación insuficiente | El _Handle_ necesita autenticación para ser leído |
| Error por longitud de clave de cifrado demasiado corta | El _Handle_ necesita un cifrado con una contraseña más larga|
| Error por cifrado insuficiente | El _Handle_ necesita un nivel de cifrado superior (i.e., pasar de emparejamiento Legacy a Secure) |
| Error por _Handle_ invalido | El _Handle_ no existe |
| Error por lectura no permitida | El _Handle_ no es legible |


Las respuestas a una petición de escritura pueden ser:

| Respuesta | Descripción |
|---------------|---------|
| Respuesta afirmativa a la Escritura | El _Handle_ se puede escribir sin permisos |
| Error por longitud invalida | El _Handle_ se puede escribir pero la longitud es incorrecta |
| Error por Autorización insuficiente | Se necesita estar autorizado para escribir el _Handle_ | 
| Error por Autenticación insuficiente | Se necesita estar autenticado para escribir el _Handle_ |
| Error por longitud de clave de cifrado demasiado corta | Se necesita una clave más larga para escribir en el _Handle_ |
| Error por cifrado insuficiente | Se necesita tener un nivel de cifrado superior para escribir el _Handle_ (i.e., pasar de emparejamiento Legacy a Secure) |
| Error por _Handle_ invalido | El _Handle_ no existe |
| Error por lectura no permitida | El _Handle_ no se puede escribir |


Se comprueba para el dispositivo que cada configuración es correcta, cotejándolo con el documento _Assigned Numbers_ del SIG de Bluetooth con la [guía de seguridad Bluetooth](https://www.nist.gov/publications/guide-bluetooth-security-2) del NIST.

El resultado del control será _FAIL_ cuando un _Handle_ tenga los permisos incorrectos para el tipo de operación asignada en función del tipo de dispositivo según las especificaciones del NIST. 

## Referencias externas

* Bluetooth Core V5.3, Vol. 3, Part H, Section 2 SECURITY MANAGER
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.1 INTRODUCTION
* Bluetooth Core V5.3, Vol. 3, Part H, Section 3 SECURITY MANAGER PROTOCOL
* Bluetooth Core V5.3, Vol. 3, Part H, Section 3.6 SECURITY IN BLUETOOTH LOW ENERGY
