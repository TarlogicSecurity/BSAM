---
layout: default
title: Bluetooth LE connection mode
description: List of connection modes and procedures between Bluetooth Low Energy devices
parent: Documentation
nav_order: 10
lang: en
page_id: doc_le_connection
permalink: documentation/bluetooth-le-connection-mode/
---

# {{ page.title }}

According to __Bluetooth Core V5.3, Vol. 3, Part C, Section 10, titled "SECURITY ASPECTS - LE PHYSICAL TRANSPORT"__, the modes and procedures are defined in the same manner for both asynchronous ACL and synchronous CIS connections. This section aims to establish how BLE devices will be paired in terms of security. It's important to note that each mode and procedure comes with specific requirements that are not elaborated here, and it will be essential to consult the mentioned section of the standard for detailed information.


## Connection modes


In Bluetooth LE, there are five connection modes that are subdivided into levels:

* _LE Security Mode 1_:

    - _Level 1_: No Security (no security or encryption)
    - _Level 1_: Unauthenticated pairing with encryption
    - _Level 1_: Authenticated pairing with encryption
    - _Level 1_: Authenticated pairing with LE Secure Connection pairing using a secure 128-bit key

* _LE Security Mode 2_:

    - _Level 1_: Unauthenticated pairing with data signing
    - _Level 1_: Authenticated pairing with data signing

* _Mixed security Mode_:

    - These are security configurations based on each type of security mode and configuration supported on each device.

* _Secure Connections Only Mode_:

    - Only secure and authenticated connections are allowed

* _LE Security Mode 3_:

    - _Level 1_: No Security
    - _Level 2_: Use of an unauthenticated broadcast code
    - _Level 3_: Use of an authenticated broadcast code

## Procedure

The procedures are not exclusive to any specific mode but are necessary to access a security mode in Bluetooth LE.

* _Authentication procedure_

    - The authentication procedure covers _LE Security Mode 1_ and is only performed after the connection has been established.
    - Authenticationo in  _LE Security Mode 1_ is achieved by enabling encryption.

* _Data Signing_

    - Data signing is used to transfer authenticated data between two devices in an unencrypted communication.
    - When _LE Security Mode 2_ is requested, the connection data must be signed.

* _Authorization procedure_

    - A service may require authorization before granting access, which is user confirmation to proceed with the procedure.
    - Authentication does not necessarily provide authorization. Authorization may be granted through user confirmation after successful authentication.

* _Encryption procedure_

    - Central device encrypts the connection using _Encryption Session Setup_ to provide integrity and confidentiality.
    - Peripheral device could encrypt the connection with the _Security Request_ command.
