---
layout: control
title: BSAM-PA-06
summary: Known Pin Codes
description: Check that your Bluetooth device is not fixed PIN code. This is important to prevent an attacker from being able to decrypt the PIN code and take control of the pairing process
parent: Pairing
grand_parent: Controls
nav_order: 6
lang: en
page_id: cont_pair_06
permalink: controls/bluetooth-known-pin-codes/
tags:
- BR/EDR
- BLE
---

One of the pairing methods used to generate the shared key between devices involves sharing a randomly generated PIN code, which the user must enter. Since the link key is derived from this PIN, it is essential that the PIN is not predictable.

The randomness of the seeds used in cryptographic procedures is critical for securing Bluetooth link keys. In some devices, the PIN may be a fixed 4 digit number. When fixed parameters like static PIN codes are used, the entropy of the input decreases, reducing the security level. As a result, link keys may be derived from this predictable value, compromising the overall security of the system.

## Description

To verify the validity of the control, multiple pairings should be performed to collect data that allows the detection of patterns in known fixed PIN numbers.

The task to be performed consists of verifying that the following is not met:

* Constant PIN number: the same PIN number is used for each pairing, which is not secure for ensuring protection against MITM attacks.

Control is met it the received PIN number are not repeated.

## Related resources

Some resources related to this control are the following:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-07' %}

## Example case

To evaluate the security of the system PIN generation, multiple pairing processes are initiated with a cell phone by capturing them using the technique {% include res_link.md res='BSAM-RES-05' %}. During this process it is evident that the device consistently displays a fixed PIN with the value `0000`.

Therefore, this device does not overcome the control since the generation of PIN codes is predictable, so it is possible to take control of a pairing process. 
