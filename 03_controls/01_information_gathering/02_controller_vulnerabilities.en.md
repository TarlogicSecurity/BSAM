---
layout: control
title: BSAM-IG-02
summary: Known Bluetooth controller vulnerabilities
description: How to identify vulnerabilities in your Bluetooth controller to ensure its security.
parent: Information gathering
grand_parent: Controls
nav_order: 2
lang: en
page_id: cont_info_02
permalink: controls/bluetooth-controller-vulnerabilities/
tags:
- BR/EDR
- BLE
---


The lower layers of the Bluetooth standard are implemented in the controller firmware, which is updated less frequently than the host and could contain critical vulnerabilities.

Identifying known vulnerabilities is a fundamental step in a security audit. It needs to be verified whether these vulnerabilities affect the analyzed device to avoid false positives.


## Description

The enumeration of known vulnerabilities can be done by searching in vulnerability databases or in general-purpose search engines.

It important to verify the applicability of each of the vulnerabilities found against the analyzed device.


## Related resources

Resources that can be useful for the identification of the driver model on the device can be found in the following table:

{% include res_table.md resources='BSAM-RES-01,BSAM-RES-02' %}

For vulnerability enumeration the following resources may be of interest:

{% include res_table.md resources='BSAM-RES-03' %}


## Example case

For an ESP32 Bluetooth controller, the following results have been identified in different search engines:

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

Known vulnerabilities should be validated and classified to assess which ones affect and which ones are not applicable to our device.
