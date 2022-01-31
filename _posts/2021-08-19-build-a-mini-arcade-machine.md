---
layout: post
title:  How to Set Up a Mini Arcade Machine or Similar Using a Raspberry Pi
author: Joshua Britain
---

This guide will explain how to set up your own mini or full size arcade machine using a Raspberry Pi. It does not contain any instructions for building the device itself, only the internals

## What you'll need
- ### A Raspberry Pi
I recommend the Raspberry Pi 4 (2GB is probably fine), but the Pi Zero will run older games alright. To find out where to buy a Raspberry Pi 4, visit [the official website](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/).

- ### A set of buttons and joystick
To build the controls for your arcade machine, you'll need some buttons and a joystick. The kit we used connected to a board that then connected to the Pi over USB, and can be bought [on Amazon](https://www.amazon.co.uk/dp/B075DFNK24) for just under £25.

- ### Some Wood
You probably want to build a box to house your controls, and possibly the screen depending on your design.

- ### A Monitor
This one is optional, if you are housing a screen within the arcade machine you will need one, otherwise it can be plugged into a TV like we have done. The Raspberry Pi outputs HDMI, so if you want to use a different display standard you will need an adapter.

- ### A Computer (with an SD card slot)
In order to install RetroPie on the Raspberry Pi you will need a computer to write to the SD Card.

## How to set it up

### 1. Setting up the controller
For our controller, we simply connected each button to the board it came with using the provided cables, and plugged it into the Pi. You will probably want at least six main face butttons (we used all 8), as well as start (in the suggested kit the one with the man icon on it) and select (coin icon). Our button layout is shown below.

![Our button layout](/assets/posts/arcade/buttons.jpg)

### 2. Installing RetroPie
RetroPie is a custom operating system for the Raspberry Pi with a variety of emulators built in. To install it, you must first [download it from the official site](https://retropie.org.uk/download/), picking the appropriate build for your device. In our case, we are using the Raspberry Pi 4 so we will select the Raspberry Pi 4/400 button. This should download a .gz file.

You will need to write the downloaded file to the SD card that came with the Raspberry Pi. For this you will need some sort of SD Card imagine software, and I recommend [Balena Etcher](https://www.balena.io/etcher/). Install the software and open it.

Insert the SD Card that came with the Pi into the computer, and in Balena Etcher select the downloaded RetroPie .gz file and the SD Card. Click flash, bearing in mind this will erase all data on the Raspberry Pi. This will install RetroPie onto the SD card, and may take a few minutes.

Once the flashing process is complete, eject the SD card from your computer and insert it back into the Pi. Turn it on and after it boots you should be prompted to configure input.
Our input config is shown above, note that to translate from the diagram to what it asks for


### 3. Setting up RetroPie

Right Face Button = B
Bottom Face Button = A
Left Face Button = X
Top Face Button = Y
Left Shoulder Button = LB
Right Shoulder Button = RB
Left Trigger = LT
Right Trigger = RT

We also mapped the joystick to the directional pad so that it would allow use to use it on older games.

### 4. Adding Games

In order to install games on your arcade machine, you will need to get the ROMs for them. Legally you should dump the games you own and only play them but of course I do not own classic arcade machines and so instead googled for example, "space invaders arcade ROM". In order to install the games, you have a few options:

- Connect your Pi to your network and access it by typing `\\RETROPIE` into the address bar [WINDOWS ONLY] [(Instructions for connecting to wifi)](https://retropie.org.uk/docs/Wifi/)
- Insert a USB drive into the Pi with the ROMs and transfer them. I haven't had an opportunity to try this but a Google search will probably tell you how.

Either way, you then place your ROM in the appropriate subfolder of 'roms', for example arcade games will go in the 'arcade' folder.

Reboot the Pi and you should be able to select the game from the menu.

## And that's it!

You have successfully set up your raspberry pi to emulate classic games!
