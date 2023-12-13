---
layout: default
title: BSAM-RES-06
summary: Modo depuración en controladores Bluetooth
description: Habilita el modo de depuración en un controlador Bluetooth para capturar paquetes de datos y otra información valiosa.
parent: Recursos
nav_order: 5
lang: es
page_id: BSAM_resources_06
permalink: recursos/habilitar-depuracion-bluetooth/
---

# {{ page.summary }}

{: .note }
> Esta técnica se basa en funcionalidades no oficiales o no documentadas. No existe un mantenimiento conocido de estas herramientas por lo que no se puede asegurar que funcionen correctamente en todos los escenarios.

Algunos fabricantes parecen incluir modos de depuración o diagnóstico no documentados en sus dispositivos.

Estos modos de diagnóstico no están estandarizados pero habilitan funcionalidades no disponibles en modos de operación comunes. Ejemplos de estas funcionalidades son la capacidad para mostrar todos los paquetes de la capa "Link Layer" enviados y recibidos.

A continuación se listan algunos procedimientos conocidos para habilitar estos modos de depuración.

## Broadcom/Cypress

| Dispositivos compatibles  |
|:--------------------------|
| Cypress CYW20735B1        |
| Cypress CYW20819A         |

Este procedimiento solo está documentado en Linux. Todos los dispositivos listados anteriormente exponen una interfaz UART cuando se conectan a una máquina. Para ser reconocidos como dispositivos Bluetooth, es necesario ejecutar el proceso `btattach`. Esto se puede realizar usando las herramientas de [Bluetooth Attach Service](https://github.com/TarlogicSecurity/Bluetooth-Attach-Service).

Cuando se ejecuta el proceso `btattach`, algunos dispositivos exponen una interfaz especial mediante debugfs. Para habilitar el modo diagnóstico se ha de escribir un `1` en el archivo `vendor_diag` correspondiente a nuestro dispositivo.

```
echo 1 > /sys/kernel/debug/bluetooth/hciX/vendor_diag
```

Si nuestra tarjeta no expone el archivo `vendor_diag`, existe la posibilidad de activar el modo de diagnóstico mediante un comando HCI específico del fabricante. Esto se puede hacer con `hcitool`:

```bash
hcitool cmd 0x3f 0xf0 0x01
```
