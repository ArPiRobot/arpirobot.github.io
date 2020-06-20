# Technical Documentation

*Note: This may not always be up to date and is intended as a reference.*

## WiFi Configuration on the Pi
- Explain hostapd/dnsmasq/dhcpcd setup

## ArPiRobot-Tools Scripts and Services
- What each script is for and what it does.
- Same for each service

## PythonLib General Structure
- Robot classes
- Scheduler
- Asyncio
- NetworkManager
- ControllerData
- Action API
- Event API
- NetworkTable
- Arduinos
- Logging (and how it integrates with networking)
    - Also prints to stdout so ArPiRobot-Tools arpirobot-program service can log to file

## PythonLib Dependencies
- Why/how each library is used

## PythonLib Devices

**Devices**

Things controlled entirely by the Pi.

- Gamepad
- INA260PowerSensor
- AdafruitMotorHatMotor
- DRV8833Motor and DRV8833Module
- TB6612Motor and TB6612Module
- L298NMotor and L298NModule

**ArduinoDevices**

Things controlled by the arduino that provide data to the Pi.

- SingeEncoder
- Ultrasonic4Pin
- OldAda9DofImu
- NxpAda9DofImu
- VoltageMonitor
- IRReflectorModule

## PythonLib Arduino Interface and Communication Protocol
- Document general communication protocol, start byte, escape byte, stop byte
- CRC16, intent behind it, what happens if the Pi gets invalid data
- Default settings for the firmware

## ArduinoFirmware Devices

- SingeEncoder
- Ultrasonic4Pin
- OldAda9DofImu
- NxpAda9DofImu
- VoltageMonitor
- IRReflectorModule

## Adding Custom ArduinoFirmware Devices
- Why
    - Use features such as interrupts that can't be done or are hard to do dynamically
    - Add support for new sensors
- Using static devices
- Adding a new dynamic device

- Explain decision not to use arduino to handle motors

## Connecting to the Robot
- What order connections must be made in
- Requirements (same source address)
    - How the robot handles this if these requirements are not met

## Drive Station Network Protocol
- 4 Ports
- 3 TCP
    - Command Port (enable, disable, sync net table)
    - NetTable Data Port (send/receive key/value pairs)
    - Log Port (send log messages from robot to DS)
- Network table sync

## Drive Station Native Library for Gamepads
- How it works
- Gamepad mappings file
- Required libraries
- JNI bindings
- Building
- Using on non-supported OSes
    - Where does it look for native library
    - And in what order

## Deploy Tool Routines / Scripts
- When and how the deploy tool uses the scripts
- Document procedure for actions not using scripts

## How Updates Work
- On the robot
- On the development PC
- Creating updates 
    - Using generate_package.sh
    - Manually
    - Custom updates (to change things, install deb packages, etc)

## VSCode Extension Functionality
- Create Projects (using template hard-coded in extension)
- Install updates on development PC