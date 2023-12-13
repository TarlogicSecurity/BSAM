---
layout: control
title: BSAM-DI-03
summary: Generic device naming
description: Avoid using Bluetooth device names that reveal personal or device information.
parent: Discovery
grand_parent: Controls
nav_order: 3
lang: en
page_id: cont_disco_03
permalink: controls/bluetooth-device-name/
tags:
- BR/EDR
- BLE
---

Bluetooth devices can send publicly and without the need of authentication or authorization their name and or any other related data.

The Bluetooth name of a device can give information about the type of device, its user or include the MAC or ID itself. It is recommended that the device has a generic name and discloses the minimum necessary information.

Knowing who owns a Bluetooth device allows targeted attacks and leads to a privacy problem by being able to uniquely identify a device at a specific time and place.

## Description

The device name may appear during discovery, in BLE announcements or in `Inquiry` responses, or by requesting it with a `HCI_Remote_Name_Request` message.

In the first case, the name can simply be found during the discovery process, so any Bluetooth application will be useful to display it. However, some devices do not send the name in the discovery packets, so it will be necessary to actively request it.

For this second case, a connection is established with the device and the name is requested with the HCI command `HCI_Remote_Name_Request`.

The name obtained in this way must not contain data indicating the purpose of the device or personal data of its user.


## Related resources

To obtain the name in the manner described above, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-07,BSAM-RES-08' %}


## Example case

We will use Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) (btvs.exe -Mode wireshark) to capture packets for analysis.

Upon opening Wireshark, packets from nearby devices' advertisements are received. Some devices may include data in their advertisement packets, such as the _device name_ field value `L600474`.

![Wireshark LE Meta Device Name]({{ '/assets/img/bsam-di-03_august_device_name.png' | relative_url}})

The name indicates to a potential attacker the existence of this device and may even provide the device model.
