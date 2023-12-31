---
layout: control
title: BSAM-PA-07
summary: Predictable PIN codes
description: Check that your Bluetooth device does not generate predictable PIN codes. This is important to prevent an attacker from being able to decrypt the PIN code and take control of the pairing process
parent: Pairing
grand_parent: Controls
nav_order: 7
lang: en
page_id: cont_pair_07
permalink: controls/bluetooth-pin-code-predictable/
tags:
- BR/EDR
- BLE
---


One of the methods used during pairing to generate the shared key between the devices is to share a PIN code randomly generated by one of the devices. From this PIN number, which the user must enter on the second device, the link key is generated, so it is essential to avoid the number generated being predictable.

It is crucial that the source used to generate PIN codes on a device is unpredictable. To achieve this, we must avoid the use of rudimentary pseudo-random number generators, such as incremental sums or fragments of the "timestamp" from the "epoch," and other similar methods. Instead, it is recommended to opt for _RNG_ (Random Number Generator) integrated into microcontrollers. While these generators may not be considered _True RNG_ (True Random Number Generators), they have a sufficiently high level of entropy to avoid being a vulnerability in themselves. This will help prevent third parties from predicting the access PIN and, consequently, deriving access keys to the connection.


## Description

To check the validity of the control, multiple pairings must be performed to collect data to detect patterns in the generated PIN numbers.

Some easily identifiable patterns are as follows:
* Successive PIN numbers: devices can be found that constantly increase the PIN number.
* Repeated PIN numbers: devices can be found that use pseudo-random generators that enter a loop after the generation of a known number of PIN codes.

The use of statistical tools or test suites for cryptographic fonts is recommended although they may require more time to study. Some of the most comprehensive tools and tests are listed below:
* DieHard statistical test
* NIST Cryptographic Algorithm Validation Program Testing: Random Number Generators

The control is fulfilled if no pattern can be determined in the received PIN numbers or ideally, if the sources of entropy for the generation of PIN codes is strong against the mentioned tests.


## Related resources

To check this control, the following resources may be useful:

{% include res_table.md resources='BSAM-RES-04,BSAM-RES-05,BSAM-RES-07' %}


## Example case

The security of the Bluetooth communications of a car radio/infotainment unit is evaluated. After verifying the control {% include ctl_link.md ctl='BSAM-PA-02' %}, it is concluded that the analyzed device exposes physical capabilities according to the available hardware, in this case a display and a YES/NO input since there is no full keyboard. Due to these input/output capabilities, the most secure authentication method involves PIN comparison generated by automatic methods based on secure entropy sources.

To evaluate the security of the system PIN generation, multiple pairing processes are initiated with a cell phone by capturing them using the technique {% include res_link.md res='BSAM-RES-05' %}. During this process it is evident that the device consistently displays an incremental PIN with the values `0000`, `0001`, `0003`, and so on.

Therefore, this device does not overcome the control since the generation of PIN codes is predictable, so it is possible to take control of a pairing process. 
