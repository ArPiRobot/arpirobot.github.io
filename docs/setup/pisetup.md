# Setup Raspberry Pi

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

## Choosing a Pi
There are many different Raspberry Pi models available, however only some have WiFi builtin. The Raspberry Pi 3 series all have builtin WiFi modules and should work without issue, however the Pi 3A+ is often the perfered model due to its smaller size. The Pi Zero W (the "W" has WiFi the normal Pi Zero does not) can also be used. The Pi Zero offers a smaller size and less power consumption, but far less computing power. This makes it ideal for simple projects or projects where low-cost is more important than computing power.

Downloads for the latest ArPiRobot images are located [here](../downloads.md).

## Write the Image to a SD Card
- Download and install balenaEtcher
- Install [balenaEtcher](https://www.balena.io/etcher/)
- Insert the SD Card into an SD Card slot on your PC or connect it using a USB adapter.
- Run balenaEtcher
- Select the downloaded image
- Select the destination SD Card
- Click flash and wait for it to finish
- Once it finishes move the SD Card into the Pi, connect keyboard, mouse, monitor and power on the pi. 

## Next Steps
[Setup for Development](devsetup.md)
