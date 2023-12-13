---
layout: default
title: BSAM-RES-01
summary: Physical identification of the controller
description: Identify the Bluetooth controller of your device by disassembling it or searching for information about it online.
parent: Resources
nav_order: 0
lang: en
page_id: BSAM_resources_01
permalink: resources/identify-bluetooth-controller-physically/
---

# {{ page.summary }}

It is sometimes possible to identify the Bluetooth controller of a device by disassembling the device and analyzing the printed circuit boards of the device.

To identify the Bluetooth controller several strategies can be used.

One possible strategy is to try to locate discrete antennas or antennas on the PCB itself. Bluetooth chips and/or other communication chips will be physically close to these antennas. An example of this can be identified in the following image where a PCB antenna can be recognized on the left of the picture (snaking copper track only connected at one end). If we follow the track, after passing through some very small components it ends up connected to a chip marked `CSR BC417`, a Bluetooth controller.

![PCB antenna on the left near a CSR BC417 bluetooth chip]({{ '/assets/img/bsam-res-01_bc417.png' | relative_url}})

Another alternative could be to search for the silkscreen of all the chips present on a PCB in search engines or on the website of the different chip manufacturers found.

This search is also useful to verify that our candidate found by the first strategy really is a Bluetooth driver.

## Recommendations

- It is advisable to photograph or record the disassembly process. If you have any doubts about the assembly process, you can consult the video of the disassembly.

- It is interesting to take detailed photographs of all the printed circuit boards and chips contained in the device. These images can be used for further analysis or for later documentation in the report.
