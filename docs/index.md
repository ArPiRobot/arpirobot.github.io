# ArPiRobot Framework

TODO: Image

The ArPiRobot Framework is a collection of programs, libraries, and tools to build robots using a Single Board Computer (eg a Raspberry Pi) and a microcontroller development board (eg an Arduino). The framework includes several tools to simplify the development process and eliminate the need for familiarity with configuring and interfacing with Single Board Computers. The goal is to make building a robot easy regardless of technical background.


## Framework Goals

1. **Easy for education use**

    This means that it not only needs to be easy to code with, but easy to **use** without assuming lots of experience with computers. Most existing solutions require knowledge of SSH, various types or wiring, writing code to communicate with different devices, creating a from-scratch wireless control solution, setting up a development environment from scratch or writing code with a command line editor. This may work well for many experienced developers, but it does not often for new developers and people who want to just get started quickly.

2. **Provide a complete setup from writing code to deploying it, to controlling the robot**

    The ArPiRobot project provides an easy to use development environment, pre-configured OS images for the robot, a GUI tool to deploy code from your PC to the robot, a GUI tool to control the robot wirelessly, scripts to manage everything from WiFi to launching the program on boot, and a program libraries to interface with many devices. Everything has been designed to make it easy to use and just get started making a robot work instead of spending time on communicating with motor controllers, WiFi, gamepads, bluetooth,  etc.

3. **Support low-power, low-cost devices**

    Not only does this make it more accessible (in line with the education use goal), but there's no reason you couldn't use it on a more powerful device. It just means that more power is left for demanding tasks such as camera streaming, vision processing, etc. The core functionality of the framework is designed to work even on the single-core Raspberry Pi Zero W and more advanced features are all designed for and tested on the Raspberry Pi 3A+.


## Overview

The ArPiRobot Framework consists of several components (which can be obtained on the [downloads](./downloads/latest.md)) page. Some of the key components are as follows

- ArPiRobot CoreLib: A C++ and Python Library for writing robot programs. Includes support for various devices (sensors, motor controllers, microcontroller interfaces, etc) as well as a general structure for robot programs.
- ArPiRobot Drive Station: A program to run on your computer (laptop / desktop) used to control a robot.
- ArPiRobot Deploy Tool: A program to run on your computer (laptop / desktop) used to configure a robot and upload a robot program to the robot.
- ArPiRobot-VSCodeExtension: An extension to make creating ArPiRobot programs easy using [Visual Studio Code](https://code.visualstudio.com/)
- ArPiRobot OS Images: Pre-configured Operating System Images for supported Single Board Computers. These can be flashed to an SD card and used immediately.


## License

All components of the ArPiRobot Framework are open source and most are released under the [GNU Lesser General Public License v3.0](https://www.gnu.org/licenses/lgpl-3.0.html).


## Where Does the Name Come From?

I got tired of typing `ArduinoAndPiRobot` so I decided to shorten it into `ArPiRobot`. Really creative right?
