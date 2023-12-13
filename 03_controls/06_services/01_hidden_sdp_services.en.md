---
layout: control
title: BSAM-SE-01
summary: Hidden Bluetooth SDP services
description: Check that your Bluetooth device lists all available services in the Service Discovery Protocol (SDP) and no hidden services exist.
parent: Services
grand_parent: Controls
nav_order: 1
lang: en
page_id: cont_services_01
permalink: controls/hidden-bluetooth-sdp-services/
tags:
- BR/EDR
- BLE
---

In Bluetooth, a service refers to a specific functionality or feature that a device offers to other devices within a Bluetooth network. Each service is identified by a Universally Unique Identifier (UUID), and can be discovered and used by other Bluetooth devices that support that particular service.

In Bluetooth there are multiple ways to discover available services in a device. One of the mechanisms to perform this discovery is the _Service Discovery Protocol_ or _SDP_ and its purpose is to discover services on a remote device, obtain information about the services and their characteristics, and establish how to connect to them.

If _SDP_ is present, it is important that it is correcly configured and that lists all the available services in the device. Failing to list a service in the _SDP_ does not prevent users from accesing it and does not secure it.

On the same topic, if a service is correctly secured, there is no need to hide it and correctly configuring _SDP_ to list it will be in conformance with Bluetooth Specifications, allowing for better device interoperability.


## Description

In order to check if there are hidden services it is important to perform an _SDP_ discovery. This is an standard Bluetooth procedure that can be performed with standard bluetooth tools.

After having a list of available _SDP_ services, it is needed to perform alternative ways of discovering Bluetooth services. This can include the following procedures:

* Capturing Bluetooth communications while operating the device to discover communications with previously unknown services.
* Performing bruteforce scans of services in a device.
* Performing service scans using different service discovery protocols such as GATT if available.

If there are services hidden to the _SDP_ service, it is not correctly configured.


## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05' %}


## Example case

The existence of hidden SDP (Service Discovery Protocol) services on a BR/EDR-enabled device is verified. The tests are conducted using a computer. Wireshark with [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) is used to capture packets for analysis.

To perform the check of available _Service_, the _sdptool_ tool, which is part of the __bluez__ package in GNU-Linux operating systems, is used. A request is made to list the system's services with the command sdptool browse.

![sdptool not discovered]({{ '/assets/img/bsam-se-01_sdptool.png' | relative_url}})

This procedure displays all the _Service_ that the SDP server has recorded. However, the _Service Discovery Protocol_ does not have a mechanism to notify SDP clients when _Service_ are added or removed from the SDP server. Due to this, there might be services not listed with the `sdptool browse XX:XX:XX:XX:XX:XX` command.

To check for hidden services, Scapy, or a similar tool, is used to generate a request to read the attributes of SDP _Service_. All available possibilities in each field need to be checked:

* ServiceRecordHandle
* MaximumAttributeByteCount
* AttributeIDList
* ContinuationState

For each request, a response is obtained that allows verifying whether the request made is correct or not.

By comparing the correct responses to the request for service attributes with those listed by the SDP server, hidden _Service_ can be identified.

The check control _FAIL_ when different services are found in the read requests compared to the SDP server's listing.