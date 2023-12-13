---
layout: control
title: BSAM-DI-01
summary: Operation modes (BR/EDR and BLE)
description: Check that your device's Bluetooth operating modes are necessary. This is important to reduce the attack surface and improve battery life.
parent: Discovery
grand_parent: Controls
nav_order: 1
lang: en
page_id: cont_disco_01
permalink: controls/bluetooth-operation-mode/
tags:
- BR/EDR
- BLE
---


Bluetooth has multiple transport/operation modes and a device can support one or more at the same time. Among the most common are BR/EDR (also known as Bluetooth Classic) and Bluetooth Low Energy (BLE), less common AMP.

Enabling unnecessary operation modes increases the attack surface of the device. Limiting the exposed capabilities to the minimum necessary is a good security practice, so it is advisable to check that the possible modes of operation on the device match the required ones.


## Description

To verify that the modes of operation are appropriate for the intended use of the device, it is necessary to discover which modes are enabled.
This can be done in multiple ways:

* Using Bluetooth device management tools. You could use `bluetoothctl` in Linux, you can start a discovery procedure, first for BR/EDR and then for BLE. This would allow you to find out if a device is advertised in both modes or only in one of them. Some tools may not allow this differentiation or may not indicate by which mode they have discovered a device.
* Using lower level frameworks and tools such as `Scapy` by sending out `HCI Inquiry` requests and capturing responses for BR/EDR and for capturing advertisements (`Advertisments`) for LE.
* By capturing and analyzing the traffic of a BR/EDR LMP feature request where the `BR/EDR Not supported`, `LE supported` and `Simultaneous LE and BR/EDR to Same Device Capable` fields indicating the modes of operation enabled on this device are present.

If the device enables more than one Bluetooth operating mode, possible reasons for this must be provided. The following table lists some of the restrictions for each mode of operation.

| Mode              | BR/EDR            | LE                                | AMP                                                                                       |
|:------------------|:------------------|:----------------------------------|:------------------------------------------------------------------------------------------|
| Speed             | Up to 3 Mb/s[^1]  | Up to 2 Mb/s[^1]                  | Depends on the selected PHY/MAC                                                           |
| Consumption       | 1W[^2]            | From 0.01W a 0.5W[^2]             | Depends on the selected PHY/MAC                                                           |
| Topology          | Point to point    | Point to point, Broadcast, Mesh   | Depends on the selected PHY/MAC                                                           |
| Common uses       | Audio             | Audio, location, networking       | Special applications where a custom physical channel or higher bandwidths are necessary   |


It must be assessed whether the uses of the device require each of the listed modes or whether it would be possible for the device to forego any of them without an impact on functionality.


## Related resources

Resources that may be useful for the recognition of enabled modes of operation:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-08' %}


## Example case

A wireless headset is audited. To do so, a capture is made with `Acrylic Bluetooth LE Analyzer` while our device performs a discovery.

![LE beacon with data about supported operating modes]({{ '/assets/img/bsam-di-01_adv_modes.png' | relative_url}})

In the screenshot you can see a Bluetooth LE announcement packet coming from the audited device. This announcement packet contains information about the supported operating modes in its `Advertising Data > Flags` field. In these fields it is specified that simultaneous BR/EDR and LE operation is supported (note the double negation in `BR/EDR not supported: false`).

This device can therefore operate in BR/EDR and LE mode simultaneously.

By consulting the specifications of the device it is found that its intended use is audio playback. This functionality is provided for in both BR/EDR and LE. Looking at the features and technical specifications it can be found that the headset supports SBC, AAC and Samsung Scalable Codec. For any of the 3 codecs the highest bitrate supported is 512Kbps. If the possible compression of the codec used is disregarded and this bitrate value is taken, both BR/EDR and LE should be able to support this bandwidth. Finally, the headphones are intended for a point-to-point connection and not for multipoint connections, so again, both modes allow this functionality.

From the above study, it is concluded that it is not necessary to enable both modes simultaneously. Enabling one of them would be sufficient to achieve all the current functionality of the device. For this reason, enabling both simultaneously increases the attack surface of the device. In this case it is recommended to disable BR/EDR mode as it offers worse battery consumption compared to LE mode.

------

[^1]: Value dependent on the version of Bluetooth used.
[^2]: Consumption value for a standard use case used as reference. This value depends on the scenario, use case and conditions under which the data transmission takes place.
