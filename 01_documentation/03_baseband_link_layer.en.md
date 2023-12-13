---
layout: default
title: 'Baseband Link Layer: LMP and LL'
description: LMP and LL - Bluetooth link layer protocols for establishing and managing connections between controllers
parent: Documentation
nav_order: 2
lang: en
page_id: doc_baseband_link_layer
permalink: documentation/baseband-link-layer/
---

# {{ page.title }}

Link layer protocols are used to control and negotiate all aspects of the operation of the Bluetooth connection between two controllers.

Link layer protocols are managed in the controller and Bluetooth hosts do not have knowledge about the exchanged link layer packets.

Bluetooth controllers may support different modes of operation such as Bluetooth Low Energy and/or Bluetooth Basic Rate and/or Bluetooth Extended Data Rate. Depending on the support of those modes, different link layer protocols may be used by the controllers.

![Representation of Bluetooth link layer protocols]({{ '/assets/img/doc_baseband_link_layer.png' | relative_url}})


## LMP (Link Manager Protocol)

Link Manager Protocol (LMP) is the link layer protocol used to stablish and manage connections between BR (Basic Rate) and EDR (Extended Data Rate) controllers, most commonly known as Bluetooth Classic.

The full Link Manager Protocol (LMP) specification can be found in the Bluetooth Core Specification V5.3, Vol. 2, Part C.

## LL (Link Layer)

Link Layer (LL) is the link layer protocol used to stablish and manage conections between BLE (Bluetooth Low Energy) controllers.

Link Layer (LL) specification can be found in the Bluetooth Core Specification V5.3, Vol. 6, Part B.
