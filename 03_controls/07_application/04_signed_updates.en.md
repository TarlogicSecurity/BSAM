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

For each update mechanism available on a device, it is necessary to verify that the updates are digitally signed. If a device accepts unsigned firmware or software, an attacker could create custom components capable of accessing data or performing malicious actions, using the device in ways not intended by the manufacturer or the user.

## Description

In order to check wether updates are digitally signed, multiple techniques may be used:
* Source code review.
* Sligtly modifying random bytes of a valid update file and trying to use the modifyed software. Beware that this may result in a bricked device if the software is finnaly installed.

This control is considered satisfactory when it is verified that the device does not remotely accept minimally modified updates.