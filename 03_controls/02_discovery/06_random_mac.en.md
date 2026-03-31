---
layout: control
title: BSAM-DI-06
summary: Use random MAC address
description: check that your Bluetooth device use a random MAC address in the discovery messages.
parent: Discovery
grand_parent: Controls
nav_order: 6
lang: en
page_id: cont_disco_06
permalink: controls/bluetooth-random-mac/
tags:
- BR/EDR
- BLE
---

In Bluetooth, two main types of MAC addresses are used:

* Public: A fixed and unchangeable identifier assigned to the device.
* Random: A changeable, pseudorandom generated identifier.

Random MAC addresses are divided into two categories:

* Resolvable: Allow derivation of the Public MAC Address using an Identity Resolving Key (IRK), if available.
* Non resolvable: Purely random and cannot be linked back to the device’s Public MAC Address.

Using Random MAC Addresses during device discovery is essential to mitigate device tracking and prevent identity spoofing attacks.

Also, although a device may claim to use random MAC addresses, it is important to verify that the address actually changes over time, as a static or rarely rotated “random” MAC fails to provide meaningful privacy protection.

## Description

During the advertising process, it's necessary to verify if the device is generating a random address by inspecting the corresponding field in the HCI_LE_Extended_Advertising_Report message. The Address_Type[i] field can take on the following values:

| Value | Parameter Description |
|:------|:--------------------------|
| 0x00 | Public Device Address |
| 0x01 | Random Device Address |
| 0x02 | Public Identity Address (corresponds to _Resolved Private Address_) |
| 0x03 | Random (static) Identity Address (corresponds to _Resolved Private Address_) |
| 0xFF | No Address provided (anonymous advertisement) |
| All other values | Reserved for future use |

This check is considered satisfactory when the value in the field Address_Type is 1, 2, or 3.

## Example case

Opening Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode Wireshark)  allows reading the discovery messages of nearby devices. One of the tags shown in the advertisement packets is the type of MAC address, indicated by the _Address Type_ field, in this case with a value of `0x00` (`Public Device Address`).

![Wireshark LE Meta Address Type]({{ '/assets/img/bsam-di-06_august.png' | relative_url}})

The MAC address type is specified according to the Bluetooth standard. In general terms, the obtained value will be in the range of 0x00 to 0x03.

The check control _FAIL_ when the value is 0x00.

## External references

* Bluetooth Core V5.3, Vol. 3, Part C, Section 10.8 Random Device Address
* Bluetooth Core V5.3, Vol. 1, Part A, Section 2.1.1.3 Security Manager Protocol
* Bluetooth Core V5.3, Vol. 4, Part H, Section 7.7.65.13 LE Extended Advertising Report event
