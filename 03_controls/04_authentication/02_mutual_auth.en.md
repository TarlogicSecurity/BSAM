---
layout: control
title: BSAM-AU-02
summary: Mutual authentication
description: Check if your Bluetooth device supports mutual authentication. This is important to prevent impersonation attacks, in which a malicious device can impersonate another device
parent: Authentication
grand_parent: Controls
nav_order: 2
lang: en
page_id: cont_auth_02
permalink: controls/bluetooth-mutual-authentication/
tags:
- BR/EDR
---

During the Bluetooth authentication process, it is not necessary for the two devices involved to check each other's identity, which can lead to spoofing attacks, where a malicious device can impersonate another device.

The authentication process can be performed using the following methods:
 * `Legacy Authentication`: Performs authentication unilaterally, from master to slave, and may allow the master of a communication to be unauthenticated. It should be avoided as it allows spoofing attacks.
 * `Secure Authentication`: Requires authentication of both parties to the communication, preventing either party from being spoofed by a malicious device.


## Description

To check if the device supports `Legacy Authentication`, it is necessary to modify the LMP capabilities of the Bluetooth driver of the auditor. The LMP features indicate the functionalities supported by the Bluetooth driver. In particular, the "Secure Connections (Host Support)" and "Secure Connections (Controller Support)" capabilities indicate whether the host and the controller support Secure Connection type connections, which require the `Secure Authentication` authentication method.

To check whether the device can be forced to downgrade the authentication security to `Legacy Authentication`, set the bits corresponding to the `Secure Connections (Host Support)` and `Secure Connections (Controller Support)` capabilities to 0.

Depending on the controller manufacturer, this will be possible using a specific HCI message. In the case of the CYW920819EVB-02 development board, the manufacturer, Broadcom, allows writes to RAM through an HCI message, and, thanks to the PoC of the [BIAS](https://github.com/francozappa/bias) vulnerability, the location in memory of the bit strings corresponding to the LMP features is known, so we can overwrite them while ensuring that the device indicates that "Secure Connection" connections are not supported.

After modifying the driver capabilities, a connection to the audited device is initiated. If the device initiates authentication via `Legacy Authentication` the control is not enforced.


## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07,BSAM-RES-09' %}

## Example case

We will use Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) to capture packets for analysis.

To enable mutual authentication between two devices, one Central and the other Peripheral, it is necessary for the device's host to support secure connections. This is achieved by setting a value of `0x01` (`Secure_Connections_Host_Support is ‘enabled’. Host supports Secure Connections.`) in the _Secure\_Connections\_Host\_Support_ field of the _HCI\_Write\_Secure\_Connections\_Host\_Support_t command.

This packet should be identified in Wireshark captures, during the pairing process, between the Central and Peripheral devices to verify its value. The absence of this packet indicates that the devices did not configure the connection securely.

The check control _FAIL_ if the _HCI\_Write\_Secure\_Connections\_Host\_Support_ command is not found or if the value of the _Secure\_Connections\_Host\_Support_ field is different from `0x01`.