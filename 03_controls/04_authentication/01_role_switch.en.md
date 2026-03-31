---
layout: control
title: BSAM-AU-01
summary: Role switch before authentication
description: Bluetooth devices should avoid allowing role switching from slave to master before authentication. This can be exploited by attackers to avoid authentication
parent: Authentication
grand_parent: Controls
nav_order: 1
lang: en
page_id: cont_auth_01
permalink: controls/bluetooth-role-switch-authentication/
tags:
- BR/EDR
---

In Bluetooth Classic (BR/EDR), devices operate under two possible roles:

* Central: Sets the physical parameters of the connection.
* Peripheral: Follows the Central's instructions.

When a connection is initiated, the initiating device automatically assumes the Central role.

Role switching from Central to Peripheral is sometimes used by devices that are designed to operate only as Peripherals. However, this mechanism can be exploited to bypass authentication, since in legacy authentication only the Central authenticates the Peripheral. Attacks such as BIAS take advantage of this behavior by forcing a role switch just before authentication, allowing an attacker to impersonate a previously paired device.

To reduce this risk, the Peripheral to Central role change mechanism should be avoided unless it is strictly required for normal device operation.


## Description

The role change message is produced with LMP messages and is therefore dependent on the firmware of the Bluetooth driver.

To check whether a device allows a role change from slave to master, it is necessary to have a driver that allows sending LMP role change messages at arbitrary moments of the communication.

Modifications to the CYW920819WCD2 driver firmware made as part of the [BIAS](https://github.com/francozappa/bias) PoC include a patch to change the role of the device from slave to master prior to authentication, so in conjunction with Wireshark it can be used to test control, although this requires the CYW920819EVB-02 development board, or one compatible with the "Patch ROM" mechanism.

The process consists of patching the firmware on the board (an example of how to do this is available at [the BIAS repository](https://github.com/francozappa/bias/blob/master/bias/bias-template.py)) and using the board to initiate a connection to the audited device while capturing the communication with Wireshark. Before starting the communication it is necessary to activate the debug messages on the board, which include the LMP messages sent and received. In addition, traffic must be captured through the bluetooth monitor interface and the Broadcom debug message dissector must be used to observe and interpret the LMP messages correctly.

Just before authentication, you will observe an `LMP_role_switch` message being sent. If the message receives an `LMP_accepted` response, the device is not in compliance with the control.


## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07,BSAM-RES-09' %}

## Example case

We will use Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) to capture packets for analysis.

There is a laptop with the Scapy tool that is performing pairing with Peripheral role to another device with the Central role. During the connection process is possible to perform an additional step called _role switch_.

![Bluetooth Core 5.3 Connection Establishment]({{ '/assets/img/bsam-au-01_connection_stablishment.png' | relative_url}})

The peripheral device receives the central device connection request through the _HCI\_Connection\_Request_ command.

![Wireshark Connection request]({{ '/assets/img/bsam-au-01_connection_request.png' | relative_url}})

The Peripheral device response by accepting the connection using the _HCI\_Accept\_Connection\_Request_ command. It communicates its intention to function as a Central device by configuring the Role field with a value of _Role_ field with value `0x00` (`Become Central for this connection. The LM will perform the role switch.`).

If the remote device accepts this role change, it will respond with the _HCI\_Role\_Change_ command, and the Status field will have a value of `0x00` (`A Role change has ocurred.`). Any other value would indicate an error code for the procedure.

To determine whether this role switch is occurring before the authentication process, the _HCI\_Link\_Key\_Request\_Reply_ command followed by the _HCI\_Command\_Complete_ command should be located in the Wireshark capture.

If the _HCI\_Accept\_Connection\_Request_ command is before the two commands (_HCI\_Link\_Key\_Request\_Reply_ / _HCI\_Command\_Complete_), it indicates a _role switch_ before the authentication stage.

The check control _FAIL_ if the _HCI\_Role\_Change_ command with the _Status_ field set to `0x00` is found before the authentication of the devices.