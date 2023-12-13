---
layout: control
title: BSAM-IG-03
summary: Known Bluetooth host stack vulnerabilities
description: How to identify vulnerabilities in the Bluetooth host stack to ensure the security of your device.
parent: Information gathering
grand_parent: Controls
nav_order: 3
lang: en
page_id: cont_info_03
permalink: controls/bluetooth-host-stack-vulnerabilities/
tags:
- BR/EDR
- BLE
---


The protocols of the higher layers of the standard are implemented in the host stack (BlueZ, Bluetooth drivers in Windows, etc.). If vulnerabilities have been introduced in this stack, it is possible that they affect the entire device, so they can be critical to its security.

Just like with vulnerabilities in the controller, it must be verified which stack vulnerabilities affect the analyzed device.


## Description

First, the host stack in use must be identified. The identification of the host stack depends on the type of device in use:

* On devices with an operating system, the Bluetooth stack is usually implemented as a driver. In the case of Unix systems, this is usually BlueZ, while in Windows the Bluetooth Driver Stack is used.

* For devices with proprietary firmware, the Bluetooth stack can be a proprietary implementation or a public library.

The implementation of the Bluetooth stack used and its version may not be known to the client. It is usually not directly exposed either, so reverse engineering of the device's firmware may be necessary to identify which implementation of the Bluetooth stack is used.

Vulnerability enumeration can usually be done by searching vulnerability databases or general purpose search engines.


## Related resources

The following resources may be of interest for vulnerability enumeration:

{% include res_table.md resources='BSAM-RES-03' %}


## Example case

During the analysis of a vehicle multimedia unit, it is found that the operating system it runs is Android. Interacting with the device, in the `Settings > Device Info` section it is identified that the Android version it runs is Android 8.0.

From Android 4.2 onwards, the operating system includes its own Bluetooth stack. A search is performed in specialized databases and the following CVEs are found as possible vulnerabilities for our device:

* [https://nvd.nist.gov/vuln/detail/CVE-2017-0785](https://nvd.nist.gov/vuln/detail/CVE-2017-0785)
* [https://nvd.nist.gov/vuln/detail/CVE-2017-0781](https://nvd.nist.gov/vuln/detail/CVE-2017-0781)
* [https://nvd.nist.gov/vuln/detail/CVE-2017-13262](https://nvd.nist.gov/vuln/detail/CVE-2017-13262)
* [https://nvd.nist.gov/vuln/detail/CVE-2017-13261](https://nvd.nist.gov/vuln/detail/CVE-2017-13261)
* [https://nvd.nist.gov/vuln/detail/CVE-2017-13260](https://nvd.nist.gov/vuln/detail/CVE-2017-13260)
* [https://nvd.nist.gov/vuln/detail/CVE-2017-13258](https://nvd.nist.gov/vuln/detail/CVE-2017-13258)

For the listed CVEs, the following exploits are found:

* [https://www.exploit-db.com/exploits/44555](https://www.exploit-db.com/exploits/44555)
* [https://www.exploit-db.com/exploits/44554](https://www.exploit-db.com/exploits/44554)
* [https://www.exploit-db.com/exploits/44327](https://www.exploit-db.com/exploits/44327)
* [https://www.exploit-db.com/exploits/44326](https://www.exploit-db.com/exploits/44326)

After the execution of the exploits, it is verified that some of them still apply to the device. A recommendation is issued to upgrade the device to a newer Android version.
