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


#### **Supported Raspberry Pi Sensors**


#### **Supported Arduino Sensors**
