---
layout: control
title: BSAM-DI-02
summary: Adequate device's signal
description: Check the range of your device's Bluetooth signal. This is important to reduce the attack surface and prevent an attacker from being able to access it from a greater distance than necessary.
parent: Discovery
grand_parent: Controls
nav_order: 2
lang: en
page_id: cont_disco_02
permalink: controls/bluetooth-signal-strenght/
tags:
- BR/EDR
- BLE
---


A Bluetooth device with a signal strength that allows interaction over long distances increases its exposure. For this reason, it is necessary to keep the signal strength of a Bluetooth device small, covering only the area necessary for its intended use.

This is also influenced by the environment, noise sources and the receiving antenna: depending on the signal-to-noise ratio (SNR) the Bluetooth device may allow connections at different distances.

To check the signal connection area, it will be necessary to describe the characteristics of the equipment and the environment in which the measurements are made, so that this is recorded in the evaluation of the result and can be compared with other tests.


## Description

To check the area reached by the signal, different measurements must be made:
* Measure the received strength (RSSI) at different distances from the transmitting device, without obstacles and noting the characteristics of the receiving device.
* Perform discovery tests at different distances from the transmitting device, without obstacles and noting the characteristics of the receiving device, until it is no longer detected.
* Perform connection tests at different distances from the transmitting device, without obstacles and noting the characteristics of the receiving device, until it is no longer detected.

The strength measurement (RSSI) is indicated in HCI events by the Bluetooth controller when a message is received from a remote device. It appears, for example, in the announcement messages issued in Bluetooth LE or in the `inquiry` responses in BR/EDR, so discovery messages can be used to take the measurement. The RSSI for a device can also be requested with the `HCI_Read_RSSI` message.

The RSSI parameter received at the same distance varies greatly between receiving devices, manufacturers and environments. Furthermore, RSSI alone does not determine the quality of detection either, as the signal-to-noise ratio must be taken into account.

In general, however, the lower limit is close to the value -120, beyond which the device will no longer be detected. Thus, by detecting the point at which an RSSI close to or below -120 is received, the maximum distance at which the device can be interacted with can be approximated.

Using the same method of device discovery, the distance beyond which the device is no longer detected can be measured and used as an approximation.

Connection tests can also be carried out at different distances, until it is no longer possible, and the maximum distance can be noted and used as a reference.


## Related resources

Resources that may be useful for estimating the maximum distance at which it is possible to communicate with the device:

{% include res_table.md resources='BSAM-RES-08' %}

Other resources that could help in the extraction of signal strength data are:

{% include res_table.md resources='BSAM-RES-05,BSAM-RES-07' %}


## Example case

We will use Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) to capture packets for analysis.

When Wireshark is opened, the computer initiates the scanning of discoverable devices. `Beacons` emit information about the device's availability. When this data is received, the controller adds the RSSI field, representing the signal intensity in dBm.

![Wireshark LE Meta RSSI]({{ '/assets/img/bsam-di-02_rssi.png' | relative_url}})

A Bluetooth headset broadcasting Bluetooth LE announcements and responding to `Inquiry` BR/EDR messages can be detected at a distance of 15 metres, with a received RSSI of about -115 at the furthest point, using the Cypress CYW920819WCD2 Bluetooth controller as receiving equipment with the integrated antenna of the evaluation kit itself.
With any computer or phone application you can measure the signal strength with which a device is emitting.

It is relevant for the study to know that reliable connections cannot be established more than 10 metres away from the device, therefore, a radius of approximately 10 metres is considered for interaction and 15 metres for detection.

Considering that this is a headset usually used in small enclosed spaces (within the same room) or with nearby devices, it is considered a suitable area for use.
