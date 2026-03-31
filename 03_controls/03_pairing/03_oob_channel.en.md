---
layout: control
title: BSAM-PA-03
summary: Bluetooth OOB channel security
description: Check that your Bluetooth device uses a secure OOB channel for pairing. This is important to prevent an attacker from being able to intercept the traffic and compromise the pairing
parent: Pairing
grand_parent: Controls
nav_order: 3
lang: en
page_id: cont_pair_03
permalink: controls/bluetooth-oob-channels/
tags:
- BR/EDR
- BLE
---

In Bluetooth, it is necessary to establish a shared link key between two devices to authenticate them and encrypt communications. Pairing introduces both devices to each other and generates this shared key through different methods. Some devices use out of band (OOB) data, exchanged through external technologies such as NFC or Wi Fi.

Through an OOB mechanism, each device exchanges the identification data required for authentication, functioning similarly to a manual PIN exchange. Once this data is exchanged, the pairing is considered protected against MitM attacks, assuming the OOB channel is secure. However, if the OOB communication method is not properly protected against sniffing, an attacker may capture the exchanged data and compromise the pairing. The verification method for this control depends on the specific OOB mechanism used.

## Description

A capture of a device's pairing/announcement data must be performed to verify that a device wishes to use OOB data for pairing.

If this is the case, it is necessary to analyze the external channel to be used for data exchange. During this analysis it must be checked that the channel used is confidential.

## Example case

A device can share, through an out-of-band (_OOB_) procedure, the necessary data for establishing a connection with another device. These data must conform to the format specified by the Bluetooth standard.

We will use Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) to capture packets for analysis.

When negotiating a connection, the _Pairing Request_ or _Pairing Response_ command has a field called OOB data, which can have two values:

| Value | Name              | Description                                                                           |
|:------|:------------------|:----------------------------------------------------------------------------------- --|
| 0x00  | Data Not Present  | No OOB data available                                                                 |
| 0x01  | Data Present      | OOB data is available and must be considered in the communication or pairing process. |

![Wireshark OOB data not present]({{ '/assets/img/bsam-pa-03_oob_data_no_present.png' | relative_url}})

When the device has a value of 0x01 (Data Present), the OOB data exchange mechanism must be audited to ensure that it is carried out securely, usually via WiFi or RFID.

## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-08' %}
