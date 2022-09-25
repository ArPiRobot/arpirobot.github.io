# Technical Documentation

*Note: This may not always be up to date and is intended as a reference.*

*Sections containing only bulleted lists have not yet been written and exist only as an outline.*

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

## CoreLib General Structure
- Robot classes
- Scheduler
- NetworkManager
- NetworkTable
- ControllerData
- Action API
- Arduinos
- Logging (and how it integrates with networking)
    - Also prints to stdout so ArPiRobot-Tools arpirobot-program service can log to file
- Use of references for statically allocated objects (user must keep in scope) and shared_ptrs for dynamically allocated that the user does not need to manage the lifecycle of. Also document why some places such as BaseDevice::action and ArduinoDevice::arduino use raw pointers (these should not keep what they point to alive and the object that sets these vars clears these vars on its deletion).
- Document bridge lifecycle management
    - Bridge creates objects using make_shared and keeps a map of raw ptr to shared_ptr
    - Bridge returns raw ptr. When passed a raw ptr it looks up the shared_ptr and passes that to c++ functions. The c++ funcs may keep a copy of the shared_ptr to keep objects in scope even if the bridge's calling language (eg python) has its object leave scope and call destroy. Destroy just removes the shared_ptr copy from the bridge's map. If a copy is held elsewhere the object is not deallocated. If it is not held elsewhere, the object is deallocated immediately. BaseRobot does not use this method as the robot is started using its own blocking start method. No need to handle scenarios where a reference is not kept by user code for the duration of the program. Additionally, the bridge calling language must make sure objects with callbacks (eg Actions) are not cleaned up in the calling language (eg python) while C++ may call functions that are members (pointers to member functions). To this end, Actions are always kept in scope once instantiated by the python bindings.

## CoreLib Devices

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
- QuadEncoder
- Ultrasonic4Pin
- OldAda9DofImu
- NxpAda9DofImu
- Mpu6050Imu
- VoltageMonitor
- IRReflectorModule

## CoreLib Arduino Interface and Communication Protocol
- Document general communication protocol, start byte, escape byte, stop byte
- CRC16, intent behind it, what happens if the Pi gets invalid data
- Default settings for the firmware

## ArduinoFirmware Devices

- SingeEncoder
- QuadEncoder
- Ultrasonic4Pin
- OldAda9DofImu
- NxpAda9DofImu
- Mpu6050Imu
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
- Gamepads (SDL, why no joystick support, etc)
- Network table sync

## Deploy Tool Routines / Scripts
- When and how the deploy tool uses the scripts
- Document procedure for actions not using scripts

## CoreLib Updates
- On the robot
- On the development PC
- Creating updates 
    - Using generate_package.py

## VSCode Extension Functionality
- Create Projects (using template hard-coded in extension)
