---
layout: control
title: BSAM-IG-04
summary: Known Bluetooth standard version vulnerabilities
description: How to identify vulnerabilities in the Bluetooth standard to ensure the security of your device.
parent: Information gathering
grand_parent: Controls
nav_order: 4
lang: en
page_id: cont_info_04
permalink: controls/bluetooth-standard-vulnerabilities/
tags:
- BR/EDR
- BLE
---


Throughout the history of the Bluetooth standard, security issues have been identified and have led to vulnerabilities. Devices with Bluetooth capabilities rarely implement only the latest version of the standard, instead, for compatibility reasons, support earlier versions that may have unresolved security issues.

As with vulnerabilities in the driver and stack, it's necessary to verify which vulnerabilities of the standard affect the analyzed device.

It should also be noted that the different Bluetooth specifications have a lifecycle announced by the _Bluetooth SIG_. It is important that the used Bluetooth specification version will be supported during the product lifecycle because if new vulnerabilities in the standard appear, they will not be fixed.


## Description

First, the version of the Bluetooth standard supported by the device must be identified. The implemented version of the standard can be obtained from different sources:

* Inspection of the packets of a connection at the `Link Layer` or link layer (`LMP` or `LLL`) for `LMP_VERSION_REQ`, `LMP_VERSION_RES` and `LLL_VERSION_IND` packets.
* Requesting version data from the device via the HCI `Read remote version` command.
* Device information inspection: devices like smartphones usually specify the Bluetooth version among their features.
* Searching to obtain the public information of the device: device models sometimes indicate the Bluetooth version they implement.
* Requesting data from the manufacturer.

It should be borne in mind that information provided on the Internet may be outdated and it's advisable to contrast it, whenever possible, with the information provided by the device.

Vulnerability enumeration can generally be done by searching in vulnerability databases or general-purpose search engines.

Checking the _Status_ of the specification version can commonly be found in the [list of Bluetooth SIG documents](https://www.bluetooth.com/specifications/specs/?types=specs-docs&keyword=core&filter=).

The most common specifications and their status as of May 2023 are listed below:

| Version                   | Status    | Deprecation   | Withdrawal    |
|:--------------------------|:----------|:--------------|:--------------|
| Core Specification 4.0    | Withdrawn | 28/01/2019    | 01/02/2022    |
| Core Specification 4.1    | Withdrawn | 28/01/2019    | 01/02/2023    |
| Core Specification 4.2    | Adopted   | 01/02/2026    | 01/02/2031    |
| Core Specification 5.0    | Adopted   | 01/02/2027    | 01/02/2032    |
| Core Specification 5.1    | Adopted   | 01/02/2029    | 01/02/2034    |
| Core Specification 5.2    | Adopted   | 01/02/2030    | 01/02/2035    |
| Core Specification 5.3    | Adopted   | 01/02/2032    | 01/02/2037    |
| Core Specification 5.4    | Adopted   | 01/02/2033    | 01/02/2038    |


## Related resources

Listed below are some resources that may be useful for obtaining the Bluetooth version supported by a Bluetooth device:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07' %}

For vulnerability enumeration the following resources may be of interest:

{% include res_table.md resources='BSAM-RES-03' %}


## Example case

A connection that established between our PC and a device is captured using the resource {% include res_link.md res='BSAM-RES-05' %}. This capture has been done with the debug modes of a Cypress card enabled following the {% include res_link.md res='BSAM-RES-06' %} technique. In this way, packets from the `Link Layer` are captured, in this case from BR/EDR.

Among the exchanged packets, an `LMP_VERSION_RES` packet is found.

![Wireshark dissected LMP_VERSION_RES packet]({{ '/assets/img/bsam-ig-04_lmp-version-res.png' | relative_url}})

The dissected packet has a `VersNr` field with value `0x0b`. By consulting the Bluetooth Assigned Numbers Rev. 2022-12-20 document in Section 2.1 (Core specification versions), it is concluded that the device under test supports Bluetooth v5.2. 

![Table of Bluetooth core specification names]({{ '/assets/img/bsam-ig-04_core-ver.png' | relative_url}})

A search following the resource {% include res_link.md res='BSAM-RES-03' %} finds three vulnerabilities against this version of the Bluetooth standard:

* [BIAS](https://francozappa.github.io/publication/bias/paper.pdf)
* [KNOB](https://knobattack.com/#vulnerable)
* [BlueTrust](https://www.tarlogic.com/es/blog/bluetrust-vulnerabilidad-bluetooth/)

For each of the vulnerabilities found, a proof of concept is executed. It is found that the analyzed device does not accept low entropy keys and is therefore considered not vulnerable to KNOB. However, the device allows authentication of devices with cloned identities and is therefore considered vulnerable to BIAS and BlueTrust. The controller manufacturer is notified of this security issue to obtain a firmware update to address the vulnerabilities found.


## External references

* Bluetooth Core V5.3, Vol. 6, Part B, Section 2.4.2.13 - LL_VERSION_IND
* Bluetooth Core V5.3, Vol. 2, Part C, Section 4.3.3 - LMP version
* Bluetooth Core V5.3, Vol. 4, Part E, Section 7.1.23 - Read Remote Version Information command
* Bluetooth Core V5.3, Vol. 4, Part E, Section 7.7.12 - Read Remote Version Information Complete event
* Bluetooth Assigned Numbers Rev. 2022-12-20, Section 2.1 - Core specification versions
