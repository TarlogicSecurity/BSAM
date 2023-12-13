---
layout: default
title: BSAM-RES-06
summary: Enabling debug mode on a Bluetooth controller
description: Enable debug mode on a Bluetooth controller to capture data packets and other valuable information
parent: Resources
nav_order: 5
lang: en
page_id: BSAM_resources_06
permalink: resources/enable-bluetooth-debug-mode/
---

# {{ page.summary }}

{: .note }
> This resource relies on unofficial or undocumented functionalities. There is no known maintenance of these tools so it is not possible to ensure that they may work correctly in all scenarios.

Some manufacturers seem to include undocumented debug or diagnostic modes in their devices.

These diagnostic modes are not standardised but enable functionalities not available in common modes of operation. Examples of such functionalities are the ability to display all sent and received Link Layer packets.

Some known procedures for enabling these debug modes are listed below.


## Broadcom/Cypress

| Supported Devices         |
|:--------------------------|
| Cypress CYW20735B1        |
| Cypress CYW20819A         |

This procedure is only documented on Linux. All the devices listed above expose a UART interface when connected to a machine. To be recognised as Bluetooth devices, it is necessary to run the `btattach` process to flag them as bluetooth capable. This may be done using the [Bluetooth Attach Service](https://github.com/TarlogicSecurity/Bluetooth-Attach-Service) tools.

When running the `btattach` process, some devices expose a special interface via debugfs. To enable diagnostic mode, write a `1` to the `vendor_diag` file for your device.

```
echo 1 > /sys/kernel/debug/bluetooth/hciX/vendor_diag
```

If our board does not expose the `vendor_diag` file, there is the possibility to enable diagnostic mode via a manufacturer-specific HCI command. This can be done with `hcitool`:

```bash
hcitool cmd 0x3f 0xf0 0x01
```
