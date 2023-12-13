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

A MAC address (Media Access Control) is a unique identifier assigned to a device's network interface for identification on a device network. These addresses are used in communication networks that utilize protocols from the IEEE 802 family, including Bluetooth (IEEE 802.15.1).

In Bluetooth, there are two types of MAC addresses:

* _Public MAC Address_: This address is the actual and fixed device identification, which cannot be changed.
* _Random MAC Address_: This address is randomly generated to represent the device and can be altered.

Within the group of _Random MAC Addresses_, there are two subgroups: those that allow connection to the device and those that do not.

The Random _MAC Addresses_ that permit connection and access to a set of Bluetooth services do not necessarily need to be resolved against the _Public MAC Address_. However, it is possible to calculate the _Public MAC Address_ from a _Random MAC Address_.

The generation of _Random MAC Addresses_ during device discovery is essential to prevent attacks such as `BlueTrust`.

__Note__: There is a similarity between the real _MAC address_ in Wi-Fi and the Public MAC Address in Bluetooth, just as there is a similarity between the _random MAC address_ in Wi-Fi and _random MAC Address_ in Bluetooth.

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
