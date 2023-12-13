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



In Bluetooth it is necessary to establish a shared link key between two devices to subsequently authenticate them to each other or encrypt communications. The pairing procedure "introduces" the two devices to each other and establishes the shared key using different methods. Some Bluetooth devices use pairing methods using out-of-band data (OOB Data), which is data exchanged via other technologies, such as NFC or WiFi.

Through an OOB mechanism, identification data of each device is exchanged, which is necessary for the authentication process during pairing. This is equivalent to the manual exchange of PIN numbers.

Once the identification data has been exchanged, it is assumed that the pairing is protected against _MitM_ and that both devices are known to the user. However, an insecure OOB communication mechanism can lead an attacker to capture traffic and compromise the pairing. A suitable OOB communication method must have protection against _sniffing_.

The method of checking this control depends on the OOB mechanism used.


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
