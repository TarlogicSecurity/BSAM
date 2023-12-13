---
layout: control
title: BSAM-PA-01
summary: Pairable mode by default
description: Check that your Bluetooth device is not in pairing mode by default. Prevent an attacker from being able to pair with your device without your intervention
parent: Pairing
grand_parent: Controls
nav_order: 1
lang: en
page_id: cont_pair_01
permalink: controls/bluetooth-device-pairing-mode/
tags:
- BR/EDR
- BLE
---

The first step when using a Bluetooth device is pairing, at which time trust is established between the two to allow future connections. This pairing for security reasons must require user validation and supervision so that a new pairing attempt can validate or cancel the process.

Devices configured to answer to pairing requests without user intervention allow the extraction of part of the information necessary to be impersonated (MAC, Name, Bluetooth Version supported...). It is recommended to prevent a device from being paired with other devices if it is not necessary. Preferably, the pairing mode should require physical user intervention such as pressing a button.

A paired device can make requests and interact with the audited device and may therefore pose a security risk if the device contains sensitive information.


## Description

It must be proven that it is only possible to pair with the device by changing its status to pairable. The change of mode to pairable mode must require user intervention to be enabled. Pairable mode must be enabled for a limited time, until a pairing is performed, or the user manually deactivates the status.


## Related resources

To check if the device is pairable, a pairing process can be initiated with user tools or by using libraries such as Scapy. From the resources section, the following may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07' %}


## Example case

A Bluetooth headset is discoverable and pairable after it is turned on. During that time, another unpaired device can access information about these headsets through pairing requests without the user being notified.

We will use Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) to capture packets for analysis.

We are pairing headphones with the laptop, which initiates a new connection by requesting the input/output capabilities, 'IO Capability,' with the _IO Capability Request_ command:

![Wireshark IO Caps Request]({{ '/assets/img/bsam-pa-01_io_cap_request.png' | relative_url}})

The headphones, at that moment, allow the connection as they respond with 'IO Capability' using the _IO Capability Request Reply_ command:

![Wireshark IO Caps Request Reply]({{ '/assets/img/bsam-pa-01_io_cap_request_reply.png' | relative_url}})

The procedure culminates with the establishment of the connection, notified by the _Simple Pairing Complete_ command:

![Wireshark Simple Pairing]({{ '/assets/img/bsam-pa-01_simple_pairing_complete.png' | relative_url}})

The check control _FAIL_ because the device is pairable by default.
