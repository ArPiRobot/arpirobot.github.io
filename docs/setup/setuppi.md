# Setup the Raspberry Pi
ArPiRobot robots are made functional with a variety of components. The collection of these components is refered to as an ArPiRobot version. Certain versions of each component below must be used with each ArPiRobot version. 

## Requirements
Before starting you will need a computer with a WiFi adapter, a [supported Raspberry Pi](../reference/hardware.md), a power supply for the Raspberry Pi (this can be a 2A or greater USB power adapter, a 2A or greater battery pack, etc), a micro SD card (at least 8GB), and a way to connect the micro SD card to your computer.


## Choosing an Image
It is always recommended to use the latest ArPiRobot image, a list of which is availbe on the [downloads](../downloads.md).


## Flash the Image
To start, [download](../downloads.md) the newest Raspberry Pi image.. This image contains a modified versoin of Raspbian, the official Raspberry Pi OS. It has tools, libraries, and programs pre-installed to allow it to work with ArPiRobot robot code. 

After downloading the image (which will be a `.zip` file) it needs to be extracted from the compressed (zipped) folder before other programs can use it. The compressed folder (`.zip` file) will look similar to the image below in Windows explorer.


To extract the image right click it and select "Extract all..." and click "Extract" on the dialog that pops up. After extracting there will be a normal (uncompressed) folder. The one that does not end in `.zip` is the normal folder. Inside this folder there is the image file (`.img`).


The Raspberry Pi loads the OS form a micro SD Card, so the image must be written to a micro SD Card. To do so, we will use [balenaEtcher](https://www.balena.io/etcher/). Download and run it. You should see a screen like the one below.

Choose select image and choose the ArPiRobot image file (`.img`) from the uncompressed folder. Then connect your micro SD card to your computer. Choose select target and choose the micro SD Card. Click flash and wait until it completes.

Put the SD Card in the Raspbery Pi and power it on. The activity light (green) should start blinking, indicating that the Pi is booting. Wait for it to finish booting (about 30-60 seconds). 

Once the Pi boots it will be generating a WiFi network called "ArPiRobot-Robot" with a password of "arpirobot123". This network is how we will interface with the Raspberry Pi later.