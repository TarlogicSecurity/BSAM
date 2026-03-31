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

At the start of pairing, both devices exchange the information required to establish a shared key for secure future connections. Among the exchanged data are the input and output capabilities (IO capabilities), which indicate whether a device can show information on a screen, accept simple button presses, or allow the user to enter a code through a keypad. These capabilities help determine the safest and most suitable authentication method for the pairing process, depending on what each device can actually do.

Devices with greater input and output capabilities should use them to enable more secure pairing modes, since part of the security relies on user interaction to verify that the devices being paired are the intended ones. This verification, commonly through PIN confirmation, provides protection against MitM attacks.

Devices with limited IO capabilities cannot perform this verification and may be restricted to insecure methods such as “Just Works”, which does not require user interaction nor protects against MitM attacks. Therefore, devices should declare the highest IO capabilities available and avoid declaring none.


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
