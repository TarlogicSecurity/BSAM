---
layout: control
title: BSAM-AP-05
summary: Replay attacks
description: Check if your device is vulnerable to replay attacks. This is important to prevent valid packets from being reused to perform unauthorized actions
parent: Application
grand_parent: Controls
nav_order: 5
lang: en
page_id: cont_app_05
permalink: controls/bluetooth-replay-attacks/
tags:
- BR/EDR
- BLE
---

In a replay (or relay) attack, an attacker intercepts and retransmits a valid message. This is possible when no mechanism exists to detect repeated transmissions or to ensure message freshness.

If an application implements custom security mechanisms and uses cryptography at the application layer for a specific service, these measures must adequately protect against replay attacks. Otherwise, even application level security can be bypassed.

## Description

The procedure consists on capturing a valid packet or transaction of a service with custom security measures in place and sending it back to check wether it performs the desired actions or if the packet is ignored.

This control is considered satisfactory when it is verified that the device does not remotely accept the same update packet twice.

## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}
