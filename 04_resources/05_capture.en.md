---
layout: default
title: BSAM-RES-05
summary: Capture of a Bluetooth connection
description: Capture of a Bluetooth connection between a device and its controller to analyze Bluetooth traffic.
parent: Resources
nav_order: 4
lang: en
page_id: BSAM_resources_05
permalink: resources/capture-bluetooth-connection/
---

# {{ page.summary }}

This resource consists on capturing packets of a connection with the same device with which the Bluetooth connection is made. In short, the process consists of capturing the packets exchanged between the "Host" and the "Controller" of a device while it is connected to another device. The protocol used to exchange messages between the controller and the "Host" is HCI (Host Controller Interface). All the tools listed below are used to capture and/or store HCI protocol packets in one or the other format.

Since this technique only captures the HCI protocol, many of the Link Layer packets exchanged between controllers will not be present in the capture. There are debugging mechanisms by which it is possible to capture these packets, but they are only available on specific hardware. See {% include res_link.md res='BSAM-RES-06' %} for more information.

This technique requires tools and procedures that depend on the operating system in use. Some alternatives for the most common operating systems are listed below.


## Capture Bluetooth connection on Linux

In Linux we have several alternatives to capture packets from established connections to our machine. Almost all Linux machines with Bluetooth support have the `BlueZ` stack installed, so the `btmon` tool is a good alternative for capturing without having to install many dependencies. `Wireshark` is one of the most attractive alternatives as it has the ability to graphically dissect packets. For cases where we want to interact programmatically with packages or perform automated captures, `Scapy` is a good alternative.

References:

* [btmon](https://man.archlinux.org/man/extra/bluez-utils/btmon.1.en)
* [Wireshark](https://wiki.wireshark.org/Bluetooth)
* [Scapy](https://scapy.readthedocs.io/en/latest/layers/bluetooth.html)


## Capture Bluetooth connection on Windows

Windows also offers a native Bluetooth packet dump tool known as `BTVS` or `Bluetooth Virtual Sniffer` with the ability to dump in `Wireshark` format for further analysis. `Wireshark` also supports Bluetooth packet dump directly in Windows.

References:

* [Wireshark](https://wiki.wireshark.org/Bluetooth)
* [Bluetooth Virtual Sniffer](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs)


## Capture Bluetooth connection on MacOS, iOS, tvOS & watchOS

On Apple platforms, the only supported method for Bluetooth packet dumping is the `PacketLogger` debugging tool. It is compatible with all Apple platforms and allows to export the captures afterwards to a `Wireshark` compatible format.

References:

* [PacketLogger](https://developer.apple.com/bug-reporting/profiles-and-logs/?name=bluetooth)


## Capture Bluetooth connection on Android

Android natively supports the export of communication logs using a mechanism known as `btsnoop` or `Bluetooth HCI snoop log`. There are Frida scripts such as `Frida BLEMon` that allow you to implement Bluetooth API calls to generate Bluetooth communication dumps.

The extraction of the `btsnoop` logs is different depending on the Android device and its manufacturer, although some of the steps might be common.

### Developer Options

On most Android devices, the `Bluetooth HCI snoop log` is a developer feature, so the developer options must be activated first.

Go to `Settings > About phone` and tap repeatedly on the `Build number` until a bubble indicates that the developer options have been activated. 

### Extracting btsnoop logs on OxygenOS 11.1.2.2

With the developer options activated, go to `Settings > System > Developer Options` and tap on `Enable Bluetooth HCI snoop log` to enable it. Then, stop and start the Bluetooth functionality. Now the HCI messages are being stored in a log, as long as the `Bluetooth HCI snoop log` remains enabled.

To extract the log from the phone, go to `Settings > System > Developer Options > Get logs` and start the capture of `Bluetooth Exception`, tapping on `NOT REBOOT` to avoid rebooting. This is only to extract the previously generated log file, it's not necessary to repeat the tests while capturing with `Bluetooth Exception`.

After a few seconds, stop the `Bluetooth Exception` capture and wait until the report is generated. When it's finished, tap on `SHARE` and select the `btsnoop` folder. Then tap on the menu `(...)` and on `Share`. This allows to send the `btsnoop` folder, containing every `HCI snoop log` generated, to some other place where they can be analyzed. 

References:

* [Btsnoop.log](https://source.android.com/docs/core/connect/bluetooth/verifying_debugging?hl=es-419#debugging-with-logs)
* [Frida BLEMon](https://github.com/optiv/blemon)
