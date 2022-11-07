# Descriptions of Components

#### **ArPiRobot Development Board (Entire Project)**

[Github Project Board](https://github.com/orgs/ArPiRobot/projects/2/views/1)


#### **ArPiRobot OS Images**
Built using scripts available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-ImageScripts)

These are modified versions of Operating System Images for various supported single board computers with required software for ArPiRobot robots.

#### **ArPiRobot CoreLib**
Source code available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-CoreLib)

The Core (C++) library for ArPiRobot robot code and bindings for other languages.


#### **ArPiRobot Tools**
Source code available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-Tools)

Configuration and tools used on the Pi to run and configure robots and robot programs.


#### **ArPiRobot Drive Station**
Source code available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-DriveStation)

The PC-side control software for ArPiRobot robots. This interfaces with a running robot program (using the CoreLib's NetworkManager). This means that Drive Station compatibility mostly depends on the version of the CoreLib in use on the robot.


#### **ArPiRobot Mobile Drive Station**

Source code available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-MobileDriveStation)

Android version of the ArPiRobot Drive Station featuring a virtual on-screen controller.


#### **ArPiRobot Deploy Tool**
Source code available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-DeployTool)

The PC-side configuration software for ArPiRobot robots. The deploy tool mostly just provides a GUI to run ArPiRobot-Tools scripts via SSH. This means that Deploy Tool compatibility mostly depends on the version of the ArPiRobot-Tools in use on the robot.


#### **ArPiRobot Arduino Firmware**
Source code available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-ArduinoFirmware)

The program to run on an Arduino co-processor for ArPiRobot robots. The ArduinoFirmware and CoreLib must use a compatible set of devices/sensors and a compatible communication protocol. This means that changing the communication protocol between the Pi and the Arduino co-processor requires changes to the CoreLib. The same is true when adding support for a new device/sensor or changing what data comes from a device/sensor.

#### **ArPiRobot Visual Studio Code Extension**

Source code available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-VSCodeExtension)

Enables creating ArPiRobot projects in VSCode and installing ArPiRobot Updates on the development PC.

#### **ArPiRobot Toolchain**

Source code available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-Toolchain/)

Documentation of the process used to create cross compiler C/C++ toolchains for the Raspberry Pi. Repository also used to host toolchain downloads.

#### **ArPiRobot Camera Streaming**

Source code available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-CameraStreaming)

Scripts and services to support realtime low-latency camera streaming on ArPiRobot robots. Also includes scripts to setup the services used during the creation of the ArPiRobot images.
