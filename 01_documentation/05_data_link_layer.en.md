---
layout: default
title: 'Data Link Layer: L2CAP'
description: L2CAP is a Bluetooth data link layer protocol for multiplexing, segmentation, and abstraction of data
parent: Documentation
nav_order: 4
lang: en
page_id: doc_data_link_layer
permalink: documentation/data-link-layer/
---

# {{ page.title }}

The Logical Link Control and Adaptation Layer Protocol (L2CAP) resides in the data link layer.  L2CAP provides connection-oriented and connectionless data services to upper layer protocols.

The focus of this protocol is to add protocol multiplexing capability, segmentation and reassembly operation and group abstractions.

It is important to note that L2CAP specification is only defined for ACL logical transports and no SCO support is planned.

![Bluetooth data link layer protocols]({{ '/assets/img/doc_data_link_layer.png' | relative_url}})

## L2CAP packet structure 

L2CAP general packet structure is defined as a _basic frame_ or _b-frame_.

![L2CAP b-frame structure]({{ '/assets/img/doc_l2cap_bframe.png' | relative_url}})

L2CAP provides different channels (CID - Channel identifier) where different protocols or services can be multiplexed. Some of the most common CIDs are `0x0001` for the Signaling Channel, `0x0002` for the Connectionless Channel or `0x0006` for the SMP (Security Manager Protocol).

Particularly, the Connectionless Channel defines an extended packet structure called _group frame_ or _g-frame_ that provides a second layer for service multiplexing called _Protocol/Service Multiplexer_ or _PSM_.

![L2CAP g-frame structure]({{ '/assets/img/doc_l2cap_gframe.png' | relative_url}}
)

PSM services are classified in two ranges. The first is assigned by the Bluetooth SIG while the second is free to be assigned by vendors. Bluetooth SIG assigned PSM services can be found in the Bluetooth Assigned numbers document at Sección 2.5. PSM services are further described in the Bluetooth Core Spec V5.3 Vol 3 Part A Section 4.2.
