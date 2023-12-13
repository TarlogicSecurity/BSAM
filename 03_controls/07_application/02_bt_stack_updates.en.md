---
layout: control
title: BSAM-AP-02
summary: Bluetooth stack update
description: Verify that your Bluetooth device supports Bluetooth stack updates to fix errors and vulnerabilities.
parent: Application
grand_parent: Controls
nav_order: 2
lang: en
page_id: cont_app_02
permalink: controls/bluetooth-stack-update/
tags:
- BR/EDR
- BLE
---
The bluetooth standard evolves and from time to time a new version becomes available, so the devices can include mechanisms to update this bluetooth standard by a bluetooth stack with new capabilities and wich fixes security issues.

There is always a possibility that either bugs or security flaws will be found in a device's Bluetooth stack during the lifespan of the device. The only mechanism that can be used to solve those bugs and security flaws is via Bluetooth stack (a part of device's firmware) updates.

For the above reasons, it is fundamental to include a Bluetooth stack update or a full firmware update mechanism in a device, otherwise if a problem is found, it will be impossible to fix it without recalling all the devices in use.


## Description

The procedure consists on verifying that there is in place any update mechanism to update Bluetooth stack software in the device under study.

Each manufacturer may choose to include different proprietary mechanisms.

This control is considered satisfactory when it is verified that the device supports remote Bluetooth stack updates.

## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}
