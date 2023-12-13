---
layout: control
title: BSAM-PA-04
summary: Rejection of legacy pairing
description: Check that your Bluetooth device uses a Secure Connections mode in the pairing process
parent: Pairing
grand_parent: Controls
nav_order: 4
lang: en
page_id: cont_pair_04
permalink: controls/bluetooth-rejection-legacy-pairing/
tags:
- BR/EDR
- BLE
---


Various pairing methods are used in Bluetooth. One of these methods is known as _Legacy pairing_ designed to simplify the connection between devices with limited computational capabilities.
Due to this limited computational capability, key generation for _Legacy pairing_ uses obsolete and insecure algorithms.

During the initial phase of pairing, there is an exchange of characteristics called _Pairing Feature Exchange_, which determines the type of key generation method that can be used, either long term key (_LTK_) or short term key (_STK_). _STK_ keys are generated with _Legacy Pairings_ and should, therefore, be avoided.

It is recommended to use pairing in the _LE Secure connection_ mode whenever the device's capabilities allow for it.

## Description

In a Bluetooth pairing, a distinction is made between the device that initiates the _Pairing Feature Exchange_, referred to as the __initiator__, and the device that responds to these requests, known as the __responder__. During this exchange of features, one of the fields is called authentication requirements (_AuthReq_), and within this, we find the Secure Connections (_SC_) subfield of 1 bit. A value of 0 indicates that the device will use _Legacy Pairing_, while a value of 1 enables the use of _LE Secure Connections_.

It's important to note that a device that supports the capability of _LE Secure Connections_ can theoretically support connections of the _Legacy pairing_ type, but this is not recommended based on the type of device and its intended use, which will be evaluated by an auditor.

To verify compliance with security controls, the following scenarios should be considered:

* If the _SC_ field is 0 in the __initiator__, it should result in a _Pairing Failure_ response from the __responder__. This is considered satisfactory for the __responder__.

* If the _SC_ field is 0 in the __responder__, it should result in a _Pairing Failure_ response from the __initiatior__. This is considered satisfactory for the __initiatior__.

* If both devices have an _SC_ field of 1, and the __initiatior__ requests _Legacy pairing_, it should receive a _Pairing Failure_ response from the __responder__. This is considered satisfactory for the __responder__.

* If both devices have an _SC_ field of 1, and the __responder__ requests _Legacy pairing_, it should receive a _Pairing Failure_ response from the __initiatior__. This is considered satisfactory for the __initiatior__.

## Example case

We will use Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) to capture packets for analysis.

During the Bluetooth pairing process between two devices, they exchange capabilities, initiated by the _Pairing Request_ command. To indicate whether the connection is LE _Secure Connection_ or _Legacy pairing_, the _Secure Connection_ flag is used. In this case, it is set to `0` (`False`).

![Wireshark Legacy Pairing Response]({{ '/assets/img/bsam-pa-04_pairing_legacy_resquest_no_sec.png' | relative_url}})

The device responding to the _Pairing Request_ command has two options: disconnect because it does not support _Legacy pairing_, or continue with the connection using the _Pairing Response_ command, indicating its supported _Secure Connection_ flag. In this case, it is set to 0 (`False`).

![Wireshark Legacy Pairing Request]({{ '/assets/img/bsam-pa-04_pairing_legacy_response_no_sec.png' | relative_url}})

Finally, both devices accept the insecure connection.

![Wireshark Legacy Pairing Confirm]({{ '/assets/img/bsam-pa-04_pairing_legacy_confirm.png' | relative_url}})

The result of the control will be _FAIL_ if the device returns the _Secure Connection_ flag with a value of `0` (`False`), or if it returns `1` (`True`) but we can respond that we will use the value `0` (`False`) and it accepts.


## External references

* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.1 Introduction
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.2 - IO capabilities
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3. Pairing Methods
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.1 Selecting Key Generation Method
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.5 LE Legacy pairing phase 2
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.3.5.6 LE Secure Connections pairing phase 2
