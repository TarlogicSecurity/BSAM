---
layout: default
title: Physical architecture
description: Bluetooth physical architecture - Host and Controller, two essential elements for communication
parent: Documentation
nav_order: 1
lang: en
page_id: doc_physical_architecture
permalink: documentation/physical-architecture/
---

# Bluetooth physical architecture

The physical architecture of Bluetooth is split into two main elements: _Host_ and _Controller_.

![Representation of the physical architecture of Bluetooth]({{ '/assets/img/doc_physical_architecture_host_controller.png' | relative_url}})

## Bluetooth Controller

A controller is a _chip_, or a set of them, capable of transmitting and receiving radio waves, responsible for performing the lower-level tasks of the Bluetooth communication protocol.

A controller may support one or more Bluetooth technologies. There are controllers that only support _Bluetooth LE_, others that support _Bluetooth BR/EDR_, and others that support combined operation in both modes.

The controller, besides having the hardware to convert radio signals into bits and vice versa, also has multiple responsibilities when it comes to creating, maintaining, and closing connections. The lower layers of the Bluetooth standard are implemented in the firmware of these controllers, and they have packet processing capability without the need for these packets to reach the Host.


## Host

The _Host_ refers to the hardware that uses a _Controller_ to communicate via Bluetooth. This _Host_ must run a _software stack_ that provides an abstraction and allows applications to interact with Bluetooth devices independently of the hardware.

This software run on the _Host_ is responsible for functions related to discovery and pairing processes. Some of these functions include enumerating nearby devices, controlling whether a device should be discoverable or not, enumerating the capabilities of the _Host_ to decide which pairing methods are available, intervening during the pairing process to allow the user to confirm or deny this action, or storing pairing keys in a secure place.


## Host Controller Interface or HCI

Host Controller Interface_ or _HCI_ refers to the protocol used to communicate a _Host_ with a _Controller_. The _HCI_ protocol usually limits the ability of a _Host_ to modify the behaviour of a _Controller_. Overcoming this barrier involves modifying the _Controller_ or modifying the _Controller_ firmware.
