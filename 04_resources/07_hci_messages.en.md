---
layout: default
title: BSAM-RES-07
summary: Sending and receiving HCI messages
description: Send and receive HCI messages to control the Bluetooth controller and get information about the device
parent: Resources
nav_order: 6
lang: en
page_id: BSAM_resources_07
permalink: resources/send-receive-hci-messages/
---

# {{ page.summary }}

## Host Controller Interface

The controller device communicates with the host via Host Controller Interface (HCI) messages. The HCI interface defines and limits the control that can be exercised from the host and is the basis of any desktop tool.

HCI transports L2CAP data between host and controller via HCI Data Packets, but also allows interaction with the controller via two other types of messages:

| Message type    | Description |
|:----------------|:------------|
| HCI Command     | Contains an instruction sent from the host to the controller |
| HCI Event       | Contains an event sent from the controller to the host       |

HCI Command messages contain an instruction to the controller and are always sent from the host. HCI Event messages are always sent from the controller to the host and can contain different data:
- Upon receipt of an instruction, the controller can respond with a status message (Command Status Event).
- After completing a process requested by the host, the controller can issue a message with the result of the process (Command Complete Event).
- The controller can issue an event with other information in an unsolicited manner.

Each HCI Command is assigned a 2-byte Opcode as a unique identifier. The 6 most significant bits of the Opcode indicate the Opcode Group Field (OGF) and the 10 least significant bits indicate the Opcode Command Field (OCF): the OGF classifies messages into groups and the OCF indicates the message within the group. The following example shows Opcode `0x0401` (`0b00000100000000000001`), which corresponds to the HCI Inquiry message (OCF `0x0001`) of the Link Control group (OGF `0x01`):

| OGF      | OCF          |
|:---------|:-------------|
| 0b000001 | 0b0000000001 |

Events are identified by a unique 1-byte _event code_ assigned in the standard.

The complete listing of the standard HCI Command and HCI Event messages with their parameter descriptions can be found in Bluetooth Core Specification v5.3, Volume 2, Part E (Host Controller Interface Functional Specification), although the organisation and content may vary between versions of the document.

In addition to the standard messages, controller manufacturers can implement _vendor-specific_ messages with extra functionality using the OGF `0x3f`. Some manufacturers implement debugging functionalities through this mechanism that allow for more complete control of the driver. Some Broadcom boards, for example, allow receiving LMP messages exchanged between devices.

References:
* Bluetooth Core Specification v5.3, Volumen 2, Parte E (Host Controller Interface Functional Specification)


## HCI tools

The BlueZ project, along with the commonly used Bluetooth stack in Linux, distributes different tools that allow interacting with Bluetooth devices at different levels. `hcitool` and `hciconfig`, in particular, allow sending different HCI messages, configuring connections and querying parameters.

Despite being considered obsolete and unsupported, some of their functionalities are not covered by the more modern tools that replace them, such as `bluetoothctl`.

Both tools are normally available as packages in the various Linux distributions. On Arch Linux, both are available in the AUR package `bluez-utils-compat`. On Kali Linux, they are in the `bluez` package.

To send an HCI message not supported by any of the tools, you can use the `cmd` option of `hcitool`, which allows you to send an HCI Command message with Opcode and arbitrary parameters:

```bash
hcitool cmd <ogf> <ocf> [parameters]
```

The following example sends a _vendor-specific_ message for Broadcom boards that triggers the debug log with LMP messages exchanged between devices:

```bash
hcitool cmd 0x3f 0xf0 0x01
```

References:
* [bluez source](https://github.com/bluez/bluez)
* [hcitool source](https://github.com/bluez/bluez/blob/master/tools/hcitool.c)
* [hciconfig source](https://github.com/bluez/bluez/blob/master/tools/hciconfig.c)


## Libraries and APIs

In addition to the tools shown above, operating systems such as Linux offer APIs through which to interact with the Bluetooth stack and send messages at different levels.

In Linux, you can interact with HCI through sockets, using the `BTPROTO_HCI` protocol when creating the socket. Examples of the use of HCI sockets can be found in the BlueZ repository tools.

In Python, there is the PyBluez API, which allows interacting with HCI through the `BluetoothSocket` class, however, it is recommended to use the Scapy module, which facilitates sending packets and is easily extensible.

Many of the HCI packages are not implemented and need to be specified as an extension to the available libraries, so Scapy facilitates much of the task.

References:
* [btmon source](https://github.com/bluez/bluez/blob/master/tools/btmon-logger.c)
* [bluez tools source](https://github.com/bluez/bluez/tree/master/tools)
* [PyBluez](https://pybluez.readthedocs.io/en/latest/)
* [Scapy](https://scapy.net/?ref=glue)
