# Technical Documentation

*Note: This may not always be up to date and is intended as a reference.*

## WiFi Configuration on the Pi

Currently, robots are configured to host a WiFi access point on the 2.4GHz band. 

The WiFi AP is run by `hostapd` using `dnsmasq` as its DHCP server. In order for this to work a static IP was set in dhcpd and wpa supplicant was disabled for the builtin WiFi adapter. If you need to change this you will have to modify the `dhcpcd` config file.

- dhcpcd config file : `/etc/dhcpcd.conf`
- hostapd config file: `/etc/hostapd/hostapd.conf`
- dnsmasq config file: `/etc/dnsmasq.conf`

There is currently no support to change this to a 5GHz network through the Deploy Tool, however if you are using a Pi that supports 5GHz WiFi networks you should be able to modify `/etc/hostapd/hostapd.conf` to generate a 5GHz AP.

Previously a dual AP and client mode setup was used, which allowed the robot to connect to a home WiFi network (and generally have internet access) in addition to generating its own network. This was useful for debugging, but not so useful for end users. Often, as the generated AP and client network had to be on the same channel, this made the Pi's AP unusable. It also caused some other problems with WiFi throughput. In addition, starting both on boot relied on a time-based workaround, so it was not the most reliable of things.


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