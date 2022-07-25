
A device is an object in code that represents any component on the robot that is directly controlled or used by the robot program. This includes motor controllers, gamepads, sensors, coprocessors, and generic digital input / output devices.

## BaseDevice

=== "Python"
    ```py
    from arpirobot.core.device import BaseDevice
    ```
=== "C++"
    ```cpp
    #include <arpirobot/core/device/BaseDevice.hpp>
    ```

`BaseDevice` is the base class upon which all devices are based. Most functions in this device are not intended to be called on device instances. Rather, they are used when implementing a new device object.

When implementing a device the following functions must be implemented (note that devices cannot be implemented via python bindings, and must be implemented in C++).

=== "C++"
    ```cpp
    void begin(){
        // Run when the device is ready to be started
        // This will occur just before robot_started / robotStart is run, 
        // but after calling start() on the BaseRobot child class instance
        // This is where communication with the device should be started
        // Note that a custom destructor should be used to "stop" a device and close communication with the device
    }

    bool isEnabled(){
        // Must return true if the device is enabled
        // Each device must track this itself.
        // Typically a variable set in enable() / disable() is sufficient
    }

    bool shouldMatchRobotState(){
        // Return true if this device should be disabled when the robot becomes disabled
        // If this device is "potentially dangerous" it should not be allowed to run when the robot is disabled
        // Thus return true for "potentially dangerous" devices
    }

    bool shouldDisableWithWatchdog(){
        // Return true if this device should be disabled by the watchdog
        // If this device is "potentially dangerous" it should not be disabled by the watchdog
        // Thus return true for "potentially dangerous" devices
    }

    void enable(){
        // Do anything necessary to enable the device
    }

    void disable(){
        // Do anything necessary to disable the device
        // This should return the device to a "safe" state
    }

    std::string getDeviceName(){
        // Should return the name of this device
        // This should closely match the constructor of the class
        // For example, if the device is for Sensor A (with a class name SensorA)
        // This would return "SensorA()"
        // If the class takes arguments (pin numbers, addresses, etc) these should be included in the name
        // For example, if the sensor has one pin
        // return "SensorA(" + std::to_string(pin) + ")";
    }
    ```


## MotorController

=== "Python"
    ```py
    from arpirobot.core.device import MotorController
    ```
=== "C++"
    ```cpp
    #include <arpirobot/core/device/MotorController.hpp>
    ```

`MotorController` is a base class for motor controller devices. It cannot be instantiated directly, but serves as a base class for all motor controllers. This ensures that all motor controllers have the same function set available (making them easily interchangeable in code). It also abstracts some of the underlying functions of `BaseDevice` to make implementing a motor controller object easier.

The following function set must be implemented for any motor controller

=== "C++"
    ```cpp
    void begin(){
        // Just like BaseDevice::begin
        // Setup communication with the device so it is ready to use
    }

    void run(){
        // This function is run when the motor's speed should be changed
        // This needs to use the internal "speed" variable and 
        // "brakeMode" variable to determine what the motor should be doing
    }

    void close(){
        // Stop the motor (zero speed) and close communication
        // It is important to stop the device here as the code
        // will loose control of it afterwards (most motor controller continue latest motion when communication closed)
    }
    ```



## Device Objects

If you are using supported components there will be no need to implement custom devices. The CoreLib provides code for supported devices in the following locations (where "DEVICE" is replaced with a specific device package and name).

=== "Python"
    ```py
    from arpirobot.devices.DEVICE import DEVICE
    ```
=== "C++"
    ```cpp
    #include <arpirobot/devices/DEVICE/DEVICE.hpp>
    ```

See [Python source](https://github.com/ArPiRobot/ArPiRobot-CoreLib/tree/master/python_bindings/arpirobot/devices) or [C++ source](https://github.com/ArPiRobot/ArPiRobot-CoreLib/tree/master/cpp_library/include/arpirobot/devices) for a full list of devices.


## Using Devices

Devices should generally be public members of the `BaseRobot` child class that will be instantiated. This allows other objects, such as actions, to access the devices too. Keeping them member variables preserves a logical organization where devices are parts of the robot.

While devices should be instantiated in the constructor&ast; of the `Robot` class (`BaseRobot` child) they should **not** be configured until `robot_started` / `robotStarted`.


&ast;Or using initializer lists / inline initialization in C++.
