---
layout: control
title: BSAM-AU-03
summary: Forced disconnection
description: Check if your Bluetooth device is vulnerable to forced disconnection attacks. This is important to prevent an attacker from being able to disconnect your device from a Bluetooth connection
parent: Authentication
grand_parent: Controls
nav_order: 3
lang: en
page_id: cont_auth_03
permalink: controls/bluetooth-forced-disconnection/
tags:
- BR/EDR
- BLE
---

During a Bluetooth connection the devices are responsible for keeping the connection open so that they can continue to exchange messages, these messages have a maximum response time from the remote device.

## BR/EDR disconnection
If the timeout is exceeded the connection is automatically closed. For the closing of a connection by one of the devices, either device can send an _LMP_detach_ message with the reason for the disconnection.

This disconnection should only be allowed by messages coming from the same connection that is being closed. An attacker can create a new connection by impersonating another device and send a disconnect message, being able to disconnect all devices with the MAC address of the message. This behavior can be exploited to produce denials of service and initiate other attacks.

## BLE disconnection
If the timeout is exceeded the local device sends a cancellation of the previous send. If the command is related to the pairing process the result will be the cancellation of the pairing with a data of the reason for disconnection.

In addition to disconnection due to a timeout for a response in BLE, there is the possibility of forcing an unpairing at any point during the connection using the _HCI_Disconnect_ command. The Handle will indicate the connection to be disconnected. Depending on whether the connection is synchronous or asynchronous, the PDU (Protocol Data Unit) of the message will differ:


| Connection Type    |  PDU Message         | Opcode |
|:-------------------|:---------------------|:-------|
| Asynchronous (ACL) | LL_TERMINATE_IND     | 0x02   |
| Synchronous (CIS)  | LL_CIS_TERMINATE_IND | 0x22   |


## Description

Assuming the scenario where devices A and B are connected, it must be proven that device C cannot force disconnection of B. To do so, C must impersonate B and initiate a connection to A to immediately send a disconnect message.

For the check to be satisfied, the process must be rejected or the disconnect message must only disconnect C, but not B.

## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07,BSAM-RES-09' %}

## Example case

We will use Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) to capture packets for analysis.

We have Bluetooth headphones paired with a mobile device during the test. These headphones allow establishing a new connection with a laptop using the Scapy application, maintaining a simultaneous connection between both devices.

To initiate the connection, the laptop, with the assistance of the Scapy tool, starts the process with the _LE Create Connection_ command.

![Wireshark LE Create Connection]({{ '/assets/img/bsam-au-03_create_connection.png' | relative_url}})

The headphones notify the acceptance of the connection with the _LE Meta_ event, with a value of `0x00` (`Success`) in the _Status_ field.

![Wireshark Connection complete]({{ '/assets/img/bsam-au-03_connection_complete.png' | relative_url}})

After accepting the connection, a disconnection has been requested, proceeding to disconnect all devices that were connected to the headphones using the _Disconnect_ command.

![Wireshark Disconnect]({{ '/assets/img/bsam-au-03_disconnect.png' | relative_url}})

It can be verified that the laptop have been unpaired in the Wireshark capture through the _Disconnect Complete_ command.

![Wireshark Forced Disconnect full process]({{ '/assets/img/bsam-au-03_forced_disconnection.png' | relative_url}})

The check control _FAIL_ if it disconnects from all devices.

The auditor must verify that it is no longer paired with additional devices.

## External references

* Bluetooth Core V5.3, Vol. 6, Part B, Section 4.5.12 Connection Termination and loss
* Bluetooth Core V5.3, Vol. 6, Part B, Section 5.1.6 ACL Termination Procedure
* Bluetooth Core V5.3, Vol. 6, Part B, Section 5.1.16 Connected Isochronous Stream Termination procedure
* Bluetooth Core V5.3, Vol. 4, Part E, Section 7.1.6 Disconnect Command