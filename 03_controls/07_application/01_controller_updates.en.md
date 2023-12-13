---
layout: control
title: BSAM-AP-01
summary: Controller firmware update
description: Check that your Bluetooth device has a mechanism to update the controller firmware. This is important to fix bugs and security vulnerabilities that may be found
parent: Application
grand_parent: Controls
nav_order: 1
lang: en
page_id: cont_app_01
permalink: controls/bluetooth-controller-firmware-update/
tags:
- BR/EDR
- BLE
---

Bluetooth devices may have driver firmware update capabilities.

There is always a possibility that either bugs or security flaws will be found in a device's controller firmware. The only mechanism that can be used to solve those bugs and security flaws is via firmware updates.

For the above reasons, it is fundamental to include a controller firmware update mechanism in a device, otherwise if a problem is found,  it will be impossible to fix it without recalling all the devices in use.


## Description

The procedure consists on verifying that there is in place any update mechanism to update controller firmware in the device under study.

Each manufacturer may choose to include different proprietary mechanisms.

This control is considered satisfactory when it is verified that the device supports remote firmware updates.

## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}
