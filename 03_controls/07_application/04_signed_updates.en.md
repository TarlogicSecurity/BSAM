---
layout: control
title: BSAM-AP-04
summary: Digitally signed updates
description: Digitally signed updates - Verify if updates are digitally signed to prevent malicious software
parent: Application
grand_parent: Controls
nav_order: 4
lang: en
page_id: cont_app_04
permalink: controls/digital-signature-bluetooth-updates/
tags:
- BR/EDR
- BLE
---


For each of the available update mechanisms in a device, a check to verify if updates are digitally signed is needed.

If a device uses and accepts non-signed updates or software, then, it is possible to craft custom software pieces, firmwares and applications that may access data or perform malicious actions by using the device in ways non-intended by the manufacturer and user.


## Description

In order to check wether updates are digitally signed, multiple techniques may be used:
* Source code review.
* Sligtly modifying random bytes of a valid update file and trying to use the modifyed software. Beware that this may result in a bricked device if the software is finnaly installed.

This control is considered satisfactory when it is verified that the device does not remotely accept minimally modified updates.