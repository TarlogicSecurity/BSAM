---
layout: control
title: BSAM-PA-08
summary: Bluetooth link key removal
description: Bluetooth devices should avoid deleting shared link keys when receiving new pairing requests. This can be exploited by attackers to hijack communications
parent: Pairing
grand_parent: Controls
nav_order: 8
lang: en
page_id: cont_pair_08
permalink: controls/remove-bluetooth-link-keys/
tags:
- BR/EDR
- BLE
---

After pairing, devices store the generated link keys so they can be reused in future connections. Bluetooth devices should avoid deleting these shared keys when receiving new pairing requests, as removing previously established keys can create scenarios that compromise the device’s security.

* If an attacker forces a disconnection and triggers a new pairing process, the device may erase the previously stored link key and generate a new one. This re‑pairing scenario allows the attacker to replace the legitimate key with one negotiated under their control, effectively enabling them to intercept or manipulate future communications.

* By forcing the user to repeat the pairing procedure, an attacker can also capture the new pairing attempt and exploit weaknesses in the process itself. This includes brute‑forcing the PIN code, whose entropy is typically lower than other components of Bluetooth security, or leveraging user confusion to trick them into approving an unauthorized pairing request.

This technique, commonly known as Bluesnarfing, allows an attacker to take control of the communication between two devices. To mitigate this risk, shared key removal must only occur when both devices are properly linked through an authenticated connection.


## Description

For the following sections, we will consider A and B as two devices paired with each other and C as a device used for control verification.

To check if C can maliciously unpair B from A, different methods can be tried:
* Impersonate device B and perform a pairing with A
* Impersonate device B and initiate an authenticated connection with A without having a link key
* Impersonate device B and initiate an authenticated connection with A with a fake link key

In the first case, if A does not comply with this check, it is possible that A will accept the new pairing and delete the old key.

In the second case, when attempting to initiate authentication, C's driver will send the error "key or pin missing". Some devices, upon receiving this error, delete the old key and become unpaired.

In the third case, C will fail authentication with A. Some devices, upon receiving a failed authentication, remove the shared key and become unpaired.

In either case, A should not delete the shared key, to prevent a third party from unpairing the two devices and subsequently trying to capture the link.

## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07,BSAM-RES-09,BSAM-RES-10' %}

## Example case

We will use Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) to capture packets for analysis.

We have a computer with _Scapy_ installed, and a pair of Bluetooth headphones paired with a mobile phone.

The computer is impersonating the mobile phone using the scapy tool and tries to recover a previous connection with the headphones, which will be denied by the _Link Key Request Negative Reply_ command.

![Wireshark Link Key Request Negative Reply]({{ '/assets/img/bsam-pa-01_link_key_request_negative_reply.png' | relative_url}})

The computer makes a new connection to the headphones, requesting the input/output capabilities _-_IO Capability_-_ with the _IO Capability Request_ command.

![Wireshark IO Caps Request]({{ '/assets/img/bsam-pa-01_io_cap_request.png' | relative_url}})

After sharing the _IO Capabilities_ of both devices, a connection is established, notified by the _Simple Pairing Complete_ command.

![Wireshark Simple Pairing]({{ '/assets/img/bsam-pa-01_simple_pairing_complete.png' | relative_url}})

After the establishment, a key negotiation process begins between the two Bluetooth controllers that cannot be recorded in Wireshark, but it ends with the notification of a new link key with the _HCI_Link_Key_Notification_ command.

To verify that the headphones were susceptible to this attack, an attempt is made to connect from the mobile phone, and if they respond with the _Link Key Request Negative Reply_ command, it means that it was possible to erase the link key and therefore a _FAIL_ for this check.

## External resources

* Bluetooth Core V5.3, Vol. 3, Part H, Appendix B.1 DATABASE LOOKUP