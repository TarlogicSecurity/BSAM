---
layout: control
title: BSAM-EN-02
summary: Uso de cifrado forzado
description: Comprueba que tu dispositivo Bluetooth requiere el uso de cifrado. Es importante para proteger tus datos de ataques de sniffing y extracción de datos.
parent: Cifrado
grand_parent: Controles
nav_order: 2
lang: es
page_id: cont_enc_02
permalink: controles/forzar-uso-cifrado-bluetooth/
tags:
- BR/EDR
- BLE
---

El uso de cifrado en las conexiones Bluetooth es opcional.

El uso de mecanismos de cifrado es muy recomendable para el acceso a cualquier servicio que exponga información sensible o permita controlar el dispositivo de forma no autorizada.

No utilizar cifrado expone los datos de una conexión a ataques de sniffing y permite el uso de ataques como BIAS para extraer datos del dispositivo sin compartir o conocer la clave de enlace.

## Descripción del proceso

Para comprobar si el uso de cifrado es requerido, deben clasificarse primero los servicios que requieren confidencialidad.

Una vez listados los servicios sensibles según su funcionalidad, se prueba a conectarse a ellos sin disponer de la clave de enlace.

Este control se considerará satisfactorio si no se permite la lectura de datos, acceso a funciones o control del dispositivo. 


## Recursos relacionados

Para comprobar este control, pueden ser útiles los siguientes recursos:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07,BSAM-RES-09' %}

Otra documentación relacionada:

| ID               | Descripción                                                        |
|:-----------------|:-------------------------------------------------------------------|
| Documentación    | [Modo de conexión en Bluetooth LE]({% link 01_documentation/06_le_modes.es.md %})                                   |

## Caso de ejemplo

Usaremos Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) para realizar la captura de paquetes para su análisis.

Se procede a comprobar el emparejamiento entre unos auriculares Bluetooth y un ordenador. Se solicita la conexión con la aplicación de gestión de dispositivos Bluetooth integrada en el sistema operativo del ordenador.

En la captura de Wireshark se debería de poder localizar el proceso de emparejamiento completo con un comando _Pairing Request_ seguido del comando de respuesta _Pairing Response_ y para finalizar debería de confirmarse el emparejamiento con el comando _Pairing Confirm_.

![Wireshark Pairing Process]({{ '/assets/img/bsam-en-02_pairing_process.png' | relative_url}})

En el comando _Pairing Request_ el campo _Secure Connections_ debería tener valor `0x01`.

![Wireshark Pairing Request]({{ '/assets/img/bsam-en-02_pairing_request.png' | relative_url}})

En el comando _Pairing Request_ el campo _Secure Connections_ debería tener valor `0x01`.

![Wireshark Pairing Response]({{ '/assets/img/bsam-en-02_pairing_response.png' | relative_url}})

De esta forma se fuerza el cifrado entre ambos dispositivos y se establece una generación de claves seguras.

Si en los mensajes de _Pairing Request_ y _Pairing Response_ inmediatamente anteriores al _Pairing Confirm_ tiene el campo _Secure Connections_ un valor diferente a `0x01` este control habrá fallado.

El resultado del control será _FAIL_ si alguno de los dos dispositivos presenta campo _Secure Connections_ con valor `0x00` y se establece la conexión.