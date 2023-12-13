---
title: Bluetooth Security Assessment Methodology
description: The BSAM methodology is a guide for security evaluation in devices with Bluetooth capabilities.
image: /assets/img/BSAM.png
layout: home
nav_order: 0
lang: en
page_id: index
permalink: /
---

![BSAM - Bluetooth security assessment methodology logo]({{ '/assets/img/BSAM.png' | relative_url}})

# BSAM: Bluetooth Security Assessment Methodology

**BSAM** is the acronym for **Bluetooth Security Assessment Methodology**. BSAM is an open and collaborative methodology developed to standardize the security evaluation of devices using Bluetooth technology.
 
The BSAM methodology defines all the necessary [Bluetooth security controls]({% link 03_controls/00_index.en.md %}) to provide manufacturers, security researchers, software developers, enthusiasts, and cybersecurity professionals with a guide for conducting security assessments on devices with Bluetooth communications.

BSAM is a fundamental pillar for assessing the security of communications in IoT devices with Bluetooth and Bluetooth LE capabilities and to help plan **penetration testing**.

## Impact of Bluetooth on security

The Bluetooth standard is constantly evolving, and Bluetooth technology is today integrated into a wide range of devices, such as:

* **Personal devices:** Laptops, smartphones, or tablets use Bluetooth to connect to input devices such as keyboards, headphones, watches, VR headsets, or gaming consoles.
* **Transportation devices:** Onboard entertainment and vehicle control systems, autonomous vehicles, or traffic control systems.
* **Home devices:** Smart locks, smart speakers, smart lights, smart thermostats, appliances, and other smart home sensors.
* **Health and fitness devices:** Fitness trackers, heart rate monitors, and medical devices to collect and transmit data about users' health and fitness.
* **Industrial devices:** Industrial robots, sensors, and controllers that use Bluetooth to communicate with each other.

Bluetooth is widely used in devices that impact cybersecurity, safety, and privacy. The complexity of the Bluetooth standard and the variability in its configuration make it difficult to implement effective security measures. BSAM standardizes **Bluetooth security testing** to ensure that Bluetooth devices are secure and private.

## Structure of the BSAM methodology

To familiarize yourself with the Bluetooth standard, you can check out the [Documentation section]({% link 01_documentation/00_index.en.md %}). This section contains basic information about where to obtain additional technical details and summaries of the technology behind Bluetooth.

Before starting a security audit, it is important to know the list of [preliminary considerations]({% link 02_preliminary_considerations/00_index.en.md %}) of the methodology in order to understand the advantages and disadvantages of the proposed analysis, as well as to make sure that the law is abided at all times.

The proposed methodology consists of multiple [controls]({% link 03_controls/00_index.en.md %}) to be evaluated individually to verify that the device is secure from a Bluetooth communications point of view.

The controls in this methodology focus on the checks to be performed but not on how they are to be performed. To facilitate the work, some [resources]({% link 04_resources/00_index.en.md %}) are listed to enable the execution of the checks.

## Status of the Bluetooth security guide

The BSAM project aims to create a complete **Bluetooth security guide**, and is therefore in constant development. This Bluetooth methodology is not considered finished and is in a state where it could be subject to major modifications affecting its content and even its structure!

Listed below are some of the major tasks to be carried out in the future:
* Expand the documentation section.
* Expand the resources section.
* Relate controls to security standards and regulations like GSMA IoT Security Assessment, IEC 62443 industrial cybersecurity standard, ETSI EN 303645 .

This methodology has been developed by [Tarlogic](https://www.tarlogic.com/) under the [CC BY](https://creativecommons.org/licenses/by/4.0/) license.

<a href="https://github.com/TarlogicSecurity/BSAM" class="btn btn-primary">View it on GitHub</a>