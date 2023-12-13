---
layout: default
title: BSAM-RES-08
summary: Device discovery
description: Bluetooth device discovery is the process of finding Bluetooth devices within the communication range of another Bluetooth device
parent: Resources
nav_order: 7
lang: en
page_id: BSAM_resources_08
permalink: resources/device-discovery/
---

# Bluetooth device discovery

Device discovery procedures are used to enumerate Bluetooth devices in the communication range of another Bluetooth device. It should be noted that these procedures are different depending on the variants of Bluetooth used.


## Bluetooth LE

The discovery process in Bluetooth LE can be entirely passive. It consists of listening on the 3 announcement channels awaiting for announcement packets coming from other devices.

It is possible that many tools send packets to complete the information received in the announcement packets. An example of a common data request could be a name request after discovering a device in an announcement packet that does not expose this data.


## Bluetooth BR/EDR

In Bluetooth BR/EDR the discovery process is active. A discovery request has to be sent and devices respond to it. These discovery requests can request more or less data up to and including optional `RSSI` or manufacturer data.

Many tools that perform Bluetooth BR/EDR discovery send name request requests to complete the discovery process.

At a low level, initiating a discovery consists of notifying the driver that you want to perform an `Inquiry scan` and/or a `Page scan`. This is done using the `HCI Write Scan Enable Command` described in Bluetooth Core V5.3, Vol. 4, Part E, Section 7.3.18.


## Bluetooth discovery tools

Virtually any device with support for Bluetooth communications has some tool that allows Bluetooth device discovery as it is a basic process to establish a connection. However, it is important to know that not all tools request the same information and follow the same procedure.

In order to know which procedure is being used and to be able to extract as much information as possible, it is recommended to capture the discovery processes using the resource {% include res_link.md res='BSAM-RES-05' %}. This will allow further analysis with specialised tools showing all exchanged data and not only name and MAC as in many cases.

Some of the following tools can be used to start the discovery processes:

* Android Bluetooth settings screen
* iOS Bluetooth settings screen
* Windows Bluetooth settings screen
* Bluetooth settings screen of any Linux distribution
* bluetoothctl
* Scapy
* [Acrylic Bluetooth LE Analyzer](https://www.acrylicwifi.com/bluetooth-analyzer/)


## External references

* Bluetooth Core V5.3, Vol. 4, Part E, Section 7.3.18 - Write Scan Enable command
