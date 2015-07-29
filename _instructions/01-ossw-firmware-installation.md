---
layout: subpage
title: Firmware installation
description: how to install an OSSW firmware on a Weloop Tommy/Chronos Eco watch.
permalink: instructions/ossw-firmware-installation/index.html
---

##How to install OSSW

First we need to get rid of the old bootloader on the watch and then overwrite the firmware. I assume you haven't modified your watch in any way. Just the plain, out of the box watch with Weloop Tommy or Chronos Eco firmware. Because that may cause problems.

You will need:

- Android 4.3+ Phone with [nRF Toolbox](https://play.google.com/store/apps/details?id=no.nordicsemi.android.nrftoolbox&hl=en) installed
- Latest [ossw-installer](https://github.com/ossw/ossw-installer/releases) release (most recent is ossw-installer-combined-0.3.bin)
- Latest [ossw-firmware-s120](https://github.com/ossw/ossw-firmware-s120/releases) release (most recent is ossw-firmware-0.2.0.zip)

Save those two files as it is (don't extract the zip) on your phone and start nRF Toolbox. Select DFU and turn your bluetooth on. Important: Put your watch in OTA Mode. First select the binary installer file and pick the Weloop Tommy/Chronos Eco watch as device. You have to choose "Application" and "Init packet: No" when uploading this file.

Wait patiently while the update is doing its magic.

Next install the firmware: You have to choose "Distribution packet (ZIP)" when uploading this file. When everything is done go outside and make a happy dance :)

##Problems while updating...

- When updating from the original bootloader (first file) the watch will reset every 60 seconds while in OTA mode - you have to start the updating process right after a reset. So it doesn't while receiving the file. It might take a few tries to get the timing right.

- "GATT Error" while updating - Enter OTA mode: To enter OTA mode just connect a charging cable, press all 3 buttons on the right side of the watch and release top and bottom keeping middle button pressed few seconds more.

##How it works

If you really want to know how things work, check [this](https://hackaday.io/project/4510/log/16559-softdevicebootloader-installation-internals) post out.
