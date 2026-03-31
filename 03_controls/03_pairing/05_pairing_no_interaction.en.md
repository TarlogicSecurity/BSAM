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

In Bluetooth, a shared link key must be established between two devices to encrypt communications. The pairing procedure introduces both devices to each other and generates this shared key. Different pairing methods exist, and some do not include mechanisms to verify the identity of the devices involved.

Pairing essentially functions as a form of manual authentication, where the user confirms that the two devices attempting to connect are indeed the intended ones, whether by entering a PIN, comparing a displayed code, or explicitly accepting or rejecting a pairing request. When this user‑driven validation is absent or weak, the risk of connecting to an unauthorized device increases significantly.

The Just Works method performs pairing without user interaction and does not provide any mechanism for the user to verify the identity of the remote device. This means that any nearby device can initiate pairing and establish a valid shared key without the user’s knowledge or approval. As a result, an attacker within range can still pair with the device and gain unauthorized access to device services.


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

In this example, a pairing test with wireless headphones is demonstrated.

For the test, Wireshark has been used with BTVS (check out the [resources section](https://www.tarlogic.com/bsam/resources/capture-bluetooth-connection/)) to capture and analyze packets. To perform the connection and pairing, the Bluetooth device configuration tool of the laptop's operating system is used.

The headphones use Bluetooth Classic, where most of the pairing process occurs in the controller and is not visible from the host. However, in this case, it is sufficient to observe the exchange of HCI messages, where the _IO Capability_ values ​​appear.

The first HCI messages of the pairing process between the two devices are the _IO Capability Request_ event, where the controller requests the local host's _IO Capabilities_, and the _IO Capability Request Reply_ command, where the host responds. These messages are not relevant for the current example.

Later, the _IO Capability Response_ message is observed, where the headphones' _IO Capabilities_ appear. In this case, it is observed that the value is 0x03 (NoInputNoOutput).

![Wireshark Pairing IO Capabilities]({{ 'assets/img/bsam-pa-05_io_cap_response.png' | relative_url}})

This value does not allow pairing to be done with user interaction. In the capture, it is observed that the pairing is indeed completed successfully. Pairing is confirmed through the Simple Pairing Complete command.

![Wireshark Simple Pairing]({{ '/assets/img/bsam-pa-05_pairing_no_user_interaction.png' | relative_url}})

The input/output capabilities of the headphones have allowed pairing without user confirmation or notification.

The control result will be _FAIL_ when a device is pairable without user intervention.

The _Just Works_ pairing mechanism should be avoided. In this case, the device had buttons on both headphones and can be used as Yes/No buttons, thus confirming the pairing.

Full capture: [Galaxy Buds Pairing]({{ 'assets/captures/GalaxyBudsPairing.pcapng' | relative_url}})

## External references

* Bluetooth Core V5.3, Vol. 3, Part C, Section 5.2.2.4 - IO capabilities
* Bluetooth Core V5.3, Vol. 3, Part C, Section 5.2.2.5 - Mapping of input / output capabilities to IO capability
* Bluetooth Core V5.3, Vol. 3, Part C, Section 5.2.2.6 - IO and OOB capability mapping to authentication stage 1 method
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.2 - IO capabilities
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.1 - Selecting key generation method
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.1 Pairing Methods
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.2 LE Legacy pairing - Just Works
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.4 Out Of Band
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.6.2 Authentication Stage 1 - Just Works or Numeric Comparison
