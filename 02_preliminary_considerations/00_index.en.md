---
layout: default
title: Preliminary considerations
description: Preliminary considerations for performing a Bluetooth security assessment.
nav_order: 2
has_children: false
lang: en
page_id: prel_index
permalink: preliminary-considerations/
---

# Preliminary considerations for security assessment
This section is devoted to considerations prior to conducting a Bluetooth security assessment.

General considerations focus on those pre-assessment considerations that affect all security assessments equally. Particular considerations are those that vary between different assessment exercises.

## Laws and regulatory limitations on wireless communications

Before assessing a device, it is necessary to consult and understand the laws that regulate the radio spectrum in the place where the audit will be conducted.

It is also essential to take the necessary preventive measures to not break the law, both in activities involving listening and broadcasting in the radio space.

There are very specific limitations regarding transmission powers and the use of different frequencies, as well as the emission of signals that may cause interference. If it is necessary to perform tests outside what is stipulated by law, it will be necessary to resort to mitigating elements such as Faraday cages to isolate the test signals.

## Interaction with devices

Intrusive tests should not be performed, nor should devices be interacted with by sending packets until it is clearly identified that the device we are going to interact with is withing the scope of the assessment and we are authorized to perform tests on it.

## Types of analysis

Depending on the amount of information available during a security assessment, the following perspectives can be considered:

* **Black box**: Analysis without prior information or from a complete lack of knowledge about the device. It's an approach that allows getting a global view of a device's security status. It simulates an external attack attempt.

* **Gray box**: In this approach, it is expected that the customer provides data about the device with the aim of reducing the time necessary for the information gathering process. This allows the security analyst to guide and focus testing efforts on the evaluation of controls more suitable to the device analysed.

* **White box**: In white box analysis, the scope of the audit is clearly defined. The customer provides as much information as possible to facilitate a review process and guide the analyst to critical points. It is an appropriate approach for reviewing the evolution of a device in terms of security after applying technical modifications.


## Work authorization

Before starting a security analysis of a device that does not belong to us, it is necessary to have an accreditation or work order signed by the company for whom the work is being done.

This work order should clearly and concisely identify the following elements:

* Company name (customer)
* Contact person
* Contact phone
* Person or company performing the analysis
* Time duration

In certain scenarios, such as security reviews in office buildings, shopping centres, or security zones where sensitive agencies are located (government agencies, airports...), it may be necessary to request third-party authorization before performing the analysis.