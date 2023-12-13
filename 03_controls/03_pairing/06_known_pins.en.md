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


One of the methods used during pairing to generate the shared key between devices involves sharing a randomly generated PIN code by one of the devices. From this PIN number, which the user must input, the link key is generated, making it crucial to prevent the generated number from being predictable. In some cases, such as car radios or Apple TVs, the PIN can be a fixed 4-digit number.

The randomness of the seeds used in cryptographic procedures to generate Bluetooth link keys is of paramount importance. When a fixed parameter, such as the PIN code, is used, the entropy of this input is reduced, leading to a decrease in the level of security and, as a result, weakening the protection of the device's link keys. This is because link keys can potentially be derived from this fixed PIN code, compromising the system's security.

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
