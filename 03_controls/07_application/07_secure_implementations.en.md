---
layout: control
title: BSAM-AP-07
summary: Secure services
description: Check that the applications using Bluetooth on your device are implemented securely. This is important to prevent a malformed message from causing any vulnerabilities
parent: Application
grand_parent: Controls
nav_order: 7
lang: en
page_id: cont_app_07
permalink: controls/secure-bluetooth-services/
tags:
- BR/EDR
- BLE
---

Applications use Bluetooth services to transmit information among paired devices.

Data should always be treated as untrusted, so these applications must implement the necessary controls to validate that both input and output data are appropriate.

## Description

To verify that Bluetooth services used by applications are implemented securely, the following recommendations can be employed:
* Source Code Review: Provides a comprehensive view of the implementation and allows validation of whether it's a secure implementation.
* Fuzzing: Allows for the creation of data flows that are not as expected by the application, and in the event of validation issues in their content, it can reveal faults in a device.
* Reverse Engineering: If the source code is not available, reverse engineering can be used to evaluate the mechanisms handling data for implementation issues.

The ultimate goal of this control is to ensure that in the face of random data inputs, the application can maintain its integrity and functionality.