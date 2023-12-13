---
layout: default
title: BSAM-RES-10
summary: Link key management on the host
description: Link key management on the host allows controlling the controller's responses to Bluetooth connection attempts
parent: Resources
nav_order: 9
lang: en
page_id: BSAM_resources_10
permalink: resources/link-key-management/
---

# {{ page.summary }}

The host software stack, among many other functions, is responsible for link key management.

Depending on the presence of link keys for connection to another device, the host is able to control (via the HCI protocol) some responses that the driver makes to an incoming or outgoing connection attempt. Although there are alternatives to this technique, such as the implementation of our host stack, it is sometimes easier to install or delete keys from another device on our host through APIs for this purpose.

This resource is dependent on the host stack on which it is attempted.


## BlueZ link key management

The BlueZ Bluetooth stack exposes different APIs to interact with it. The management (MGMT) API, described in the [mgmt-api.txt](https://github.com/bluez/bluez/blob/master/doc/mgmt-api.txt) file, allows manipulation of link keys, among other tasks.

As indicated in the document, to use the API, a Bluetooth Management socket must be opened and the message format must be followed.

The [Load Link Keys](https://github.com/bluez/bluez/blob/8c452c2ec1739efe581273bacd738e5294d0ca0f/doc/mgmt-api.txt#L788) command, in particular, allows you to modify the keys stored in the database: you pass it an array of keys to store. An empty array removes all keys from the database.

To facilitate the use of this API, BlueZ provides the [btmgmt](https://github.com/bluez/bluez/blob/master/tools/btmgmt.c) tool. Although it does not implement all the commands specified in the API, it implements many of them and can serve as a base. The Load Link Keys command is not implemented in the tool, but there is a command line option and it does not require much effort to add the functionality.
