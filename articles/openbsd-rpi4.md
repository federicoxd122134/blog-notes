---
publishDate: 'May 27 2024'
title: 'Installing OpenBSD on Raspberry Pi4'
description: 'How to install OpenBSD on Raspberry Pi4 in 2024 without using a serial adapter'
image: '~/assets/images/openbsd-rpi.png'
tags: [linux]
---

Installing OpenBSD on a Raspberry Pi 4 has been a challenge for me, specifically with respect to setting up the EFI boot partition. U-Boot is not working as of 05/27/2024, and although most people advise using EDK II, information about how to install it is quite outdated. This document tries to describe how to install OpenBSD on an RPi4 in 2024. If you have any questions and/or want to contribute, you can contact me through any of the social media that appear in this blog. All references can be found at the end of the document in case you need them. Let's start!

## Ingredients

* 1 Raspberry Pi4
* 1 Micro SD card
* 1 USB drive
* HDMI cable
* Keyboard & mouse
* Operating system running GNU/Linux or similar

## Preparation

The first step is to install Pi OS on a microSD card. You can use the same SD card that we will use for installing OpenBSD, but you can also use another if you want to keep Pi OS, which is really useful.

1. Install Pi OS on a microSD card
2. Update Pi OS
3. Set the EEPROM release
4. Update the EEPROM & reboot
5. Set the boot order

## Installation

1. Get the EDK II Bootloader
2. Get the OpenBSD arm image
3. Erase the sd card and the USB drive
4. Copy the install image to the USB drive
5. Replace the boot partition on the USB drive

## Installing OpenBSD on the SD card

1. Install OpenBSD on the SD card
2. Change the console on the new system
3. Replace the firmware on the new system

## Post installation

1. Modify RAM and device table settings
2. Change the boot order in the firmware

## Closing remarks

## References

* openbsdhandbook.com
* mtsapv.com/rpi4obsd
* undeadly.org/cgi?action=article&sid=20170409123528
* github.com/AshyIsMe/openbsd-rpi4
* gist.github.com/astreknet/8e177cfb2a0900efa385e6062ae8f7a7

## set up chrony history update
