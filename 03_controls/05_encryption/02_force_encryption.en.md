---
layout: control
title: BSAM-EN-02
summary: Forced bluetooth encryption
description: Check that your Bluetooth device requires encryption. This is important to protect your data from sniffing and data extraction attacks
parent: Encryption
grand_parent: Controls
nav_order: 2
lang: en
page_id: cont_enc_02
permalink: controls/force-use-encryption-bluetooth/
tags:
- BR/EDR
- BLE
---


The use of encryption in Bluetooth connections is optional.

The use of encryption mechanisms is highly recommended for access to any service that exposes sensitive information or allows unauthorized control of the device.

Not using encryption exposes the data on a connection to sniffing attacks and allows the use of attacks such as BIAS to extract data from the device without sharing or knowing the link key.


## Description

To check if the use of encryption is required, the services that require confidentiality must first be classified.

Once the sensitive services have been listed according to their functionality, test connecting to them without having the link key.

This check will be considered successful if no data reading, access to functions or control of the device is allowed. 


## Related resources

To check this control, the following resources can be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07,BSAM-RES-09' %}

Other related documentation:

| ID               | Description                                                        |
|:-----------------|:-------------------------------------------------------------------|
| Documentation    | [Bluetooth LE connection mode]({% link 01_documentation/06_le_modes.en.md %})                                       |


## Example case

We will use Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) (btvs.exe -Mode wireshark) to capture packets for analysis.

The pairing between Bluetooth headphones and a computer is then verified. Connection is requested through the Bluetooth device management application integrated into the computer's operating system.
In the Wireshark capture, the complete pairing process should be identifiable, starting with a _Pairing Request_ command followed by the response command, _Pairing Response_, and concluding with the confirmation of pairing using the _Pairing Confirm_ command.

![Wireshark Pairing Process]({{ '/assets/img/bsam-en-02_pairing_process.png' | relative_url}})

In the _Pairing Request_ command, the _Secure Connections_ field should have the value `0x01`.

![Wireshark Pairing Request]({{ '/assets/img/bsam-en-02_pairing_request.png' | relative_url}})

In the _Pairing Response_ command, the _Secure Connections_ field should have the value `0x01`.

![Wireshark Pairing Response]({{ '/assets/img/bsam-en-02_pairing_response.png' | relative_url}})

This process forces encryption between both devices, establishing a generation of secure keys.

If, in the messages of _Pairing Request_ and _Pairing Response_ immediately preceding the _Pairing Confirm_, the _Secure Connections_ field has a value different from `0x01`, this control will have failed.

The check control _FAIL_ if either of the two devices has a Secure Connections field with a value of 0x00, and the connection is established.

