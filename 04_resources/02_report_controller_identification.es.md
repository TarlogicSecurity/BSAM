---
layout: default
title: BSAM-RES-02
summary: Identificación del controlador mediante informes
description: Identifica el controlador Bluetooth de tu dispositivo buscando los informes de certificación de radio.
parent: Recursos
nav_order: 1
lang: es
page_id: BSAM_resources_02
permalink: recursos/identificar-controlador-informes/
---

# {{ page.summary }}

Para poder ser comercializados en distintos países, los dispositivos con comunicaciones inalámbricas han de pasar procesos de certificación para asegurar que no producen interferencias en otros dispositivos.

Dado que Bluetooth es una tecnología inalámbrica, si el dispositivo ya ha sido certificado, en ocasiones es posible obtener los informes de certificación. Estos informes pueden contener información muy relevante para nuestro análisis como fotos de las placas de circuito impreso, métricas de potencia de emisión y recepción, modelos de controlador Bluetooth...

En algunos casos esta búsqueda puede requerir el uso de motores de búsqueda o el estudio de las páginas web de las principales agencias reguladoras, aunque en otros casos, la propia certificación requiere que el dispositivo sea marcado con un identificador único.

Un ejemplo de esto último sería el `FCC ID`, un identificador visible que ha de ser marcado en dispositivos certificados por la `FCC`. En un dispositivo analizado, se encuentra el texto `FCC ID: A3LSMR177R`. Tras una búsqueda se encuentra el reporte del dispositivo en [https://fcc.report/FCC-ID/A3LSMR177R](https://fcc.report/FCC-ID/A3LSMR177R). En el informe correspondiente a este identificador FCC podemos encontrar fotografías e incluso el controlador Bluetooth del dispositivo.

![Captura de parte de un informe de la FCC]({{ '/assets/img/bsam-res-02_fccid-report.png' | relative_url}})

## Agencias reguladoras de las telecomunicaciones
Algunas de las agencias reguladoras de las telecomunicaciones que pueden contener informes sobre dispositivos con comunicaciones inalámbricas se listan en la tabla a continuación:

| Nombre      | URL                             |
|:------------|:--------------------------------|
| UKCA        |                                 |
| FCC         | <https://fccid.io>              |
| ISED        | <https://ised-isde.canada.ca>   |
| TELEC       | <https://www.telec.or.jp>       |
| ACMA        | <https://www.acma.gov.au>       |
| KCC         |                                 |
| Vietnam MIC |                                 |
| Taiwan BSMI |                                 |
| CTIA        |                                 |
