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

Bluetooth devices should provide firmware update capabilities for their controller or driver components. Bugs or security vulnerabilities may arise in the controller firmware, and the only viable method to correct them is through firmware updates.

For this reason, incorporating a controller firmware update mechanism is essential. Without it, any discovered flaw would be impossible to remediate without physically recalling or replacing all deployed devices.


## Description

The procedure consists on verifying that there is in place any update mechanism to update controller firmware in the device under study.

Each manufacturer may choose to include different proprietary mechanisms.

This control is considered satisfactory when it is verified that the device supports remote firmware updates.

## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}
