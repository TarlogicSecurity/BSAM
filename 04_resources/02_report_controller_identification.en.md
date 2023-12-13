---
layout: default
title: BSAM-RES-02
summary: Identify controller through reports
description: Identify the Bluetooth controller of your device by searching for radio certification reports
parent: Resources
nav_order: 1
lang: en
page_id: BSAM_resources_02
permalink: resources/identify-controller-reports/
---

# {{ page.summary }}

In order to be marketed in different countries, devices with wireless communications have to pass certification processes to ensure that they do not interfere with other devices.

Since Bluetooth is a wireless technology, if the device has already been certified, it is sometimes possible to obtain the certification reports. These reports may contain very relevant information for our analysis such as pictures of the printed circuit boards, transmitting and receiving power metrics, Bluetooth driver models...

In some cases, this search may require the use of search engines or the study of the web pages of the main regulatory agencies but in other cases, the certification itself requires the device to be marked with a unique identifier.

An example of the latter would be the 'FCC ID', a visible identifier that has to be marked on devices certified by the 'FCC'. On a device tested, the text `FCC ID: A3LSMR177R` is found. After a search, the device report is found at <https://fcc.report/FCC-ID/A3LSMR177R>. In the report for this FCC identifier we can find pictures and even the Bluetooth driver of the device.

![Screenshot of part of an FCC report]({{ '/assets/img/bsam-res-02_fccid-report.png' | relative_url}})

## Telecommunications regulatory agencies
Some of the telecommunications regulatory agencies that may contain reports on wireless communications devices are listed in the table below:

| Name        | URL                             |
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
