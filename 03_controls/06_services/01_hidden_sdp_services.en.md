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

In Bluetooth, a service represents a specific functionality offered by a device, identified by a Universally Unique Identifier (UUID). Services can be discovered and used by other Bluetooth devices that support them.

Bluetooth provides multiple mechanisms to discover available services, one of which is the Service Discovery Protocol (SDP). SDP enables the identification of remote services, retrieval of their characteristics, and the determination of how to connect to them.

To identify hidden or undocumented services, an SDP discovery must be performed using standard Bluetooth tools. After obtaining the list of services advertised via SDP, additional service discovery techniques should be applied to verify that all detected services match those advertised. The presence of a service not listed in SDP indicates a hidden service but does not secure it, as access remains possible.

If a service is properly secured, hiding it is unnecessary. Maintaining accurate SDP listings improves device interoperability and aligns with Bluetooth specifications.

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