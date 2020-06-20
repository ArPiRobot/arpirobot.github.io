# Descriptions of Components

#### **Raspberry Pi Image**
Downloads are available on the [downloads page](../downloads/latest.md)

Built using scripts available on [GitHub](https://github.com/MB3hel/ArPiRobot-ImageScripts)

This is the modified version of Raspbian (the official Raspberry Pi OS) with required software for ArPiRobot robots. This contains an initial version of ArPiRobot Pythonlib and ArPiRobot Raspbian Tools, but future updates may chagne which versions are used. While ArPiRobot software can be updated with the ArPiRobot (described below), it is sometimes necessary to make changes to the OS itself. If this becomes necessary new Raspberry Pi Images will be released.

#### **ArPiRobot PythonLib**
Source code available on [GitHub](https://github.com/MB3hel/ArPiRobot-PythonLib)

The Python library for ArPiRobot robot code.

#### **ArPiRobot JavaLib**
Source code available on [GitHub](https://github.com/MB3hel/ArPiRobot-JavaLib)

The Java library for ArPiRobot robot code.

#### **ArPiRobot RaspbianTools**
Source code available on [GitHub](https://github.com/MB3hel/ArPiRobot-RaspbianTools)

Configuration and tools used on the Pi to run and configure robots and robot programs.

These are not in any way associated with the [Raspbian Project](https://www.raspbian.org/). The name comes from the fact that all the scripts and services were designed to work on the Raspberry Pi OS (formerly called Raspbian).


#### **ArPiRobot Drive Station**
Source code available on [GitHub](https://github.com/MB3hel/ArPiRobot-DriveStation)

The PC-side control software for ArPiRobot robots. This interfaces with a running robot program (using the PythonLib's NetworkManager). This means that Drive Station compatibility mostly depends on the version of the PythonLib in use on the robot.


#### **ArPiRobot Deploy Tool**
Source code available on [GitHub](https://github.com/MB3hel/ArPiRobot-DeployTool)

The PC-side configuration software for ArPiRobot robots. The deploy tool mostly just provides a GUI to run RaspbianTools scripts via SSH. This means that Deploy Tool compatibility mostly depends on the version of the RaspbianTools in use on the robot.


#### **ArPiRobot Arduino Firmware**
Source code available on [GitHub](https://github.com/MB3hel/ArPiRobot-ArduinoFirmware)

The program to run on an Arduino co-processor for ArPiRobot robots. The ArduinoFirmware and PythonLib must use a compatible set of devices/sensors and a compatible communication protocol. This means that changing the communication protocol between the Pi and the Arduino co-processor requires changes to the PythonLib. The same is true when adding support for a new device/sensor or changing what data comes from a device/sensor.

#### **ArPiRobot Updates**
The [Update Packager](https://github.com/MB3hel/ArPiRobot-UpdatePackager) repository contains the install script used with updates. The contents of the updates themselves are the Python Library and the Raspbian tools.

An update to ArPiRobot Raspbian Tools, ArPiRobot Pythonlib, and python libraries (dependencies for ArPiRobot-PythonLib) for the Raspberry Pi. When applying an update using the Deploy tool the update zip is copied to the robot the extracted. After extracting the update it's install.sh script is run. If the script exits with a non-zero exit code the deploy tool will read an install log file (`/home/pi/arpirobot-update.log`) and display it to the user.

The same update zip can be installed on the development computer using the VSCode Extension. When installing an update on the PC only the PythonLib is installed (along with dependencies, which are downloaded using pip).

#### **ArPiRobot Visual Studio Code Extension**

Source code available on [GitHub](https://github.com/MB3hel/ArPiRobot-VSCodeExtension)

Enables creating ArPiRobot projects in VSCode and installing ArPiRobot Updates on the development PC.
