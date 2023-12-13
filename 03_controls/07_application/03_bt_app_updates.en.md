---
layout: control
title: BSAM-AP-03
summary: Application software update
description: Check that your Bluetooth device has a mechanism to update the application software. This is important to fix bugs and security vulnerabilities that may be found
parent: Application
grand_parent: Controls
nav_order: 3
lang: en
page_id: cont_app_03
permalink: controls/bluetooth-software-update/
tags:
- BR/EDR
- BLE
---

Devices with Bluetooth capabilities require tools and applications that make use of these communications. If new functionalities are to be added or problems corrected, these devices must have an update mechanism.

There is always a possibility that either bugs or security flaws will be found in a device's application software during thea device's lifespan. These applications can refer to applications on devices like smartphones, but they can also include applications that consume Bluetooth resources on not so powerful embedded systems, such as applications developed on open hardware and software platforms. The only mechanism that can be used to solve those bugs and security flaws is via software updates.

For the above reasons, it is fundamental to include an application software update mechanism  that consumes Bluetooth controller resources on a device, otherwise if a problem is found, it will be impossible to fix it without recalling all the devices in use.


## Description

The procedure consists on verifying that there is in place any update mechanism to update application software in the device under study.

Each manufacturer may choose to include different proprietary mechanisms.

This control is considered satisfactory when it is verified that the device supports remote software updates.

## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}
