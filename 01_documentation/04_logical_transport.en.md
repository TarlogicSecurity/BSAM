---
layout: default
title: 'Logical Transport: ACL and SCO'
description: Bluetooth logical transport - ACL and SCO, protocols for data and voice transmission
parent: Documentation
nav_order: 3
lang: en
page_id: doc_logical_transport
permalink: documentation/logical-transport/
---

# {{ page.title }}

Logical transport protocols are used to stablish and manage connections between Bluetooth Hosts.

The logical transport layers are the lowest communication layers available to a Host and implementation of logical transport protocols are part of the host Bluetooth stack.

There are two logical transport protocols depending on the characteristics of a connection: ACL and SCO.

![Bluetooth logical transport protocols]({{ '/assets/img/doc_logical_transport.png' | relative_url}})


## ACL (Asynchronous Connection-Less)

The ACL protocol is an asynchronous connection-less oriented protocol.

Asynchronous means that this protocol is meant to exchange irregular amounts of data in time. Upper layers of this protocols may need to send varying amounts of data depending on external factors such as device usage, interaction or data availability...

Connection-less refers to the capability to setup both unicast and multicast channels for data transmission.

ACL protocol will use as much bandwidth as available in a certain moment. This means that connection speed is dependent on other connections and other exchanges of information, slowing down when multiple devices request transmission at the same time. It also means that there may be times when ACL provides huge bandwidths and allows for high amounts of data transfer.

This is the de-facto protocol for Bluetooth logical transport.


## SCO (Synchronous Connection-Oriented)

The SCO protocol is a synchronous connection-oriented protocol.

Synchronous means that this protocol is meant to exchange fixed bandwidth data in time. Upper layers of this protocol will have the capability to transmit the same amount of data through time.

Connection-oriented refers to the unicast only capability of the protocol.

In practice, SCO is implemented as fixed time ACL slots, thus priorizing low latency comunications.

The characteristics of this logical transport make it ideal for the transmission of data and voice streams such as phone call audio. The streams do not require high bandwidth but require low latency and consistent voice streams.

