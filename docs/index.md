# ArPiRobot
Welcome to the ArPiRobot home page.

## About
ArPiRobot is a simple robotics framework built around a control system consisting primarily of a Raspberry Pi and an Arduino.

## Why was ArPiRobot Created? Why not use ROS or another existing framework?
It really boils down to three main goals for the project:

1. **Easy for education use**

    This means that it not only needs to be easy to code with, but easy to **use** without assuming lots of experience with computers. Most existing solutions require knowledge of SSH, various types or wiring, writing code to communicate with different devices, creating a from-scratch wireless control solution, setting up a development environment from scratch or writing code with a command line editor. This may work well for many experienced developers, but it does not often for new developers and peopel who want to just get started.

2. **Provide a complete setup from writing code to deploying it, to controlling the robot**

    ArPiRobot provides an easy to use development environment, preconfigured OS images, a GUI tool to deploy code that is written on a traditional PC, a GUI tool to control the robot wirelessly, scripts to manage everything from wifi to launching the program on boot, and a python library to interface with many devices. Everything has been designed to make it easy to use and just get started making a robot work instead of spending time on communicating with motor controllers, wifi, gamepads, bluetooth,  etc.

3. **Support low-power, low-cost devices**

    Not only does this make it more accessible (in line with the education use goal), but working on lower power devices just means that the framework is designed to be efficient, while having enough features to perform many tasks. There is also no reason the framework couldn't work on more powerful devices, it just means that more power is left for demanding tasks such as camera streaming, vision processing, etc. The core functionality of the framework is designed to work even on the single-core Raspberry Pi Zero W and more advanced features are all designed for and tested on the Raspberry Pi 3A+.

## License
All the source code for all components of ArPiRobot is currently not released, however will soon be released, likely under the GNU General Public License v3.0. Until then, no part of this work is to be used or distributed in any way, shape, or form except by the author (Marcus Behel).

## Where Does the Name Come From?
I got tired of typing `ArduinoAndPiRobot` so I decided to shorten it into `ArPiRobot`. Really creative right?

