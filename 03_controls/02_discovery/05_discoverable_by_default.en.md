---
layout: control
title: BSAM-DI-05
summary: Device discoverablility
description: Check that your Bluetooth device is not discoverable by default to reduce attack surface.
parent: Discovery
grand_parent: Controls
nav_order: 5
lang: en
page_id: cont_disco_05
permalink: controls/bluetooth-device-discovery/
tags:
- BR/EDR
- BLE
---

Bluetooth devices have the ability to be discoverable at all times, either because they actively advertise themselves by sending _advertising_ packets or because they respond to _inquiry_ type queries by indicating that they are available. This is for the convenience of the user, so that the devices automatically connect when they are nearby. 
It is a good practice that this discovery is not active by default and that it is only active on user demand, so that the user can activate and deactivate this state to avoid being detected and identified when it is not necessary, even penalizing the usability and user experience.

Discoverable devices by default allow the extraction of some of the information needed to be impersonated (MAC, Name, Supported Bluetooth Version...), as well as other relevant information. It is recommended to keep them non-discoverable as long as it is not necessary for pairing or connection.

Devices with input controls, such as buttons and keyboards, or similar items, should allow to change the discoverability status via these controls.


## Description

To fulfil the requirement it must be proven that the device is only discoverable by changing the state to discoverable and only for a limited time or until a connection is established or the state is manually deactivated.


## Related resources

To test the discoverability of the device, Bluetooth LE `beacons` or `Extended Inquiry Response` messages can be obtained using the following resources:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-07,BSAM-RES-08' %}


## Example case

Bluetooth headphones are turned on to carry out this analysis. Opening Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) allows the capture of packets for analysis. The "beacons" provide visibility of the device emitting them, in this case, the headphones.

![Wireshark BR/EDR Extended inquiry result]({{'/assets/img/bsam-di-05_extended_inquiry_result.png' | relative_url}})

The control _FAIL_ when the device is discoverable by default.

This behavior could be improved by making the headset not discoverable unless the user forces this mode. To achieve automatic connection of the headset, the headset would have to try to initiate a connection to the mobile device or computer to which it was previously connected.
