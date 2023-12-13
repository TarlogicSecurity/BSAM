---
layout: control
title: BSAM-IG-02
summary: Vulnerabilidades en el controlador Bluetooth
description: Cómo identificar las vulnerabilidades en el controlador Bluetooth de tu dispositivo para garantizar su seguridad.
parent: Recopilación de información
grand_parent: Controles
nav_order: 2
lang: es
page_id: cont_info_02
permalink: controles/vulnerabilidades-controlador-bluetooth/
tags:
- BR/EDR
- BLE
---


Las capas más bajas del estándar de Bluetooth están implementadas en el firmware del controlador, que es actualizado con menor frecuencia que el host y puede contener vulnerabilidades críticas.

La identificación de vulnerabilidades conocidas es un paso fundamental en una auditoría de seguridad. Ha de verificarse si estas vulnerabilidades afectan al dispositivo analizado para evitar falsos positivos.

## Descripción del proceso

La enumeración de vulnerabilidades conocidas puede realizarse con búsquedas en bases de datos de vulnerabilidades o en motores de búsqueda de propósito general.

Es importante verificar la aplicabilidad de cada una de las vulnerabilidades encontradas sobre el dispositivo analizado.

## Recursos relacionados

Los recursos que pueden ser de utilidad para la identificación del modelo de controlador en el dispositivo se pueden encontrar en la siguiente tabla:

{% include res_table.md resources='BSAM-RES-01,BSAM-RES-02' %}

Para la enumeración de vulnerabilidades pueden ser de interés los siguientes recursos:

{% include res_table.md resources='BSAM-RES-03' %}

## Caso de ejemplo

Para un controlador Bluetooth ESP32 se han identificado los siguientes resultados en distintos motores de búsqueda:

* [https://vuldb.com/?id.178685](https://vuldb.com/?id.178685)
* [https://vuldb.com/?id.145789](https://vuldb.com/?id.145789)
* [https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-15894](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-15894)
* [https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-17391](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-17391)
* [https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-11015](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-11015)
* [https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-13594](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-13594)
* [https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-13595](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2020-13595)
* [https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-28135](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-28135)
* [https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-28136](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-28136)
* [https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-28139](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-28139)
* [https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-34173](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-34173)
* [https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-41104](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-41104)

Las vulnerabilidades conocidas se deben validar y clasificar con el fin de evaluar cuales afectan y cuales no son aplicables a nuestro dispositivo.
