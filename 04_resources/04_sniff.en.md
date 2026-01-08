---
layout: default
title: BSAM-RES-04
summary: Bluetooth connections sniffing 
description: Bluetooth connections sniffing is the process of capturing the traffic of a Bluetooth connection to analyze it. This technique can be used to detect vulnerabilities or perform attacks
parent: Resources
nav_order: 3
lang: en
page_id: BSAM_resources_04
permalink: resources/bluetooth-connection-sniffing/
---

# {{ page.summary }}

This resource consists on capturing traffic from a Bluetooth connection using specific hardware with the ability to intercept packets of third-party setup networks. It is a technique similar to the "monitor mode" in Wi-Fi.

This technique commonly captures packets from the Bluetooth "Link Layer" layer, i.e. "LMP" packets in BR/EDR and "LL" packets in BLE. Depending on the connection being monitored, it is possible that the captured packets are encrypted.

## Bluetooth Sniffers

The following table lists some hardware and software that allows this technique to be performed. It is important to check the limitations of the projects below as many do not allow reliable capture of communications due to the channel hopping techniques used in Bluetooth.

| Hardware                                                                                         | Software                                                                                                           | Modes              |
| :----------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------- | :----------------- |
| Ubertooth                                                                                        | [Ubertooth tools](https://github.com/greatscottgadgets/ubertooth)                                                  | BR* / EDR* / BLE   |
| TI CC1352/CC26x2                                                                                 | [Sniffle](https://github.com/nccgroup/Sniffle)                                                                     | BLE 4.x / BLE 5    |
| nRF51822                                                                                         | [Btlejack](https://github.com/virtualabs/btlejack)                                                                 | BLE 4.x / BLE 5.x* |
| Bluefruit LE sniffer                                                                             | [Btlejack](https://github.com/virtualabs/btlejack)                                                                 | BLE 4.x / BLE 5.x* |
| Micro:Bit                                                                                        | [Btlejack](https://github.com/virtualabs/btlejack)                                                                 | BLE 4.x / BLE 5.x* |
| nRF52840                                                                                         | [nRF Sniffer](https://infocenter.nordicsemi.com/index.jsp?topic=%2Fug_sniffer_ble%2FUG%2Fsniffer_ble%2Fintro.html) | BLE                |
| [PANalyzr](https://spanalytics.com/product/panalyzr/)                                            | -                                                                                                                  | BR / EDR / BLE     |
| [Ellisys Bluetooth Vanguard](https://www.ellisys.com/products/bv1/index.php)                     | -                                                                                                                  | BR / EDR / BLE     |
| [Ellisys Bluetooth Explorer](https://www.ellisys.com/products/bex400/index.php)                  | -                                                                                                                  | BR / EDR / BLE     |
| [TeledyneLecroy Frontline X500](https://teledynelecroy.com/protocolanalyzer/frontline-x500.aspx) | -                                                                                                                  | BR / EDR / BLE     |
| [Sena-UD100](http://www.senanetworks.com/ud100-g03.html)                                         | -                                                                                                                  | BR / EDR / BLE     |

\* Limited support. See product or project for more information.
