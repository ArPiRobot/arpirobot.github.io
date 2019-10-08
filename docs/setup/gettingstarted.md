# Getting Started
ArPiRobot robots are made functional with a variety of components. The collection of these components is refered to as an ArPiRobot version. Certain versions of each component below must be used with each ArPiRobot version. 


## How to Start
If you want to understand what each component does see the [components page](../reference/components.md). Otherwise follow the basic guide below.

## Choosing a Version
It is always recommended to use the latest ArPiRobot version, a list of which is availbe on the [versions page](../versions.md). The versions page lists some basic information about each ArPiRobot version, including supported versions of each component. It also has Raspberry Pi Image downloads for each ArPiRobot version and a link to updates for that ArPiRobot version. 

## Requirements
Before starting you will need a computer with a WiFi adapter, a [supported Raspberry Pi](reference/hardware.md), a power supply for the Raspberry Pi (this can be a 2A or greater USB power adapter, a 2A or greater battery pack, etc), a micro SD card (at least 8GB), and a way to connect the micro SD card to your computer.

## First Steps
To start, download the newest Raspberry Pi image for the latest [ArPiRobot Version](versions.md). This image contains a modified versoin of Raspbian, the official Raspberry Pi OS. It has tools, libraries, and programs pre-installed to allow it to work with ArPiRobot robot code. 

After downloading the image (which will be a `.zip` file) it needs to be extracted from the compressed (zipped) folder before other programs can use it. The compressed folder (`.zip` file) will look similar to the image below in Windows explorer.


To extract the image right click it and select "Extract all..." and click "Extract" on the dialog that pops up. After extracting there will be a normal (uncompressed) folder. The one that does not end in `.zip` is the normal folder. Inside this folder there is the image file (`.img`).


The Raspberry Pi loads the OS form a micro SD Card, so the image must be written to a micro SD Card. To do so, we will use [balenaEtcher](https://www.balena.io/etcher/). Download and run it. You should see a screen like the one below.

Choose select image and choose the ArPiRobot image file (`.img`) from the uncompressed folder. Then connect your micro SD card to your computer. Choose select target and choose the micro SD Card. Click flash and wait until it completes.

Put the SD Card in the Raspbery Pi and power it on. The activity light (green) should start blinking, indicating that the Pi is booting. Wait for it to finish booting (about 30-60 seconds). 

Once the Pi boots it will be generating a WiFi network called "ArPiRobot-Robot" with a password of "arpirobot123". This network is how we will interface with the Raspberry Pi later.

## Download Tools
There are two main tools that run on your computer that you will use to interact with the Rasbperry Pi: the [ArPiRobot Drive Station](https://github.com/MB3hel/ArPiRobot-DriveStation/releases) and the [ArPiRobot Deploy Tool](https://github.com/MB3hel/ArPiRobot-DeployTool/releases). Download each and (if appropriate) install them.

## Applying an ArPiRobot Update to the Raspberry Pi
First, download the latest update for the ArPiRobot version you are using. The update will be a `.zip` file.

Next, connect your computer to the Raspberry Pi's WiFi network (and disconnect from other networks including wired networks). Run the deploy tool and click "Connect". If asked about SSH fingerprints, approve it and continue connecting. Once connected the Deploy Tool's other tabs will be available.

Select the updates tab and choose the downloaded `.zip` file. Click apply update and wait for it to finish. *Do not unplug the Raspberry Pi while it is applying an update.*

## Next Steps
The Raspberry Pi is now ready to run ArPiRobot robot code. Next your computer needs to be setup for ArPiRobot robot code development.
