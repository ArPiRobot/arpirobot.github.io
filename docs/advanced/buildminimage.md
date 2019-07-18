# Building a Minimal ArPiRobot Image
This guide details how the prebuilt minimal Raspbian image is created for ArPiRobot robots. This guide exists mostly to document the image creation process, but can be followed to create a custom Raspbian image or an image with another Linux OS. The minimal image configured should be used when development will not be done on the robot or when the robot may be powered off unexpectedly.

## Why a Custom Image?
There are few reasons to ever need a custom image. Most users do not need to spend the time creating their own image. The most common reasons for wanting to build a custom image are:

1. If you want to use or test a newer version of Raspbian that the prebuilt image uses.
2. If you want to use a different Linux OS (not Raspbian).
3. If you want to understand more about how all the components of this project fit together.

It is important to be aware that this guide is only up to date for the version of Raspbian that was used to make the prebuilt image. There will likely be slight differences if using another version of Raspbian or another Linux OS.

## Raspbian Version
This guide is currently up to data for Raspbian Buster Lite July 2019. The resulting image has been tested on a Raspberry Pi Zero W and a Raspberry Pi 3A+.

## Base Raspbian Install
- Download the Raspbian Buser Lite July 2019 Image from [here](https://www.raspberrypi.org/downloads/raspbian/). If the correct version is no longer available it is likely OK to use a newer version, but older images can be found [here](https://downloads.raspberrypi.org/raspbian/images/).
- Download and install [balenaEtcher](https://www.balena.io/etcher/)
- Install balenaEtcher
- Insert the SD Card into an SD Card slot on your PC or connect it using a USB adapter.
- Run balenaEtcher
- Select the downloaded raspbian image
- Select the destination SD Card
- Click flash and wait for it to finish

### Initial Raspbian Setup
It is recommended to just connect the pi to a monitor and connect a keyboard and mouse until the final Wireless configuration has been completed. It is possible to perform a headless install, however if something goes wrong with the network configuration later on it will be hard or impossible to recover from it without a keyboard, mouse, and monitor. If performing a headless install it would be a good idea to rely on ethernet for network access and to enable serial console access first thing.

Remove the SD Card from your PC and insert it in the Raspberry Pi. Connect the power cable and wait for the pi to boot. The first boot may take a minute or two.

To change the hostname and password for the pi user run the following command.

```
sudo raspi-config
```

To change the user password select "Change User Password" and enter "arpirobot" without the quotes when prompted.

To change the hostname select "Network Options" then choose "Hostname". Change the hostname to ArPiRobot-Robot when prompted.

After changing the hostname and password select Finish and choose "Yes" when asked to reboot.

## Upgrade software

To setup a temporary WiFi configuration (this is not necessary if using an ethernet cable to access the internet is an option) run `sudo raspi-config` and select "Network Options" then "Wi-fi". Select the region, enter an SSID and password for the network to connect to.

After connecting to a network (either ethernet or wifi) run the following command to upgrade packages
```
sudo apt update && sudo apt upgrade
```


## Remote Login (SSH)
To enable the SSH server for remote login access (if not already done by the file on the boot partition) run `sudo raspi-config` again and select "Interfacing Options" then select "SSH" and choose "Yes" to enable. Then, select "Finish" and reboot when prompted.


## Wireless Setup
WARNING: If setting the image up over a network (SSH) make sure not to reboot until all the following configuration is completed and verified!!!

#### Install Required Software

```
sudo apt install hostapd dnsmasq
```

#### Configuration
Create a virtual adapter to host the access point on boot. Create `/etc/udev/rules.d/70-persistent-net.rules` with the following contents. Replace the mac address (MACADDRESSHERE) with the Mac Address of the Raspberry Pi's WiFi adapter which can be found by running the command `iw dev` . Look for the addr field under the wlan0 interface section. The AP Mac address (APMACADDRESS) can usually be the same.

```
		SUBSYSTEM=="ieee80211", ACTION=="add|change", ATTR{macaddress}=="MACADDRESSHERE", KERNEL=="phy0", \
	      RUN+="/sbin/iw phy phy0 interface add ap0 type __ap", \
		  RUN+="/bin/ip link set ap0 address APMACADDRESS"
```

Now that all the networking configuration is done it is OK to complete the rest of the configuration over SSH and/or VNC instead of using a keyboard, mouse, and monitor.

## Readonly Filesystem

RAMDISK FOR LOGS???
MOVE PROGRAM LAUNCH LOG TO RAMDISK OR ONLY LOG IF TESTED WRITABLE???
DISABLE LOGGING
READONLY
DISABLE FS CHECKING AT BOOT

#### Make Filesystem Readonly and disable FS Checking on boot

## Installing Software

### Python and IDEL and and Arduino IDE and other utilities (text editor, chromium, archive manager)

### Python libraries (install with pip)
apscheduler, ansicolors, adafruit, arpirobot

### ArPiRobot Helper Scripts

### Arduino firmware

## Setting up directory structure for arpirobot programs

