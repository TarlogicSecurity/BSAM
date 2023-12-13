---
layout: control
title: BSAM-AP-04
summary: Firma digital de actualizaciones
description: Actualizaciones firmadas digitalmente - Verifica si las actualizaciones están firmadas digitalmente para evitar software malicioso
parent: Aplicación
grand_parent: Controles
nav_order: 4
lang: es
page_id: cont_app_04
permalink: controles/firma-digital-actualizaciones-bluetooth/
tags:
- BR/EDR
- BLE
---

Para cada uno de los mecanismos de actualización disponibles en un dispositivo es necesario realizar una verificación para asegurar que estén firmadas digitalmente las actualizaciones.

Si un dispositivo utiliza y acepta actualizaciones o software no firmados, entonces es posible crear piezas de software personalizadas, firmware y aplicaciones que puedan acceder a datos o realizar acciones maliciosas al utilizar el dispositivo de maneras no previstas por el fabricante y el usuario.

## Descripción del proceso

Para verificar si las actualizaciones están firmadas digitalmente, se pueden utilizar múltiples técnicas:

* Revisión del código fuente.
* Modificar ligeramente bytes aleatorios de un archivo de actualización válido y tratar de usar el software modificado. Tenga en cuenta que esto puede resultar en un dispositivo inutilizable si finalmente se instala el software modificado.

Este control es satisfactorio cuando se verifique que el dispositivo no admite de manera remota actualizaciones minimamente modificadas. 