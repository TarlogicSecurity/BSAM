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

The Bluetooth standard evolves over time, introducing new versions that add capabilities and address security issues. For this reason, devices should include mechanisms to update the Bluetooth stack, which forms part of the device’s firmware.

Bugs or security vulnerabilities may emerge in the Bluetooth stack during the lifespan of a device, and software based stack updates are the way to correct them. Therefore, incorporating a Bluetooth stack update mechanism is essential. Without it, any identified issue would be impossible to fix without recalling all deployed devices.

## Description

The procedure consists on verifying that there is in place any update mechanism to update Bluetooth stack software in the device under study.

Each manufacturer may choose to include different proprietary mechanisms.

This control is considered satisfactory when it is verified that the device supports remote Bluetooth stack updates.

## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}
