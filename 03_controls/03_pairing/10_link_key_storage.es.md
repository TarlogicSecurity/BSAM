---
layout: control
title: BSAM-PA-10
summary: Almacenamiento de claves de enlace Bluetooth
description: Asegúrate de que tu dispositivo Bluetooth almacene las claves de enlace de forma segura para evitar ataques.
parent: Emparejamiento
grand_parent: Controles
nav_order: 10
lang: es
page_id: cont_pair_10
permalink: controles/almacenamiento-claves-enlace-bluetooth/
tags:
- BR/EDR
- BLE
---

El proceso de emparejamiento concluye con la creación de una clave que ambos dispositivos almacenan para autenticar y cifrar futuras conexiones. Debido a que esta clave protege la confidencialidad de las comunicaciones Bluetooth, su almacenamiento debe ser seguro.

En sistemas Linux, las claves suelen almacenarse en archivos de texto plano protegidos solo por permisos de filesystem. En dispositivos sin almacenamiento cifrado, esto puede ser insuficiente, ya que un atacante con acceso al filesystem podría extraerlas.

En dispositivos pequeños o con implementaciones propietarias (p. ej., auriculares y altavoces Bluetooth), los mecanismos de almacenamiento varían según el fabricante. En estos casos, deben verificarse mediante ingeniería inversa o análisis de firmware.

## Descripción del proceso

Verificar que la confidencialidad de las claves está protegida requiere conocer el software del dispositivo y la presencia de chips TPM.

En dispositivos con sistemas operativos estándar, será posible utilizar las herramientas de que disponen. En el caso de sistemas Linux, la pila BlueZ almacena las claves de enlace en ficheros de texto plano que habitualmente pueden encontrarse en `/var/lib/bluetooth`. En el caso de Android, las claves también se pueden encontrar en `/data/misc/bluedroid/bt_config.conf`. Windows hace uso de la siguiente clave de registro `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\BTHPORT\Parameters\Keys\`.

En dispositivos con firmware propio puede ser necesario extraer el firmware y examinarlo mediante técnicas de reversing o mediante procesos de revisión de código fuente, si este está disponible.

## Caso de ejemplo

La siguiente captura muestra el contenido del fichero de información del dispositivo 84:5F:04:F1:45:CA en la ruta `/var/lib/bluetooth` de un dispositivo con Linux, emparejado con nuestro controlador (20:81:9A:10:00:00), en el que se encuentra el valor de la clave de enlace:

![Información de BlueZ para el dispositivo 84:5F:04:F1:45:CA con el controlador 20:81:9A:10:00:00]({{ '/assets/img/bsam-pa-08_bluez_linkkey.png' | relative_url}})

Los ficheros están protegidos por los privilegios de usuario administrador, pero en un dispositivo con almacenamiento accesible, esta información puede ser extraída. Para evitarlo, se debe evitar el acceso no autorizado al almacenamiento, por ejemplo, mediante cifrado.
