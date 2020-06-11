# Setup the Raspberry Pi
ArPiRobot robots are made functional with a variety of components.

- A PC program to drive / control the robot (ArPiRobot Drive Station)
- A PC program to configure the robot (ArPiRobot Deploy Tool)
- An Operating System (OS) on the raspberry pi with custom modifications (Customized Raspbian Image or ArPiRobot Image).
- A python library to enable use of motors and other devices in robot code (ArPiRobot-PythonLib)
- A set of scripts and services to configure/control the Pi on the robot (ArPiRobot-RaspbianTools)

The RaspbianTools and PythonLib are packaged as an update zip for a specific ArPiRobot image and can be sent to and installed on the robot using the Deploy Tool. So the components you'll need to worry about are:

**On the Pi**

- An OS Image
- An Update (which includes the PythonLib and Raspbian Tools)

**On the PC**

- The Drive Station to control the robot
- The Deploy Tool to configure the robot and send code to the robot

First you need to setup the Raspberry Pi with the custom ArPiRobot image (which is just a modified version of Raspbian).

## Requirements
Before starting you will need a computer<sup>&ast;</sup>, a [supported Raspberry Pi](../starting/supportedhardware.md), a power supply for the Raspberry Pi (this can be a 2A or greater USB power adapter, a 2A or greater battery pack, etc), a micro SD card (at least 8GB), and a way to connect the micro SD card to your computer.

<sup>&ast;</sup>The software used in this section (balenaEtcher) is available for Windows, macOS, and Linux computers. If using another OS you will need to find another program to write the image file to the SD Card.

## Choosing an Image
It is always recommended to use the latest ArPiRobot image, a list of which is available on the [downloads page](../downloads/latest.md).


## Flash the Image
To start, download the newest Raspberry Pi image. This image contains a modified version of Raspbian, the official Raspberry Pi OS. It has tools, libraries, and programs pre-installed to allow it to work with ArPiRobot robot code. 

The Raspberry Pi loads the OS form a micro SD Card, so the image must be written to a micro SD Card. To do so, we will use [balenaEtcher](https://www.balena.io/etcher/). Download and run it. You should see a screen like the one below.

Choose select image and choose the ArPiRobot image file (`ArPiRobot-VERSION.img.gz`) that you downloaded. Then connect your micro SD card to your computer. Choose select target and choose the micro SD Card. Click flash and wait until it completes.

Put the SD Card in the Raspbery Pi and power it on. The activity light (green) should start blinking, indicating that the Pi is booting. Wait for it to finish booting (about 30-60 seconds). The first boot will take longer than most as it will expand the root partition to fill the SD card then reboot.

Once the Pi boots it will be generating a WiFi network called "ArPiRobot-Robot" with a password of "arpirobot123". This network is how we will interface with the Raspberry Pi later. When working with the robot, keep in mind that there will be no internet access via this network. If following these docs you will want to make sure you have the pages you need open before connecting to the robot's WiFi.
