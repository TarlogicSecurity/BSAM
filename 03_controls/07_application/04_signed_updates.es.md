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

Para cada mecanismo de actualización disponible, es necesario verificar que las actualizaciones estén firmadas digitalmente. Si un dispositivo acepta firmware o software sin firmar, un atacante podría crear componentes maliciosos capaces de acceder a datos o realizar acciones no autorizadas.

## Descripción del proceso

Para verificar si las actualizaciones están firmadas digitalmente, se pueden utilizar múltiples técnicas:

* Revisión del código fuente.
* Modificar ligeramente bytes aleatorios de un archivo de actualización válido y tratar de usar el software modificado. Tenga en cuenta que esto puede resultar en un dispositivo inutilizable si finalmente se instala el software modificado.

Este control es satisfactorio cuando se verifique que el dispositivo no admite de manera remota actualizaciones minimamente modificadas. 