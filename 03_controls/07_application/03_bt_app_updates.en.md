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

Devices with Bluetooth capabilities rely on application‑layer software that manages and consumes Bluetooth service resources. To add new functionality or correct issues, these devices require a software update mechanism.

Because bugs or security vulnerabilities may arise in this application‑layer software throughout the device’s lifespan, whether on powerful platforms such as smartphones or on more constrained embedded systems, software updates are the only viable method to address them. Therefore, it is essential that the application‑layer software running on the device can be updated, otherwise, resolving identified issues would be impossible without recalling all deployed units.


## Description

The procedure consists on verifying that there is in place any update mechanism to update application software in the device under study.

Each manufacturer may choose to include different proprietary mechanisms.

This control is considered satisfactory when it is verified that the device supports remote software updates.

## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}
