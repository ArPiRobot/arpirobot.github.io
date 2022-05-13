# Sensor Data & NetworkTable

Now that your robot can move around, it's time to add support for sensors. Sensors enable the robot to determine information about itself or the world around it. In this section, we will add support for measuring motor battery voltage, measuring distance to an object with an ultrasonic sensor, and measuring robot rotation using an IMU (gyroscope + accelerometer).

## Arduino Interface & Sensors

Many sensors do not work well with the Raspberry Pi directly. In some cases the Pi may lack support for certain functions (such as analog readings) or in other cases a Pi may not provide fine enough control over timing (a big issue for something like a gyroscope). Often, a microcontroller is better suited to working with sensors. Arduinos are microcontroller development boards. Being a development board, it contains extra circuitry needed to program the device and make it easy to interface with. This makes an Arduino an ideal device to use on the robot as a sensor *coprocessor*. The Arduino manages the sensors and collects the required data and sends it back to the Raspberry Pi where it can be used in the robot program. The Arduino communicates with the Pi using a UART (a Serial port) over USB. This allows the Arduino to be connected to the Pi and powered from the Pi by a USB cable.

### ArPiRobot Arduino Firmware

The ArPiRobot Arduino Firmware is a program written using the Arduino framework. This allows the firmware itself to support many different Arduino boards (and other microcontroller development boards too) using the same program. The source code for the firmware can be downloaded on the [downloads page](../../downloads/latest.md). To build the firmware and upload it to your board, you need to have the board and a USB cable to connect it to your computer. You also need to have the [Arduino IDE](https://www.arduino.cc/en/software) installed. Once installed, open the program.

Depending on your board you may also need to install a boards package to support your board in the Arduino IDE. Follow the instructions below for the board you are using. *Note: This may not include instructions for every supported board. Check the README on the [ArPiRobot Arduino Firmware GitHub](https://github.com/ArPiRobot/ArPiRobot-ArduinoFirmware) repo for more information about supported boards.*

??? "Arduino Nano & Clones"
    The Arduino Nano (and clones of the Arduino Nano) have support builtin to the Arduino IDE. Under the `Tools` menu change the board to "Arduino Nano" (under a category called `Arduino AVR Boards`). If you are using a clone (or an older nano) you probably need to change the `Processor` to `ATmega328P (Old Bootloader)`.

    ![](../../img/arduino_nano.png){: style="height:250px"}

??? "Arduino Nano Every"
    The Arduino Nano Every is officially supported, but the `megaAVR` boards package must be installed first. Under `Tools > Board` open `Board Manger...`. Search `megaAVR` and install the `Arduino megaAVR Boards` package (latest version). 
    
    ![](../../img/megaavr_install.png){: style="height:250px"}
    
    Then, close the boards manager and select `Arduino Nano Every` under `Tools > Board` (under the `Arduino megaAVR Boards` category).

    ![](../../img/arduino_nanoevery.png){: style="height:250px"}

??? "Raspberry Pi Pico"
    The Raspberry Pi Pico is supported by the ArPiRobot Arduino Firmware *using the [arduino-pico](https://github.com/earlephilhower/arduino-pico) core, **not** the official Mbed OS core*. Follow the [install instructions](https://arduino-pico.readthedocs.io/en/latest/install.html#installing-via-arduino-boards-manager) for `arduino-pico`. Then choose `Raspberry Pi Pico` under `Tools > Board` in the Arduino IDE. This will be under a category called `Raspberry Pi RP2040 Boards` **not** `Arduino Mbed OS RP2040 Boards`.

    ![](../../img/pi_pico.png){: style="height:250px"}

Once you've configured the correct board extract the source code you downloaded earlier to the `Arduino` folder that has been created in your `Documents` folder.

![](../../img/arduino_proj_location.png){: style="height:300px"}

Then in the Arduino IDE click `File > Open` and choose the extracted project. Open the `ArPiRobot-ArduinoFirmware.ino` file.

![](../../img/select_ino.png){: style="height:250px"}

After opening the project, plug in your Arduino using the USB cable. You should now have a port listed under `Tools > Ports`. Select it (the number / name will likely not be the same as in the screenshot below).

![](../../img/arduino_port_select.png){: style="height:200px"}

Finally, to build and upload the firmware to your board click the upload button (the arrow) along the top bar.

![](../../img/arduino_ide_upload.png){: style="height:300px"}


### Connecting Sensors

*Specific information about how each sensor must be connected and how connections can be made is located in the "Building a Robot" section of the guide.*

<!--TODO: Add a link above instead of just naming the section once that part of the guide is written-->

Now that the board is programmed, it is time to disconnect it from your computer. Connect it to the Pi on the robot by USB cable.

The ArPiRobot Arduino Firmware is fairly simple and does not need to be modified before use. The firmware is built with support for various sensors, but it does not know how sensors are connected to the Arduino. This information is provided to the Arduino from the Pi by your robot program. As such, it is necessary to know what sensors are connected and how they are connected when writing the code for your sensors. In this section of the guide three sensors are used: a voltage monitor, an ultrasonic sensor, and an IMU (Inertial Measurement Unit) containing both a gyroscope (measures rotation) and an accelerometer (measures acceleration). The code is written assuming the sensors are connected to the Arduino as follows

**Voltage Monitor (Voltage Divider Module)**

| Voltage Monitor Pin | Arduino Pin          | Use                         |
| ------------------- | -------------------- | --------------------------- |
| S                   | A0                   | Voltage reading (S = signal; A0 = analog input 0). Any analog input could be used instead of A0. | 

**Ultrasonic Sensor (HC-SR04)**

| Ultrasonic Sensor Pin | Arduino Pin            | Use                                |
| --------------------- | ---------------------- | ---------------------------------- |
| TRIG                  | 7                      | TRIG = trigger pin. Arduino writes a pulse to this pin to start a distance measurement. Any digital input / output pin can be used. |
| ECHO                  | 8                      | ECHO = echo pin. Arduino reads a pulse back in response to the triggered pulse to measure distance. Any digital input / output pin can be used. |

**IMU (Mpu6050)**

| IMU PIN           | Arduino Pin              | Use                              |
| ----------------- | ------------------------ | -------------------------------- |
| SCL               | SCL                      | I2C clock. SCL pin on Arduino may be shared with another pin depending on your board. Search for your board's pinout to determine which pin. |
| SDA               | SDA                      | I2C data. SDA pin on Arduino may be shared with another pin depending on your board. Search for your board's pinout to determine which pin. |


The above information is provided to make clear the assumptions that were used when writing the code in the next section. You can change connections as desired, just make sure the connection is valid for the sensor and change the code appropriately.

Likewise, if you are using a different supported IMU, you may need to change connections (tough at the time of writing this all supported IMUs are I2C devices and connected the same way) and you will need to change the code to use the correct device. 

For more information on each supported sensor see the [Arduino Interface and Sensors](http://localhost:8000/ref/arduino/) section of the programming reference.


## The Robot Code

### Creating Arduino Interface

An Arduino Interface is an object used in robot code to represent a board running the ArPiRobot Arduino Firmware connected to the Pi on the robot. Currently, only UART is supported as a connection method, therefore the `ArduinoUartInterface` class will be used.

In order to setup UART communication with the Arduino two pieces of information are required: the name of the serial / UART of the device and the baud rate for UART communication. The baud rate must be the same on both the Arduino and the Pi. The ArPiRobot ArduinoFirmware uses a baud rate of `57600` (unless you modify it) so use this on the Pi as well. Determining which UART port is a little more difficult. In Windows serial ports are named `COM` followed by some number. On linux systems they are `/dev/tty[TYPE][NUMBER]`. For most Arduinos type will be `USB` (although it is `ACM` for some). Generally, since only one USB serial device will be connected to the Pi, the number will be `0` making the device `/dev/ttyUSB0`. This name will be used when creating the Arduino interface object as shown in the code below.

=== "Python (`robot.py`)"
    ```py
    # Add with the other imports at the top of the file
    from arpirobot.arduino.iface import ArduinoUartInterface

    # Add in __init__ under the other devices
    # (below motors, drive helper, etc)
    self.arduino = ArduinoUartInterface("/dev/ttyUSB0", 57600)
    ```

=== "C++ (`robot.hpp`)"
    ```cpp
    // Add with the other includes at the top of the file
    #include <arpirobot/arduino/iface/ArduinoUartInterface.hpp>

    // Add below the other devices at the bottom of the class declaration
    // (below motors, drive helper, etc)
    ArduinoUartInterface arduino { "/dev/ttyUSB0", 57600 };
    ```

Then, in `robot_started` / `robotStarted` add the following so the Pi begins communicating with the Arduino when your robot program starts.

=== "Python (`robot.py`)"
    ```py
    # Add at the end of robot_started
    self.arduino.begin()
    ```

=== "C++ (`robot.cpp`)"
    ```cpp
    // Add at the end of robotStarted
    arduino.begin();
    ```

Build and deploy the code to your robot. Look for some lines similar to the following in the robot program log (in Deploy Tool). This line will appear after the robot started message is logged.

```
[DEBUG]: ArduinoUartInterface(/dev/ttyUSB0, 57600) - Opened interface.
[DEBUG]: ArduinoUartInterface(/dev/ttyUSB0, 57600) - Arduino is ready. Creating devices.
[DEBUG]: ArduinoUartInterface(/dev/ttyUSB0, 57600) - Done creating devices. Starting sensor processing.
[DEBUG]: ArduinoUartInterface(/dev/ttyUSB0, 57600) - Sensor processing started successfully.
```

If you instead see a message similar to the following, the uart port name is probably wrong. Try `/dev/ttyACM0`.

```
[ERROR]: ArduinoUartInterface(/dev/ttyUCB0, 57600) - Unable to open arduino interface.
```


### Creating the Sensors

TODO: Create objects for the sensors

TODO: Add these objects to the arduino (BEFORE BEGIN CALL)

TODO: Add vmon, usonic, and imu for this part of guide

TODO: Link to reference page with info about all sensor types


### Getting sensor values

TODO: Make main vmon

TODO: Log usonic distance

TODO: Log imu angle z

TODO: Explain issues with just using log for this


### Network Table

TODO: Show network table code

TODO: Explain net table indicators in DS


### Doing something Useful

TODO: Don't allow driving forward if object to close by usonic sensor

TODO: Maybe also use brake mode here
