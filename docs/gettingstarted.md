# Getting Started

## Requirements
- Raspberry Pi with builtin WiFi (Zero W and 3A+ tested)
    - *It can be done with a USB WiFi dongle, however this is not tested, nor supported and modifications will need to be made to the image in order to do so. It is not an easy task.*
- SD Card
    - Recommended 32GB or larger. No smaller than 8GB.
- Adapters for the Pi:
    - Mostly for the Pi Zero. Zero will need mini HDMI to HDMI adapter for monitors and micro USB OTG cable to connect keyboard/mouse.
    - HDMI to VGA adapter may be necessary for either. A USB hub may be necessary for setup, unless a single receiver for a keyboard and mouse can be used.
- A PC capable of running Java Programs (Windows, macOS, or Linux) with a WiFi adapter
    - A USB WiFi adapter is OK for this
- A USB gamepad to control the robot.

## Choosing a Pi and Image
There are many different Raspberry Pi models available, however only some have WiFi builtin. The Raspberry Pi 3 series all have builtin WiFi modules and should work without issue, however the Pi 3A+ is often the perfered model due to its smaller size. The Pi Zero W (the "W" has WiFi the normal Pi Zero does not) can also be used. The Pi Zero offers a smaller size and less power consumption, but far less computing power. This makes it ideal for simple projects or projects where low-cost is more important than computing power.

After choosing a Pi it is necessary to choose an image. The ArPiRobot images are modified Raspbian installs and are easily written to an SD Card. There are two types of images provided: the minimal image and the complete image. Often using the complete image is better, but see the [image comparison page](imagedif.md) for more information on the pros and cons of each image.

Downloads for the latest version of each image are located [here](imagedownloads.md).

## Write the Image to a SD Card
- Download and install balenaEtcher
- Install [balenaEtcher](https://www.balena.io/etcher/)
- Insert the SD Card into an SD Card slot on your PC or connect it using a USB adapter.
- Run balenaEtcher
- Select the downloaded image
- Select the destination SD Card
- Click flash and wait for it to finish
- Once it finishes move the SD Card into the Pi, connect keyboard, mouse, monitor and power on the pi.

## Installing Required Software
In order to control the robot the ArPiRobot Drive Station must be installed on another PC. The drive station is written in Java, so a Java runtime must also be installed. 

### Java

#### Windows and macOS

Since Oracle changed how java was licenesed OpenJDK is the best way to use Java for non-commercial use. This change, however, made Java more difficult to install as official OpenJDK builds do not provide installers. There are, however, community builds available from several sources that do provide installers. One such provider is AdoptOpenJDK.

- Click on the link above which will take you to the AdoptOpenJDK download page.
- Select "OpenJDK 11 (LTS)" and "Hotspot" as the VM.
- Click download latest release.
- Run the downloaded installer and follow the instructions to install Java


#### Linux and BSD Users

OpenJDK packages are likely provided in your distro's package repositories. Just google it.

For example, with Ubuntu the following command would install Java

```
sudo apt install openjdk-11-jdk
```

### ArPiRobot Drive Station and Deploy Tool
Download the drive station and the deploy tool from the links on the [other downloads page](otherdownloads.md).

Double click a `.jar` file to run it. There is one for the drive station and one for the deploy tool. 

## Next Steps
If using the minimal image see [this guide](usingminimage.md).

If using the complete image see [this guide](usingcompleteimage.md).