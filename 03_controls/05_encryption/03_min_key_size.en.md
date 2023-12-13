---
layout: control
title: BSAM-EN-03
summary: Minimum encryption key size
description: Check that your Bluetooth device uses a minimum encryption key size. This is important to prevent an attacker from being able to decrypt your communications using a brute-force attack
parent: Encryption
grand_parent: Controls
nav_order: 3
lang: en
page_id: cont_enc_03
permalink: controls/minimum-size-bluetooth-encryption-key/
tags:
- BR/EDR
- BLE
---


The temporary encryption keys of a Bluetooth connection can have different levels of entropy. To decide how secure a temporary encryption key for a Bluetooth connection is, the number of entropy bytes it will have is negotiated during the start of the encryption process.

Small keys, below 7 bytes, can be discovered by brute force attacks, breaching the confidentiality of communications.


## Description

For verification of the minimum key size accepted by the device an encrypted connection attempt has to be made by imposing a maximum key size limitation. There is a [proof of concept of the KNOB attack](https://github.com/francozappa/knob) that allows this.

Thus, when the key entropy negotiation process is initiated, we will attempt to negotiate 1 byte of entropy to which the device must respond with its minimum key size.

In some cases, it will be necessary to make multiple connection attempts with different maximum key sizes to find out the minimum key size that allows already instead of negotiating a key size, closes the connection.

It is recommended to force the use of the maximum key size supported by the Bluetooth standard: 16 bytes.

This control will be considered satisfactory if the minimum encryption key size is at least 7 bytes.

## Related resources

To check this control, the following resources can be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07,BSAM-RES-09' %}

## Example case

The encryption key length of the pairing between a computer and Bluetooth headphones will be verified. We will use Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) to capture packets for analysis. The connection is initiated through the integrated Bluetooth device management application in the operating system.

In the Wireshark capture, the pairing process is identified with a _Pairing Request_ command, followed by the _Pairing Response_ command, and the pairing is confirmed with the _Pairing Confirm_ command.

![Wireshark Pairing Process]({{ '/assets/img/bsam-en-02_pairing_process.png' | relative_url}})

In the _Pairing Request_ and _Pairing Response_ commands, there is a field called _Maximum Encryption Key Size_, indicating the maximum length of the encryption key in bytes. This field should have a length between 7 and 16 bytes according to the Bluetooth standard.

In the _Pairing Request_ command from the laptop, a value of `16` bytes is shared in the _Maximum Encryption Key Size_ field.

![Wireshark Pairing Request]({{ '/assets/img/bsam-en-02_pairing_request.png' | relative_url}})

In the _Pairing Request_ command from the headphones, a value of `16` bytes is shared in the _Maximum Encryption Key Size_ field.

![Wireshark Pairing Response]({{ '/assets/img/bsam-en-02_pairing_response.png' | relative_url}})

The _Pairing Request_ and _Secure Connections_ commands should always have a Maximum Encryption Key Size value greater than 6 to pass the check. However, it is recommended not to use values lower than 16.

If any device requests a key length below 7, the other device participating in the pairing must cancel the connection.

The check control _FAIL_ if either of the two devices has a Maximum Encryption Key Size field with a value lower than 7. If this happens and the connection is established between both devices, they will not pass this check.