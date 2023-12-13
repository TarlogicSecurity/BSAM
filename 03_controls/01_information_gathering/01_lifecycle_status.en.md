---
layout: control
title: BSAM-IG-01
summary: Bluetooth controller lifecycle status
description: How to check the lifecycle of your Bluetooth controller to ensure its security.
parent: Information gathering
grand_parent: Controls
nav_order: 1
lang: en
page_id: cont_info_01
permalink: controls/bluetooth-controller-lifecycle-status/
tags:
- BR/EDR
- BLE
---

A Bluetooth controller, when it is at the end or has already finished its product life cycle, will not receive updates from the manufacturer, meaning that the identification of vulnerabilities in its firmware or any other component will make the vulnerability permanent in the device and there will not be an official way to fix it.

Therefore, it is important to check at what point in the lifecycle the Bluetooth controller is, as this can indicate whether it will be necessary to replace the device in the short, medium, or long term.


## Description

After identifying the Bluetooth controller, it will be necessary to consult the manufacturer's documentation to find out at which point in the life cycle it is.


You can find a list of controllers that have already reached an end of support status in the [End of life Bluetooth controllers]({% link 05_annex/01_eol_controllers.en.md %}) annex. Alternatively, if the device is not listed, it will be necessary to consult the manufacturer's website, different supplier websites, or various search engines. 
In addition to common search engines and the manufacturer's page, searches are recommended on the following providers/manufacturers:

| Provider  | URL                                                                       |
|:----------|:--------------------------------------------------------------------------|
| Mouser    | <https://www.mouser.es/>                                                  |
| RS        | <https://es.rs-online.com/web/>                                           |
| Farnell   | <https://es.farnell.com/>                                                 |
| Octopart  | <https://octopart.com/>                                                   |
| Digikey   | <https://www.digikey.es/>                                                 |
| TME       | <https://www.tme.eu/>                                                     |
| Element14 | <https://sg.element14.com/>                                               |
| Avnet     | <https://www.avnet.com/>                                                  |
| Microchip | <https://www.microchip.com/product-change-notifications/>                 |
| Infineon  | <https://www.infineon.com/cms/en/product/search/discontinued-products/>   |


## Related resources

The resources that can be useful for the identification of the controller model in the device can be found in the following table:

{% include res_table.md resources='BSAM-RES-01,BSAM-RES-02' %}


## Example

Upon disassembly of a Bluetooth device ({% include res_link.md res='BSAM-RES-01' %}) an `nRF51822` controller is identified. A search in Mouser's website indicates that this controller is not recommended for new designs because the manufacturer has issued this recommendation for having reached the end of its lifecycle and there are products that replace it.

![The Mouser provider indicating that the controller is not recommended for new designs]({{ '/assets/img/bsam-ig-01_nrnd.png' | relative_url}})

The manufacturer also lists an informational note on their website about a [vulnerability affecting all nRF51 family devices](https://infocenter.nordicsemi.com/pdf/in_119_v1.0.pdf). Had the device been out of support at the time, the vulnerability would not have been patched, leaving the customer exposed.

This is important because it is possible that the controller will not be supported for fixes affecting security in a timeframe shorter than the support to be given to the device under study.
