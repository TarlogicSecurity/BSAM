---
layout: control
title: BSAM-EN-01
summary: Role changes before encryption
description: Do not allow the role of the Bluetooth device to be changed after authentication.
parent: Encryption
grand_parent: Controls
nav_order: 1
lang: en
page_id: cont_enc_01
permalink: controls/role-switch-before-bluetooth-encryption/
tags:
- BR/EDR
---


In a Bluetooth communication there are two defined roles: 
 * _Master_: Is the main role of a "piconet" (network of Bluetooth devices) and defines the physical parameters of the connections.
 * _Slave_: Device that simply follows the steps of the master.
 
Role switching can be used by devices that can only act as slaves to avoid being masters of the connection. This role switching procedure usually takes place during the establishment of a connection and always before the authentication phase.

If it takes place between the authentication phase and the encryption phase, it could be used to attack the encryption phase and try to extract the temporary key used during a connection, attacking the confidentiality of the data of that connection. Therefore, it is preferable to deny the use of the role change mechanism after the authentication step if it is not necessary for the normal operation of the device.


## Description

Since the role change occurs via a link layer mechanism (_LMP_ or _Link Layer_), there are two possible options to verify that the device does not allow a role change after authentication:
* By reviewing the source code of the driver firmware.
* By sending a role change request after the authentication process, the response is captured to evaluate whether the change is allowed.

In order to send a role change request after the authentication process it is necessary to modify the usual flow of LMP messages and, therefore, it is necessary to have the ability to send and receive LMP messages. For this, it is usually necessary to modify the firmware of the Bluetooth driver used to perform the audit (not that of the audited device).

Modifications are required so that an LMP_switch_req message is sent just after authentication and before the start of encryption. The audited device may respond with an LMP_accepted message, indicating that it has accepted the role change and is not compliant, or with LMP_not_accepted, which would be the expected response from a compliant device.

In addition, to capture the response, it is necessary to be able to capture LMP messages, for which Bluetooth driver debug messages can be used: Broadcom drivers redirect LMP traffic to the host in the form of debug messages.

Para la modificación del firmware, es necesario disponer de un controlador Bluetooth que permita estas modificaciones y del que se disponga del código del firmware (o del que exista un trabajo de ingeniería inversa). Las modificaciones al firmware de la placa de desarrollo Cypress CYW920819EVB-02, realizadas para la investigación de [BIAS](https://francozappa.github.io/about-bias/), son de utilidad en este aspecto.

## Related resources

To check this control, the following resources can be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-06,BSAM-RES-07,BSAM-RES-09' %}

## Example case

We will use Wireshark with [BTVS](https://learn.microsoft.com/en-us/windows-hardware/drivers/bluetooth/testing-btp-tools-btvs) (btvs.exe -Mode wireshark) (btvs.exe -Mode wireshark) to capture packets for analysis.

There is a laptop with the Scapy tool that is performing pairing with the Peripheral role to another device with the central role. During the connection process, there is the possibility of performing an additional step called role switch.

![Bluetooth Core 5.3 Connection Establishment]({{ '/assets/img/bsam-au-01_connection_stablishment.png' | relative_url}})

The Peripheral device receives the central device connection request through the _HCI\_Connection\_Request_ command.

![Wireshark Connection request]({{ '/assets/img/bsam-au-01_connection_request.png' | relative_url}})

The Peripheral device response can accept the connection with the _HCI\_Accept\_Connection\_Request_ command, indicating its preference to work as a Central device by setting the _Role_ field to a value `0x00` (`Become Central for this connection. The LM will perform the role switch.`).

If the remote device accepts this role switch, it will respond with the _HCI\_Role\_Change_ command, and the Status field will have a value of `0x00` (`A Role change has occurred.`). Any other value would indicate an error code for the procedure.

To determine whether this role switch is occurring before or after the authentication process, the _HCI\_Link\_Key\_Request\_Reply_ command followed by the _HCI\_Command\_Complete_ command should be located in the Wireshark capture.

If the _HCI\_Accept\_Connection\_Request_ command appears after the two commands (_HCI\_Link\_Key\_Request\_Reply_ / _HCI\_Command\_Complete_), it indicates a _role switch_ before the encryption stage.

The check control _FAIL_ if the _HCI\_Role\_Change_ command with the _Status_ field set to `0x00` is found before the encryption of the devices.