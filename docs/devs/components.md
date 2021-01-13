# Descriptions of Components

#### **Raspberry Pi Image**
Downloads are available on the [downloads page](../downloads/latest.md)

Built using scripts available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-ImageScripts)

This is the modified version of Raspbian (the official Raspberry Pi OS) with required software for ArPiRobot robots. This contains an initial version of ArPiRobot Pythonlib and ArPiRobot Tools, but future updates may chagne which versions are used. While ArPiRobot software can be updated with the ArPiRobot (described below), it is sometimes necessary to make changes to the OS itself. If this becomes necessary new Raspberry Pi Images will be released.

#### **ArPiRobot CoreLib**
Source code available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-CoreLib)

The Core (C++) library for ArPiRobot robot code and bindings for other languages.


#### **ArPiRobot Tools**
Source code available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-Tools)

Configuration and tools used on the Pi to run and configure robots and robot programs.


#### **ArPiRobot Drive Station**
Source code available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-DriveStation)

The PC-side control software for ArPiRobot robots. This interfaces with a running robot program (using the PythonLib's NetworkManager). This means that Drive Station compatibility mostly depends on the version of the PythonLib in use on the robot.


#### **ArPiRobot Deploy Tool**
Source code available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-DeployTool)

The PC-side configuration software for ArPiRobot robots. The deploy tool mostly just provides a GUI to run ArPiRobot-Tools scripts via SSH. This means that Deploy Tool compatibility mostly depends on the version of the ArPiRobot-Tools in use on the robot.


#### **ArPiRobot Arduino Firmware**
Source code available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-ArduinoFirmware)

The program to run on an Arduino co-processor for ArPiRobot robots. The ArduinoFirmware and PythonLib must use a compatible set of devices/sensors and a compatible communication protocol. This means that changing the communication protocol between the Pi and the Arduino co-processor requires changes to the PythonLib. The same is true when adding support for a new device/sensor or changing what data comes from a device/sensor.

#### **ArPiRobot Visual Studio Code Extension**

Source code available on [GitHub](https://github.com/ArPiRobot/ArPiRobot-VSCodeExtension)

Enables creating ArPiRobot projects in VSCode and installing ArPiRobot Updates on the development PC.
