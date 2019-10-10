# Supported Hardware

#### **Supported Raspberry Pi's**
Any raspberry Pi with a WiFi adapter should work. While it is theoretically possible to use a USB WiFi adapter it would likely involve making changes to the WiFi configuration scripts and is not recommended.

| Raspberry Pi Model | Image Version Tested |
| ------------------ | -------------------- |
| Raspberry Pi Zero W | Beta1 |
| Raspberry Pi 3 Model A+ | Beta1 |


#### **Supported Arduino Coprocessors**
Any Arduino compatible board, that can be interfaced with the Raspberry Pi via UART (be it by pins or by USB) should work. Be aware that some Arduinos are 5V devices and some are 3.3V devices (if USB is used to supply power it will always be 5V so there will be 5V power available, but the arduino itself won't use 5V logic levels). Some sensors require 5V power, but can use a 3.3V logic level; and some sensors only support a 5V logic level. Some 3.3V devices have I/O pins that are 5V tolerant (such as the Teensy 3.2) so they can work with most 5V devices. A logic level shifter could also be used. 

There are also many Arduino clone boards available, and often times they are not obviously clones on listings online (such as on Amazon). Some clones (the ones that are obviously not Arduino brand products) are good quality, but others are not. Be careful where you buy things from.

The table below is just the boards that have been tested and are known to work.

| Arduino | Board Voltage | Description | Product Link (Can find other sellers) |
| --- | --- | --- | --- |
| Arduino Nano | 5V | Small footprint, same features as Arduino Uno | [Arduino Store](https://store.arduino.cc/usa/arduino-nano) |
| Arduino Nano Every | 5V | Same size and footprint as Arduino Nano, but newer and cheaper. For use on a breadboard make sure to get the variant with headers. | [Arduino Store](https://store.arduino.cc/usa/nano-every-with-headers) |
| Sainsmart Nano v3 | 5V | Sainsmart Arduino Nano clone. Cheaper, uses different FTDI chip, but usually works as intended. There were reports of devices with "counterfit" FTDI chips being bricked by a windows driver in 2014, but the driver no longer does this. There are also two versions of this board: a blue one and a black one. I linked the blue one here because the black ones seem to becoming harder to find. | [Sainsmart Store](https://www.sainsmart.com/products/nano-v3-atmega328-au-arduino-compatible), [Amazon](https://www.amazon.com/SainSmart-Nano-v3-0-Compatible-Arduino/dp/B00761NDHI/ref=sr_1_4?keywords=sainsmart+nano&qid=1570720152&sr=8-4) |
| Teensy 3.2 | 3.3V, I/O Pins 5V Tolerant | Powerful microcontroller with a small form factor. More powerful than many other Arduino compatible boards with a similar form-factor. Header pins must be purchased separately and soldered on. | [PJRC Store](https://www.pjrc.com/store/teensy32.html) |

#### **Supported Motor Drivers**
Currrently motor drivers are all connected to the Raspberry Pi. The arduino is not currently used as a motor driver.

| Motor Driver | Connection Type | Number of Motors | Logic Voltage | Motor Voltage | Max Current per Motor | Notes | Link |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Adafruit DRV8833 Module | GPIO/PWM with jumper wires | 2 | 2.7V min, motor voltage max | 2.7V - 10.8V | 1.2A |  | [Adafruit Store](https://www.adafruit.com/product/3297) |
| Adafruit TB6612 Module | GPIO/PWM with jumper wires | 2 | 2.7V-5V | 4.5V - 13.5V | 1.2A |Does not come with terminal block power input header. | [Adafruit Store](https://www.adafruit.com/product/2448) |
| L298N Module | GPIO/PWM with jumper wires | 2 | 2.7-7V | 5V-12V | 2A | Motor voltage can be up to 35V, but onboard 5V regulator cannot be used (remove jumper). An external 5V supply will be required. There is a ~1V drop between motor battery voltage and voltage applied to motors with this module. | [Amazon](https://www.amazon.com/Qunqi-2Packs-Controller-Stepper-Arduino/dp/B01M29YK5U/ref=sr_1_7?keywords=l298n&qid=1570723739&sr=8-7) |
| Adafruit Motor Hat | Pi GPIO Header (hat) I<sup>2</sup>C | 4 | 3.3V I<sup>2</sup>C | 4.5V-13.5V | 1.2A | Uses TB6612 chips, but communicates with Pi via I<sup>2</sup>C, so there is no need for software PWM. It comes with a standard header, but it is recommended to buy and use Adafruit's stacking header so other I/O pins are accessible. | [Adafruit Store](https://www.adafruit.com/product/2348) |
| Geekworm Motor Hat | Pi GPIO Header (hat) I<sup>2</sup>C | 4 | 3.3V I<sup>2</sup>C | 4.5V-13.5V | 1.2A | Claims to use TB6612, but mine did not. Used different driver with current limiting issues that lead to hat thermal shutoff happening easily for larger robots. Seems to be lower quality "copy" of Adafruit's Hat. | [Amazon](https://www.amazon.com/Raspberry-Function-Expansion-Support-Stepper/dp/B0721MTJ3P/ref=sr_1_6?keywords=raspberry+pi+motor+hat&qid=1570724085&sr=8-6) | 


#### **Supported Raspberry Pi Sensors**
Most sensors require an arduino either due to timing, sensor communication requirements, or voltage requirements. Some however, work well with a Raspberry Pi and are easier to use without involving the Arduino.

| Sensor | Description | Link |
| --- | --- | --- |
| Adafruit INA260 Voltage, Current Sensor | Measures motor battery voltage and current consumption of motors. Communicates with Pi via I<sup>2</sup>C. Provides more acurate voltage readings than a voltage divider does with the arduino and provides current readings. | [Adafruit Store](https://www.adafruit.com/product/4226) |


#### **Supported Arduino Sensors**
| Sensor Type | Sensor Description | Link(s) |
| --- | --- | --- |
| Rotary Encoders | Any encoder with a single data line (single channel encoders). Can be used to determine distance traveled or speed, but cannot distinguish between directions. Commonly made with slotted disks and photo interrupters. | [TT Motor Disk](https://www.adafruit.com/product/3782), [Photo Interrupter](https://www.adafruit.com/product/3986) |
| Old Adafruit 9DOF IMU | No longer sold. Legacy component. Gyro, accelerometer, and magnetometer combo board. | [Adafruit Store](https://www.adafruit.com/product/1714) |
| Adafruit NXP 9DOF IMU | Support coming soon. Newer gyro, accelerometer, and magnetometer combo board. | [Adafruit Store](https://www.adafruit.com/product/3463) |
| 4-Pin Ultrasonic Sensors | Ultrasonic sensors with trigger and echo pins. Most common is HC-SR04, which is 5V only. There are other compatible devices that work at 3.3V though. | [Standard HC-SR04](https://www.adafruit.com/product/3942), [5V/3V Compatible Device](https://www.adafruit.com/product/4007) |
| Analog Input Voltage Monitor | Using an analog input to measure motor battery voltage, usually through a votage divider. Can assemble on breadboard with through hole resistors or buy pre-assembled voltage divider. | [Voltage Divider Module](https://www.amazon.com/Diymall-Voltage-Sensor-Dc0-25v-Arduino/dp/B00NK4L97Q/ref=pd_cp_328_1/138-3529356-0891926?_encoding=UTF8&pd_rd_i=B00NK4L97Q&pd_rd_r=3f261697-5124-4ab3-80e7-11f26fb46372&pd_rd_w=ai8HI&pd_rd_wg=mZRsC&pf_rd_p=0e5324e1-c848-4872-bbd5-5be6baedf80e&pf_rd_r=FWXE8A9E4VHS9Q24H9MN&psc=1&refRID=FWXE8A9E4VHS9Q24H9MN) |
