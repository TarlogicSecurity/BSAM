---
layout: control
title: BSAM-DI-04
summary: Sensitive data exposure
description: Bluetooth devices can expose sensitive data in discovery messages. Analyze these data to prevent personal information from being exposed.
parent: Discovery
grand_parent: Controls
nav_order: 4
lang: en
page_id: cont_disco_04
permalink: controls/bluetooth-data-exposure/
tags:
- BR/EDR
- BLE
---


Inside the _advertising_ packets in BLE and some extended _inquiry_ responses in BR/EDR may contain additional data added by the manufacturer.

Some of the discovery messages contain _Manufacturer Specific Data_ that may contain sensitive information and should be analyzed carefully.

This data should be analyzed for sensitive information and verify that unnecessary data is not being exposed.

## Description

By capturing the messages issued during the discovery phase, via LE beacons or in the inquiry responses in BR/EDR, data of interest can be found, such as the services the device exposes, the name or manufacturer data.

In LE beacons, manufacturer data is exposed in `AdvData` fields, while in BR/EDR, it is exposed in `Extended Inquiry Result` messages. In both cases the field is of type `Manufacturer Specific Data`, with id `0xFF`, and a manufacturer identifier is given which can be looked up in the Bluetooth Assigned Numbers document.

The content of the Manufacturer Specific Data type is in a manufacturer-dependent format that may not be publicly available and may require a reverse engineering effort to decode.

Some popular formats for this field are the following:

| Eddystone | <https://github.com/google/eddystone> |
| Microsoft | <https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-cdp/77b446d0-8cea-4821-ad21-fabdf4d9a569?redirectedfrom=MSDN> |
| iBeacon   | <https://en.wikipedia.org/wiki/IBeacon> |


## Related resources

To obtain Bluetooth LE `beacons` or `Extended Inquiry Response` messages, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-08' %}


## Example case

A Samsung Galaxy Buds2 headset, during discovery, emits two different LE `beacons` and responds to `inquiry` requests with an `Extended Inquiry Result` message. We will use Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) to capture packets for analysis.

When Wireshark is opened, the computer initiates the scanning of discoverable devices. `Beacons` publish device information in different formats. The first one indicates as Samsung manufacturer ID and part of the device name can be read:

![Beacon with Samsung data]({{ '/assets/img/bsam-di-04_samsung_beacon.png' | relative_url}})

The second shows a Microsoft manufacturer ID:

![Beacon with Microsoft data]({{ '/assets/img/bsam-di-04_microsoft_beacon.png' | relative_url}})

The `Extended Inquiry Result` message contains different data with the Samsung vendor ID, as well as two listings of `UUIDs` corresponding to the device services:

![Extended Inquiry Result con datos de Samsung]({{ '/assets/img/bsam-di-04_samsung_eir.png' | relative_url}})

In all three cases, manufacturer data is found with unknown encoding, but shows data on the device name (Buds2) and its utility ([Headphones]).

The exposed services, on the other hand, indicate 5 services with non-standard UUIDs that expose the manufacturer's own services. This information may be useful to the auditor in later phases of the analysis and it is preferable to avoid exposing services unnecessarily.

As this information should only be accessible in a more limited context it is interesting to highlight in the report possible solutions to this assessment. A recommendation is issued to only make the device name and services accessible during pairing mode or to another authenticated device. None of the services should be accessible to unauthenticated devices.
