---
layout: default
title: BSAM-RES-09
summary: Changing the attributes of a controller
description: Changing the attributes of a Bluetooth controller can allow impersonating another device or simulating different scenarios
parent: Resources
nav_order: 8
lang: en
page_id: BSAM_resources_09
permalink: resources/change-controller-attributes/
---

# {{ page.summary }}

Many of the functions and procedures of the Bluetooth standard depend on device-specific parameters, such as: the device name, address, input and output capabilities (if it has a display or keyboard), the version of the Bluetooth standard it supports, the driver firmware version, etc. These parameters are generically referred to as device attributes and many of them are relevant during the establishment of a connection to identify or authenticate the device against other devices.

Being able to set device attributes is a necessary skill to, among other things, impersonate a known device against others or to simulate different scenarios.

For this purpose, the different attributes of a driver can be modified. Some of them can be modified through mechanisms provided by the Bluetooth standard. Others have to be modified using the manufacturer's own mechanisms or even by modifying the firmware they run.


## Standard mechanisms

These are mechanisms supported by the Bluetooth standard and should be compatible with all Bluetooth adapters on the market. Normally these procedures are done by sending HCI packets.


### Name modification

The `Local Name` is the name that our controller exposes to other devices. This parameter can be modified via standard HCI commands. More specifically this must be done through a `HCI Write Local Name Command`. This command is documented in Bluetooth Core V5.3, Vol. 4, Part E, Section 7.3.11.


### Modifying the device class

The `Device Class` can be modified by the `HCI Write Class of Device Command` command documented in Bluetooth Core V5.3, Vol. 4, Part E, Section 7.3.26.


## Vendor-specific mechanisms

### Broadcom / Cypress

The manufacturers Broadcom and Cypress implement many non-standard HCI commands that allow modification of attributes of their drivers. These commands are not officially documented, but some of the known commands that may be useful are listed below:

| Comand        | Opcode    | Parameters        |
|:--------------|:----------|:------------------|
| Write Address | 0xfc01    | MAC addr          |
| Write RAM     | 0xfc4c    | MAC, Data         |

Through these commands you can overwrite memory regions containing data such as the supported LMP version, the manufacturer identification, the _features_ and _extended features_ of your driver.


## External references

* Bluetooth Core V5.3, Vol. 4, Part E, Section 7.3.11 - Write Local Name command
* Bluetooth Core V5.3, Vol. 4, Part E, Section 7.3.26 - Write Class of Device command
