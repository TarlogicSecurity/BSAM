---
layout: control
title: BSAM-PA-05
summary: Pairing without user interaction
description: Prevent your Bluetooth device from automatically pairing to protect yourself from attacks.
parent: Pairing
grand_parent: Controls
nav_order: 5
lang: en
page_id: cont_pair_05
permalink: controls/bluetooth-pairing-without-interaction/
tags:
- BR/EDR
- BLE
---


In Bluetooth, it is necessary to establish a shared link key between two devices to encrypt communications. The pairing procedure "introduces" both devices to each other and establishes the shared key. There are different methods to establish the shared key, and some of these do not implement security controls to verify the identity of the involved devices.

The _just works_ method allows pairing using a numerical key, which is done without user interaction, and does not implement measures to ensure that the devices are known to the user. All the cryptographic material used for a _just works_ connection during the pairing process is sent over the Bluetooth band without any encryption, making it possible to capture this information with a sniffer and impersonate the device.

Additionally, if the pairing mode is _legacy,_ it is susceptible to man-in-the-middle (mitm) attacks.


## Description

The following two elements must be checked to verify that the control is satisfied:
 * The "Authentication requirements" field of the "IO Capability Response" message must show a value with MITM protection.
 * The device shall reject any pairing attempt that requires the "just works" method.

The "IO Capability Response" message, which contains the "Authentication Requirements" field, can be captured during a pairing with a user tool or by using Scapy to simulate a partial pairing.

To verify that it is not possible to pair using the "just works" method, one can first check what capabilities the audited device has, and secondly, configure a local device with the necessary capabilities to downgrade the pairing method to "just works".

The control is only fulfilled when using a value for "Authentication Requirements" that includes MITM protection and it is not possible to pair with "just works".

## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-07,BSAM-RES-09' %}


## Example case

We will use Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) to capture and analyze the packets.

The Bluetooth device configuration tool of the laptop's operating system is used to initiate the connection. In Bluetooth Classic, the connection between the two devices starts with the _IO Capability Request_ command.

![Wireshark IO Caps Request]({{ '/assets/img/bsam-pa-01_io_cap_request.png' | relative_url}})

The headphones respond with the _IO Capability Request Reply_ command with _IO Capability_ 0x03 (NoInputNoOutput). This allows pairings without user intervention.

![Wireshark IO Caps Request Reply]({{ '/assets/img/bsam-pa-01_io_cap_request_reply.png' | relative_url}})

Pairing is confirmed a few messages later with the _Simple Pairing Complete_ command.

![Wireshark Simple Pairing]({{ '/assets/img/bsam-pa-05_pairing_no_user_interaction.png' | relative_url}})

The input/output capabilities of the headphones have allowed pairing without user confirmation or notification.

The check control _FAIL_ when a device is pairable without user intervention.

The _Just Works_ pairing method should be avoided. In this case, the device had buttons on both headphones that can be utilized as Yes/No buttons, thus providing a means for pairing confirmation.

## External references

* Bluetooth Core V5.3, Vol. 3, Part C, Section 5.2.2.4 - IO capabilities
* Bluetooth Core V5.3, Vol. 3, Part C, Section 5.2.2.5 - Mapping of input / output capabilities to IO capability
* Bluetooth Core V5.3, Vol. 3, Part C, Section 5.2.2.6 - IO and OOB capability mapping to authentication stage 1 method
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.2 - IO capabilities
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.1 - Selecting key generation method
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.1 Pairing Methods
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.2 LE Legacy pairing - Just Works
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.4 Out Of Band
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.6.2 Aunthenticacion Stage 1 - Just Works or Numeric Comparison