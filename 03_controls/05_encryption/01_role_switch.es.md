---
layout: control
title: BSAM-EN-01
summary: Cambio de rol antes de cifrado
description: Evita que se pueda cambiar el rol del dispositivo Bluetooth después de la autenticación.
parent: Cifrado
grand_parent: Controles
nav_order: 1
lang: es
page_id: cont_enc_01
permalink: controles/cambio-rol-antes-cifrado-bluetooth/
tags:
- BR/EDR
---

En una comunicación Bluetooth existen dos roles definidos: 
 * Maestro (_master_): Es el rol principal de una "piconet" (red de dispositivos Bluetooth) y define los parámetros físicos de las conexiones.
 * Esclavo (_slave_): Dispositivo que se limita a seguir los pasos del maestro. 

El cambio de rol puede ser utilizado por dispositivos que sólo pueden actuar como esclavos para evitar ser maestros de la conexión. Este procedimiento de cambio de rol suele tener lugar durante el establecimiento de una conexión y siempre antes de la fase de autenticación.

Si este tiene lugar tras la fase de autenticación y antes de la fase de cifrado, puede ser utilizado para atacar la fase de cifrado y extraer la clave temporal usada durante una conexión, atacando la confidencialidad de los datos de esa conexión. Por ello, es preferible denegar el uso del mecanismo de cambio de rol tras el paso de autenticación mientras no sea necesario para el funcionamiento normal del dispositivo.


## Descripción del proceso

Dado que el cambio de rol se produce mediante un mecanismo en capa de enlace (_LMP_ o _Link Layer_), para verificar que el dispositivo no permite un cambio de rol tras la autenticación existen dos posibles opciones:
* Mediante la revisión del código fuente del firmware de controlador
* Mediante el envío de una solicitud de cambio de rol tras el proceso de autenticación, se captura la respuesta para evaluar si se permite el cambio.

Para el envío de una solicitud de cambio de rol tras el proceso de autenticación es necesario modificar el flujo habitual de mensajes LMP y, por tanto, es necesario tener la capacidad de enviar y recibir mensajes LMP. Para esto, habitualmente, es necesario modificar el firmware del controlador Bluetooth utilizado para realizar la auditoría (no el del dispositivo auditado).

Es necesario introducir modificaciones para que se envíe un mensaje LMP_switch_req justo tras la autenticación y antes del inicio del cifrado. El dispositivo auditado podrá responder con un mensaje LMP_accepted, lo que indica que ha aceptado el cambio de rol y no cumple el control, o con LMP_not_accepted, que sería la respuesta esperada por un dispositivo que cumple el control.

Además, para capturar la respuesta, es necesario poder capturar mensajes LMP, para lo cual se pueden utilizar los mensajes de depuración del controlador Bluetooth: los controladores Broadcom redirigen el tráfico LMP hacia el host en forma de mensajes de depuración.

Para la modificación del firmware, es necesario disponer de un controlador Bluetooth que permita estas modificaciones y del que se disponga del código del firmware (o del que exista un trabajo de ingeniería inversa). Las modificaciones al firmware de la placa de desarrollo Cypress CYW920819EVB-02, realizadas para la investigación de [BIAS](https://francozappa.github.io/about-bias/), son de utilidad en este aspecto.

## Recursos relacionados

Para comprobar este control, pueden ser útiles los siguientes recursos:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07,BSAM-RES-09' %}

## Caso de ejemplo

Usaremos Wireshark con [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) para realizar la captura de paquetes para su análisis.

<<<<<<< HEAD
Se dispone de un portátil con la herramienta Scapy que está realizando un emparejamiento con rol de Periférico con otro dispositivo de rol Central. Durante el proceso de conexión existe la posibilidad de realizar un paso extra que se llama cambio de rol.
=======
Se dispone de un ordenador con la herramienta Scapy que está realizando un emparejamiento con rol de Periférico con otro dispositivo de rol Central. Durante el proceso de conexión existe la posibilidad de realizar un paso extra que se llama cambio de rol.
>>>>>>> fdf8cd6a8e3182eb139f0d88e92313c413b12fbb

![Bluetooth Core 5.3 Connection Stablishment]({{ '/assets/img/bsam-en-01_connection_stablishment.png' | relative_url}})

El dispositivo Periférico recibe la petición de conexión del dispositivo central mediante el comando _HCI\_Connection\_Request_.

![Wireshark Connection request]({{ '/assets/img/bsam-en-01_connection_request.png' | relative_url}})

Tras esto la respuesta del dispositivo Periférico es aceptar la conexión con el comando _HCI\_Accept\_Connection\_Request_ indicando que su preferencia es trabajar como dispositivo central indicando en el campo _Role_ un valor `0x00` (`Become Central for this connection. The LM will perform the role switch.`).

Si el dispositivo remoto acepta este cambio de rol responderá con el comando _HCI\_Role\_Change_ con el campo _Status_ en valor `0x00` (`A Role change has ocurred.`). Con cualquier otro valor se indicaría el código de error del procedimiento.

Para saber si este cambio de rol se está produciendo antes del proceso de cifrado se deben de localizar el comando _HCI\_Link\_Key\_Request\_Reply_ seguido del comando _HCI\_Command\_Complete_ en la captura de Wireshark.

Si el comando _HCI\_Accept\_Connection\_Request_ aparece después los dos comandos (_HCI\_Link\_Key\_Request\_Reply_ / _HCI\_Command\_Complete_ ) es un _role switch_ previo a la etapa de cifrado. 

El resultado del control será _FAIL_ si se localiza el comando _HCI\_Role\_Change_ con el campo _Status_ con valor `0x00` antes del cifrado de los dispositivos.