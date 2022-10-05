
A "main computer" on the robot (a single board computer running a Linux based operating system) is not well suited to interface with many types of sensors. However, low cost Arduino and Arduino compatible microcontroller development boards are.

On ArPiRobot robots, an Arduino acts as a "coprocessor" that connects to and gets data from various sensors. It may also perform some calculations using the data. It then provides information from the sensors to the main computer of the robot.

This is done by using the ArPiRobot Arduino Firmware (see [downloads page](../downloads/latest.md)). The firmware supports several Arduino compatible development boards (see the repository on [GitHub](https://github.com/ArPiRobot/ArPiRobot-ArduinoFirmware#arpirobot-arduinofirmware) for a list).

## ArduinoInterface

An `ArduinoInterface` object is used to connect to an Arduino coprocessor from an ArPiRobot program. Currently, it is only possible to connect to an Arduino coprocessor via UART (typically over a USB cable). This uses the `ArduinoUartInterface` object.

### Creating a UART Interface Object

An `ArduinoUartInterface` object can be created as shown below. The device requires two pieces of information: a port name and a baud rate. The name of the port is in the form `/dev/tty[TYPE][NUM]`. `[TYPE]` is typically either `USB` or `ACM` depending on which arduino is used. `NUM` will typically be `0`, being the first UART device of that type. The baud rate is a "speed" that must be the same on both devices. Unless you modified the ArPiRobot Arduino Firmware running on the coprocessor, this will be `57600`.

=== "Python (`robot.py`)"
    ```py
    # Add with other imports
    from arpirobot.arduino.iface import ArduinoUartInterface

    # Add in __init__ of Robot class
    self.arduino = ArduinoUartInterface("/dev/ttyUSB0", 57600)
    ```

=== "C++ (`robot.hpp`)"
    ```cpp
    // Add with other includes
    #include <arpirobot/arduino/iface/ArduinoUartInterface.hpp>

    // Add with other member variables in Robot class
    ArduinoUartInterface arduino { "/dev/ttyUSB0", 57600 };
    ```

### Adding Devices to the Interface

Before the interface can be started, devices must be added to the interface. Various devices can be created and added to an interface (each is described in the "Arduino Devices" section below).

The ArPiRobot framework relies on the main computer to tell the Arduino coprocessor what is connected to it. This means the Arduino firmware will not need to be modified, only a robot program. However, it is necessary to tell an arduino what is connected to it (by adding devices) before "starting" the arduino. The process shown below can be repeated for multiple devices on the same arduino interface. In the example below `DeviceClass` is a placeholder and should be replaced with the name of an actual device (described in "Arduino Devices" section).

=== "Python (`robot.py`)"
    ```py
    # Add with imports
    from arpirobot.arduino.device import DeviceClass

    # Add in __init__
    self.device = DeviceClass(deice_arg1, device_arg2, ...)

    # Add in robot_started
    self.arduino.add_device(self.device)
    ```
=== "C++ (`robot.hpp`)"
    ```
    // Add with includes
    #include <arpirobot/arduino/device/DeviceClass.hpp>

    // Add as a member variable
    DeviceClass device { deviceArg1, deviceArg2, ... };
    ```
=== "C++ (`robot.cpp`)"
    ```cpp
    // Add in robotStarted
    arduino.addDevice(device);
    ```

### Starting the Interface

Once all devices have been added, the arduino interface must be "started". When "started" the Arduino coprocessor will begin acquiring sensor data and performing required calculations. Once started, the Arduino coprocessor no longer accept new devices (`add_device` / `addDevice` on the interface) will have no effect. As such, the following line should be added in `robot_started` / `robotStarted` **after** all devices are added.

=== "Python (`robot.py`)"
    ```py
    self.arduino.begin()
    ```
=== "C++ (`robot.cpp`)"
    ```cpp
    arduino.begin();
    ```

## Arduino Devices

TODO