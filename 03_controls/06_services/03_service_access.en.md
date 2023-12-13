---
layout: control
title: BSAM-SE-03
summary: Bluetooth Service access control
description: Verify the access control of your Bluetooth services to prevent unauthorized access.
parent: Services
grand_parent: Controls
nav_order: 3
lang: en
page_id: cont_services_03
permalink: controls/bluetooth-service-access-control/
tags:
- BR/EDR
- BLE
---

Bluetooth services have a number of permissions so that you can limit how you can interact with them, whether you need to authenticate first, whether you can read or write.

For each Bluetooth service exposed on a device, it is necessary to verify if access control has been implemented correctly.

This is because the standard is focused on interoperability, and there are services in which interoperability takes precedence and should allow access without authentication, as can be the case with the GATT profile service.

A device, due to the exchange of features (_Feature Exchange_), may have required encryption to establish the connection and access the GATT profile. However, since there are various types of encryption, it is possible that it may not provide a sufficient level of security to access a _Service_ that requires higher protection. Similarly, it may happen that, to access certain functionalities exposed in a _Service_ from the general attribute profile, it may be necessary to even raise the encryption level, moving from _Phase 2_ to _Phase 3_ of pairing in order to access said _Service_.

Therefore, it is important to verify the encryption configured for each security level implemented on the device.

## Description

To perform this check a list of available devices must be elaborated. For further information on the topic check {% include ctl_link.md ctl='BSAM-SE-01' %} and {% include ctl_link.md ctl='BSAM-SE-02' %}.

For each exposed service a check on what resources can be accessed and modified must be performed. This requires certain adaptation to conform to the protocols that expose the service. Some examples ilustrate this need:

* For any GATT service it must be checked wether attributes can be read, written or notified depending on our level of security/authentication. For each result one must consider whether that access is adecuate or not for the data it exposes.
* For each RFCOMM it must be checked wether an user can read or write to that service for each level of security available in that service. It must be evaluated if exposed data is adecuate for the security level and authentication.
* For the SDP protocol it must be verified if it is desirable to obtain access to the service without authentication and/or encryption at Bluetooth level.
* It is important to verify what functionalities of the SMP protocol can be accessed without authentication and/or encryption at Bluetooth level.

## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-SE-01,BSAM-SE-02' %}

Other related documentation:

| ID               | Description                                                        |
|:-----------------|:-------------------------------------------------------------------|
| Documentation    | [Bluetooth LE connection mode]({% link 01_documentation/06_le_modes.en.md %})                                       |


## Example case

The correct configuration (read/write) of GATT services on a device with BLE connectivity will be tested. The tests will be carried out with a computer that will connect (without authorization, without authentication and without encryption) to the audited device. We will use Wireshark with [BTVS](https://learn.microsoft.com/es-es/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) to perform packet capture for analysis. 

The GATT profile describes how data is exchanged with the profiles (_Profile_). In it, the _Profile_ are defined, which group a set of services (_Service_). These _Service_, in turn, are composed of characteristics (_Characteristic_), and these _Characteristic_ have descriptors (_Descriptor_).

Permissions to read, write or perform specific actions with each service and feature depends on authorization, authentication and encryption. These requirements (authorization, authentication and encryption) are device-specific and set by the device developer.

The Bluetooth driver internally lists the GATT services via the _Handle_ or _UUID_ field. The standard has limited the number of services, features and descriptors for a device to 65.535 (0xFFFF), equivalent to traversing from value 1 to 65.535 (0xFFFF) _Handle_ values. To each _Handle_ corresponds a _UUID_ and vice versa. There is a list of UUIDs associated with a particular function in the Bluetooth SIG document [Assigned Numbers](https://www.bluetooth.com/specifications/assigned-numbers/).

The computer and the audited device will be connected and a Read and Write request will be made with Scapy, or some similar tool, for each _Handle_.The possible cases of response to a read / write request, according to the ATT protocol, will allow to know the configuration of each _Handle_.

The responses to a read request can be:

| Read Response | Meaning |
|---------------|---------|
| Read Response | The _Handle_ is readable without permissions |
| Insufficient authorization error | The _Handle_ needs authorization to be read |
| Insufficient authentication error | The _Handle_ needs authentication to be read |
| Encryption key size too short error | The _Handle_ needs encryption with a longer password|
| Insufficient encryption error| The _Handle_ needs a higher encryption level (i.e., upgrade from Legacy to Secure pairing) |
| Invalid _Handle_ error | The _Handle_ does not exist |
| Read not permitted error | The _Handle_ is not readable |

The responses to a write request can be:

| Write Response | Meaning |
|---------------|---------|
| Write Response | _Handle_ is writeable without permissions |
| Invalid length error | The _Handle_ is writeable but the length is incorrect |
| Insufficient authorization error | You need to be authorized to write the _Handle_ | 
| Insufficient authentication error | Authentication required to write the _Handle_ |
| Encryption key size too short errro | Longer key is needed to write in the _Handle_ |
| Insufficient encryption error | The _Handle_ needs a higher encryption level (i.e., move from Legacy to Secure pairing) |
| Invalid _Handle_ error | The _Handle_ does not exist |
| Write not permitted error | The _Handle_ cannot be written |


Each configuration is checked for the device to ensure that it is correct by checking it against the Bluetooth SIG's _Assigned Numbers_ document with the NIST [Bluetooth Security Guide](https://www.nist.gov/publications/guide-bluetooth-security-2).

The check control _FAIL_ when a _Handle_ has the incorrect permissions for the type of operation assigned based on the device type per NIST specifications.

## Related resources

* Bluetooth Core V5.3, Vol. 3, Part H, Section 2 SECURITY MANAGER
* Bluetooth Core V5.3, Vol. 3, Part H, Section 2.1 INTRODUCTION
* Bluetooth Core V5.3, Vol. 3, Part H, Section 3 SECURITY MANAGER PROTOCOL
* Bluetooth Core V5.3, Vol. 3, Part H, Section 3.6 SECURITY IN BLUETOOTH LOW ENERGY