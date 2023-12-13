---
layout: control
title: BSAM-AU-01
summary: Cambio de rol antes de la autenticación
description: Los dispositivos Bluetooth deben evitar permitir el cambio de rol de esclavo a maestro antes de la autenticación. Esto puede ser aprovechado por atacantes para evitar la autenticación.
parent: Autenticación
grand_parent: Controles
nav_order: 1
lang: es
page_id: cont_auth_01
permalink: controles/cambio-rol-autenticacion-bluetooth/
tags:
- BR/EDR
---

En una comunicación Bluetooth existen dos roles definidos: 
 * Maestro (_master_): Es el rol principal de una "piconet" (red de dispositivos Bluetooth) y define los parámetros físicos de las conexiones.
 * Esclavo (_slave_): Dispositivo que se limita a seguir los pasos del maestro. 
 
 Tras el inicio de una conexión física, el dispositivo iniciador se convierte automáticamente en maestro.

En este contexto, el cambio de rol se utiliza habitualmente por dispositivos que sólo pueden actuar como esclavos para evitar ser maestros de la conexión cuando han sido iniciadores de esta.

Sin embargo, este mecanismo puede ser abusado para evitar el proceso de autenticación por parte de un dispositivo que actúa como maestro, ya que el proceso de autenticación _Legacy_ sólo exige que el dispositivo maestro autentique al esclavo, pero no a la inversa.

Por ejemplo, la vulnerabilidad BIAS utiliza esta capacidad para que un dispositivo esclavo pase a ser el maestro de la comunicación justo antes de la autenticación, y así evitar ser autenticado por el otro extremo. Con esta estrategia se puede suplantar la identidad de otros dispositivos frente a una víctima con la que el dispositivo suplantado estuviese emparejado.

Es preferible no permitir el uso del mecanismo de cambio de rol de esclavo a maestro si no es necesario para el funcionamiento normal del dispositivo.

## Descripción del proceso

El mensaje de cambio de rol se produce con mensajes LMP, por lo que depende del firmware del controlador Bluetooth.

Para comprobar si un dispositivo permite un cambio de rol de esclavo a maestro, es necesario disponer de un controlador que permita enviar mensajes LMP de cambio de rol en momentos clave de la comunicación.

Las modificaciones al firmware del controlador CYW920819WCD2 realizadas como parte de la PoC de [BIAS](https://github.com/francozappa/bias) incluyen un parche para cambiar el rol del dispositivo de esclavo a maestro antes de la autenticación, por lo que, en conjunto con Wireshark, puede utilizarse para comprobar el control, aunque para ello se requiere la placa de desarrollo CYW920819EVB-02, o una compatible con el mecanismo "Patch ROM".

El proceso consiste en parchear el firmware de la placa (hay un ejemplo de cómo hacerlo en [el repositorio de BIAS](https://github.com/francozappa/bias/blob/master/bias/bias-template.py)) y emplear la placa para iniciar una conexión con el dispositivo auditado mientras se captura la comunicación con Wireshark. Antes de iniciar la comunicación es necesario activar los mensajes de depuración de la placa, que incluyen los mensajes LMP enviados y recibidos. Además, se deberá capturar el tráfico a través de la interfaz de Bluetooth monitor y utilizar el disector de mensajes de depuración de Broadcom para observar e interpretar los mensajes LMP correctamente.

Antes de la autenticación, se observará el envío de un mensaje `LMP_role_switch`. Si el mensaje recibe una respuesta `LMP_accepted`, el dispositivo se puede cambiar de rol.


## Recursos relacionados

Para comprobar este control, pueden ser útiles los siguientes recursos:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07,BSAM-RES-09' %}

## Caso de ejemplo

Usaremos Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) para realizar la captura de paquetes para su análisis.

Se dispone de un ordenador con la herramienta Scapy, que está realizando un emparejamiento con rol Periférico con otro dispositivo de rol Central. Durante el proceso de conexión existe la posibilidad de realizar un paso extra de _cambio de rol_.

![Bluetooth Core 5.3 Connection Stablishment]({{ '/assets/img/bsam-au-01_connection_stablishment.png' | relative_url}})

El dispositivo Periférico recibe la petición de conexión del dispositivo Central mediante el comando _HCI\_Connection\_Request_.

![Wireshark Connection request]({{ '/assets/img/bsam-au-01_connection_request.png' | relative_url}})

La respuesta del dispositivo periférico es aceptar la conexión con el comando _HCI\_Accept\_Connection\_Request_, notificando que su preferencia es trabajar como dispositivo Central, esto se indica con el valor `0x00` (`Become Central for this connection. The LM will perform the role switch.`) en el campo _Role_.

Si el dispositivo remoto acepta este cambio de rol responderá con el comando _HCI\_Role\_Change_ con el campo _Status_ en valor `0x00` (`A Role change has ocurred.`). Con cualquier otro valor se indicaría el código de error de este procedimiento.

Para saber si este cambio de rol está siendo antes del proceso de autenticación se deben de localizar el comando _HCI\_Link\_Key\_Request\_Reply_ seguido del comando _HCI\_Command\_Complete_ en la captura de Wireshark. 

Si el comando _HCI\_Accept\_Connection\_Request_ está antes de los dos comandos (_HCI\_Link\_Key\_Request\_Reply_ / _HCI\_Command\_Complete_ ) es un _role switch_ previo a la etapa de autenticación.

El resultado del control será _FAIL_ si se localiza el comando _HCI\_Role\_Change_ con el campo _Status_ con valor `0x00` antes de la autenticación de los dispositivos.