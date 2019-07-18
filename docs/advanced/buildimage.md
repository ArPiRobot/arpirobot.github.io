# Building a Raspberry Pi Image
This guide details how the prebuilt Raspbian image is created for ArPiRobot robots. This guide can be followed to create a custom Raspbian image or an image with another Linux OS.

## Why a Custom Image?
There are few reasons to ever need a custom image. Most users do not need to spend the time creating their own image. The most common reasons for wanting to build a custom image are:

1. If you want to use or test a newer version of Raspbian that the prebuilt image uses.
2. If you want to use a different Linux OS (not Raspbian).
3. If you want to understand more about how all the components of this project fit together.

It is important to be aware that this guide is only up to date for the version of Raspbian that was used to make the prebuilt image. There will likely be slight differences if using another version of Raspbian or another Linux OS.

## Raspbian Version
This guide is currently up to data for Raspbian Buster. The resulting image has been tested on a Raspberry Pi Zero W and a Raspberry Pi 3A+.

## Building the image
### Base Raspbian Install
The Raspbian Minimal Image is used. Download the image and use [balenaEtcher](https://www.balena.io/etcher/) to write the image to the SD card. 
- After downloading the image connect the SD card to the computer (using an adapter if necessary)
- Install and run balenaEtcher
- Select the downloaded image, select the SD card as the destination device, the click flash

### Initial Raspbian Setup
The following setup is completed the first time booting with the SD card in the pi. A mouse, keyboard, and monitor will need to be connected to the Pi for this portion of the setup process.

#### User configuration
The default pi user is used. Set the password for the user to "arpirobot"
Make sudo can be used without a password (required for deploy tool to work)

#### SSH Server

#### VNC Server

### Configuring the Image

#### Wireless Setup

#### Disable Logging to SD Card

#### Make Filesystem Readonly and disable FS Checking on boot

### Installing Software

#### ArPiRobot Helper Scripts

#### Python and required libraries

#### Setting up directory structure for arpirobot programs

