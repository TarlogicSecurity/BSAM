---
layout: control
title: BSAM-PA-02
summary: Input and output capabilities
description: Bluetooth devices must declare their maximum I/O capabilities to prevent MitM attacks.
parent: Pairing
grand_parent: Controls
nav_order: 2
lang: en
page_id: cont_pair_02
permalink: controls/bluetooth-input-output-capabilities/
tags:
- BR/EDR
- BLE
---


At the start of a pairing, both devices exchange information necessary to establish a shared key and securely initiate future connections. Among the data exchanged are the input and output capabilities, or _IO Capabilities_, which specify the communication capabilities of the device: whether it can display information, to receive binary input (yes or no) or by keyboard.

The _IO Capabilities_ (input and output capabilities) provided by a device are related to the pairing methods allowed by the device, so devices with higher input and output capabilities should make use of them to enable more secure pairing modes. 

This is because part of the security during pairing depends on user interaction, which must verify that both paired devices are the ones intended to be paired, and not a nearby malicious device. This protection against MitM attacks during pairing is usually done by having the user verify a pin on both devices.

Limited input and output capabilities may make it difficult or impossible to apply this protection. For example, a device without any input or output can only pair with another device without any user interaction. However, a device with a keypad and a display allows the user to enter a PIN shared with the other device during pairing.

Generally, all devices have some input or output method available that they can use to enhance the security of the pairing, so the highest possible input and output capabilities should be declared for the device. In any case, declaring that the device does not have any input and output capabilities should always be avoided, since, in that case it is only possible to use the "just works" pairing method, which does not allow user interaction and does not provide MitM protection.

If the pairing is not secure, the shared key is not secure, and all Bluetooth security features are affected.


## Description

Information about supported input and output capabilities is exchanged between the devices when pairing, so this information must be captured during this process.

This can be done by initiating a pairing with a user tool, such as bluetoothctl, and capturing the connection with Wireshark. Another method is to use a Bluetooth packet sending library to simulate the initiation of a pairing and terminate the connection when the `IO Capabilities Response` message, which contains the necessary information, is received.

The message contains the `IO Capability` field with one of the following values:


| Value | Name            | Description                                                                                     |
|:------|:----------------|:-------------------------------------------------------------------------------------------------
| 0x00  | DisplayOnly     | It is capable of displaying information on a screen but cannot receive inputs.                  |
| 0x01  | DisplayYesNo    | It can display information and/or yes/no questions, allowing for limited interaction.           |
| 0x02  | KeyboardOnly    | It can receive input through a keyboard (e.g., entering a PIN during pairing).                  |
| 0x03  | NoInputNoOutput | It has no means to display information or receive input from, for example, keyboards or buttons.|
| 0x04  | KeyboardDisplay | It can receive input through a keyboard and it is capable of displaying information.            |

The values are used to determine which pairing method will be used with the device, so it should be the value with the highest possible capabilities. Therefore, it should be checked that the capabilities set in software are the highest that the device hardware allows.


## Related resources

To discover the _IO Capability_ and related values of a device, a pairing can be forced, and the results captured using the following resources:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07' %}


## Example case

We will use Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs)BTVS (btvs.exe -Mode Wireshark) to capture packets for the analysis of pairing between a laptop and the Samsung Galaxy Buds 2 Bluetooth Classic headphones and the Sony SP-WI600N BLE headphones.


For Bluetooth Classic, observe the _IO Capability_ field can be found in the _IO Capability Request_ and _IO Capabilitiy Response_ commands.
The provided example illustrates an _IO Capability Response_ command with _IO Capability_ field value of `0x03` (`NoInputNoOutput`).

![Wireshark Pairing IO Capabilities Classic]({{ 'assets/img/bsam-pa-02_pairing_io_caps.png' | relative_url}})

Moving to Bluetooth Low Energy, find the _IO Capability_ field in the _Pairing Request_ and _Pairing Response_ commands.
The given example displays a _Pairing Request_ command with _IO Capabilitiy_ field value of `0x03` (`NoInputNoOutput`).

![Wireshark Pairing IO Capabilities BLE]({{ '/assets/img/bsam-pa-02_pairing_io_caps_le.png' | relative_url}})

The control _FAIL_ when a device does not have the value `0x04` (`KeyboardDisplay`).

The auditor must ensure that the declared features match those present in the physical device.


## External references

* Bluetooth Core V5.3, Vol. 3, Part C, Section 5.2.2.4 - IO capabilities
* Bluetooth Core V5.3, Vol. 3, Part C, Section 5.2.2.5 - Mapping of input / output capabilities to IO capability
* Bluetooth Core V5.3, Vol. 3, Part C, Section 5.2.2.6 - IO and OOB capability mapping to authentication stage 1 method
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.2 - IO capabilities
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.1 - Selecting key generation method
