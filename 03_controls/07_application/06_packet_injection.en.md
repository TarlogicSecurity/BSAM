---
layout: control
title: BSAM-AP-06
summary: Packet forging/injection attacks
description: Apply secure encryption methods in the Bluetooth application to prevent spoofing and packet injection attacks.
parent: Application
grand_parent: Controls
nav_order: 6
lang: en
page_id: cont_app_06
permalink: controls/bluetooth-packet-injection/
tags:
- BR/EDR
- BLE
---

A packet injection attack is the deliberate sending of altered or crafted data packets for the purpose of manipulating or disrupting the normal operations of your connected devices. This is possible when there is no verification that the packet is correctly formatted or sent by a legitimate device.

If an application requires custom security methods and decides to use cryptography for a particular service, application layer security methods must be adecuate to prevent packet forging and injection attacks.

Not complying with this control may mean that, despite of the efforts of using application level security measures, theese can be bypassed.


## Description

The procedure consists on studying and analyzing if the selected encryption methods are secure against packet forging and packet injection attacks.

This control is considered satisfactory when it is verified that the mechanism for generating a valid packet is not so trivial that it allows the creation of new packets without knowledge of the encryption key for the packet.

## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}

## Example case

During an audit, a device with custom application layer encryption is found. Particularly, an user is able to set a secret password in the device that cyphers the traffic at application layer.

Data packets exchanged with the device include a PDU that contains a _sequence_ number to avoid replay attacks, as in {% include ctl_link.md ctl='BSAM-AP-05' %}. This number gets incremented each time a packet is sent and it is checked that the new number is greater than the latest record in the device. The PDU also contains a CRC32 field to verify the integrity of the packet.

Packets are encrypted using an stream cypher. This means that a pseudo-random stream is generated and xored to the original packet.

In this scenario, an attacker can captura an encrypted message _c_ for a an unknown plaintext _m_. The attacker can compute an encrypted message _c'_ = _c_ ⊕ (Δ, CRC(Δ)) that will correctly decrypt to a message _m'_ = _m_ ⊕ Δ . If the attacker uses the Δ changes to modify the _sequence_ number part of the message, it is able to reinject packets back to the device without knowing its contents.
