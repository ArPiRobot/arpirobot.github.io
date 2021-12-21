# Supported Hardware

#### **What Does Supported Mean**
In this context supported can mean a few different things. In the case of motor drivers and sensors it means that they have been tested and that there is code to work with them.

In the case of Arduinos supported means that the firmware has been tested running on a supported Arduino. However, it will likely work on any Arduino. 

In the case of Raspberry Pi's it means that the modified Raspbian images have been tested on that model of Pi. Likely any Pi (with builtin WiFi adapter) will work.

In the case of other devices such as battery packs it simply means that it should be (by specs) compatible and *may* have been tested.

Other tools and supplies listed may or may not have been tested, but should be compatible with whatever purpose is described for them.

#### **About Vendors**
Most of the time we provide links to components that can be bought from [Adafruit](https://www.adafruit.com/) or are easily found on [Amazon](https://www.amazon.com/). Occasionally, parts may not be available from these sites (or may be hard to find or not competitivly priced) so there are sometimes other sites used.

It is also worth noting that [Digikey](https://www.digikey.com/) stocks most Adafruit parts and allows backordering.

[Banggood](https://www.banggood.com/) - Stocks many items to be purchased online. Similar to Amazon, but slow shipping from China. If you are after cheap for many of the items on the "Other" list, look here.

[DFRobot](https://www.dfrobot.com/) - Provide good kits and selection of components, but shipping costs are high. 

[Robotshop](https://www.robotshop.com/) - Stock many components, including many DFRobot motors with cheaper shipping.

[Arduino Store](https://store.arduino.cc/usa/) - This is sometimes the only place to easily find some boards.

#### **Supported Raspberry Pi's**
Any raspberry Pi with a WiFi adapter should work. While it is theoretically possible to use a USB WiFi adapter it would likely involve making changes to the WiFi configuration scripts and is not recommended.

| Raspberry Pi Model      | Link |
| ----------------------- | ---- |
| Raspberry Pi Zero W     | [Adafruit](https://www.adafruit.com/product/3708) |
| Raspberry Pi 3 Model A+ | [Adafruit](https://www.adafruit.com/product/4027) |
| Raspberry Pi 3 Model B+ | [Adafruit](https://www.adafruit.com/product/3775) |

For projects that will involve many sensors, multiple concurrent actions, or many calculations you should probably avoid the Pi Zero W. The Pi Zero W is a single core device and will not handle concurrent tasks very well.


#### **Supported USB Battery Packs (for Raspberry Pi Power)**

Note: you can also use a 5V regulator to power the Pi from the motor batteries (AA's or rechargeable battery pack), but rapidly changing motor speed/direction can result in voltage drops rebooting the Pi so it is recommended to use a separate power source (USB battery pack).

| USB Battery Pack | Output Current | Works For Pi 3A/3B | Works For Pi Zero W | Notes                        | Link |
| ---------------- | -------------- | ------------------ | ------------------- | ------                       |      |
| RavPower 6700 mAh | 2.4 A         | Yes                | Yes                 | Metal case. Little on the heavy side. | [Amazon](https://www.amazon.com/Portable-Charger-RAVPower-6700mAh-External/dp/B00Y2PX4YI/) |
| GETIHU 5200 mAh | 2.4A            | Yes                | Yes                 | Plastic case. Good weight for use on smaller robots as well. | [Amazon](https://www.amazon.com/GETIHU-High-Speed-Pocket-Size-Powerbank-Flashlight/dp/B07CYNBNVW/) |
| Anker Astro E1 5200 mAh | 2.0A | Yes | Yes | Similar in size and weight to GETIHU option. Feels like slightly higher build quality. | [Amazon](https://www.amazon.com/Anker-bar-Sized-Portable-High-Speed-Technology/dp/B00P7N0320/) |
| Anker Astro E1 6700 mAh | 2.0A | Yes | Yes | Similar to 5200 mAh version | [Amazon](https://www.amazon.com/Anker-Upgraded-Candy-Bar-High-Speed-Technology/dp/B06XS9RMWS/) |
| EnergyCell 10000 | 2.4A | Yes | Yes | NOT TESTED! SPECS AND CAPABILITIES BASED ON AMAZON LISTING | [Amazon](https://www.amazon.com/10000mAh-Ultra-Compact-High-Speed-Compatible-More-Black/dp/B09B3FSL1T) |
| Miady 10000 mAh | 2.4A | Yes | Yes | Decent build quality. Smaller size than many other options. | [Amazon](https://www.amazon.com/Miady-Portable-Charger-10000mAh-External/dp/B0842FCWGM) |
| Miady 5000 mAh | 2.4A | Yes | Yes | NOT TESTED! SPECS AND CAPABILITIES BASED ON AMAZON LISTING | [Amazon](https://www.amazon.com/Miady-Portable-Charger-5000mAh-Lightweight/dp/B08GLYSTLZ) |

#### **Supported Arduino Coprocessors**
Any Arduino compatible board, that can be interfaced with the Raspberry Pi via UART (be it by pins or by USB) should work. Be aware that some Arduinos are 5V devices and some are 3.3V devices (if USB is used to supply power it will always be 5V so there will be 5V power available, but the arduino itself won't use 5V logic levels). Some sensors require 5V power, but can use a 3.3V logic level; and some sensors only support a 5V logic level. Some 3.3V devices have I/O pins that are 5V tolerant (such as the Teensy 3.2) so they can work with most 5V devices. A logic level shifter could also be used. 

There are also many Arduino clone boards available, and often times they are not obviously clones on listings online (such as on Amazon). Some clones (the ones that are obviously not Arduino brand products) are good quality, but others are not. Be careful where you buy things from.

The table below is just the boards that have been tested and are known to work.

| Arduino | Board Voltage | Description | Product Link (Can find other sellers) |
| --- | --- | --- | --- |
| Arduino Nano | 5V | Small footprint, same features as Arduino Uno | [Arduino Store](https://store.arduino.cc/usa/arduino-nano) |
| Arduino Nano Every | 5V | Same size and footprint as Arduino Nano, but newer and cheaper. For use on a breadboard make sure to get the variant with headers. | [Arduino Store](https://store.arduino.cc/usa/nano-every-with-headers) |
| Sainsmart Nano v3 | 5V | Sainsmart Arduino Nano clone. Cheaper, uses different FTDI chip, but usually works as intended. There were reports of devices with "counterfit" FTDI chips being bricked by a windows driver in 2014, but the driver no longer does this. There are also two versions of this board: a blue one and a black one. I linked the blue one here because the black ones seem to becoming harder to find. | [Sainsmart Store](https://www.sainsmart.com/products/nano-v3-atmega328-au-arduino-compatible), [Amazon](https://www.amazon.com/SainSmart-Nano-v3-0-Compatible-Arduino/dp/B00761NDHI/ref=sr_1_4?keywords=sainsmart+nano&qid=1570720152&sr=8-4) |
| Teensy 3.2 | 3.3V, I/O Pins 5V Tolerant | Powerful microcontroller with a small form factor. More powerful than many other Arduino compatible boards with a similar form-factor. | [Adafruit](https://www.adafruit.com/product/2756) |

#### **Supported Motor Drivers**
Currently motor drivers are all connected to the Raspberry Pi. The arduino is not currently used as a motor driver.

| Motor Driver | Connection Type | Number of Motors | Logic Voltage | Motor Voltage | Max Current per Motor | Notes | Link |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Adafruit DRV8833 Module | GPIO/PWM with jumper wires | 2 | 2.7V min, motor voltage max | 2.7V - 10.8V | 1.2A |  | [Adafruit Store](https://www.adafruit.com/product/3297) |
| Adafruit TB6612 Module | GPIO/PWM with jumper wires | 2 | 2.7V-5V | 4.5V - 13.5V | 1.2A |Does not come with terminal block power input header. | [Adafruit Store](https://www.adafruit.com/product/2448) |
| L298N Module | GPIO/PWM with jumper wires | 2 | 2.7-7V | 5V-12V | 2A | Motor voltage can be up to 35V, but onboard 5V regulator cannot be used (remove jumper). An external 5V supply will be required. There is a ~1V drop between motor battery voltage and voltage applied to motors with this module. | [Amazon](https://www.amazon.com/Qunqi-2Packs-Controller-Stepper-Arduino/dp/B01M29YK5U/ref=sr_1_7?keywords=l298n&qid=1570723739&sr=8-7) |
| Adafruit Motor Hat | Pi GPIO Header (hat) I<sup>2</sup>C | 4 | 3.3V I<sup>2</sup>C | 4.5V-13.5V | 1.2A | Uses TB6612 chips, but communicates with Pi via I<sup>2</sup>C, so there is no need for software PWM. It comes with a standard header, but it is recommended to buy and use Adafruit's stacking header so other I/O pins are accessible. | [Adafruit Store](https://www.adafruit.com/product/2348) |
| Adafruit Motor Bonnet | Pi GPIO Header (hat) I<sup>2</sup>C | 4 | 3.3V I<sup>2</sup>C | 4.5V-13.5V | 1.2A | Uses TB6612 chips, but communicates with Pi via I<sup>2</sup>C, so there is no need for software PWM. It comes with a header soldered on. Other IO pins are not easily accessible. | [Adafruit Store](https://www.adafruit.com/product/4280) |
| Geekworm Motor Hat | Pi GPIO Header (hat) I<sup>2</sup>C | 4 | 3.3V I<sup>2</sup>C | 4.5V-13.5V | 1.2A | Seems to be lower quality "copy" of Adafruit's Hat. | [Amazon](https://www.amazon.com/Raspberry-Function-Expansion-Support-Stepper/dp/B0721MTJ3P/ref=sr_1_6?keywords=raspberry+pi+motor+hat&qid=1570724085&sr=8-6) | 


#### **Supported Raspberry Pi Sensors**
Most sensors require an arduino either due to timing, sensor communication requirements, or voltage requirements. Some however, work well with a Raspberry Pi and are easier to use without involving the Arduino.

| Sensor | Description | Link |
| --- | --- | --- |
| Adafruit INA260 Voltage, Current Sensor | Measures motor battery voltage and current consumption of motors. Communicates with Pi via I<sup>2</sup>C. Provides more acurate voltage readings than a voltage divider does with the arduino and provides current readings. | [Adafruit Store](https://www.adafruit.com/product/4226) |


#### **Supported Arduino Sensors**
| Sensor Type | Sensor Description | Link(s) |
| --- | --- | --- |
| Rotary Encoders | Any encoder with a single data line (single channel encoders). Can be used to determine distance traveled or speed, but cannot distinguish between directions. Commonly made with slotted disks and photo interrupters. | [TT Motor Disk](https://www.adafruit.com/product/3782)<br /> [Photo Interrupter (No breakout)](https://www.adafruit.com/product/3986) <br /> [Breakout Board - Amazon 1](https://www.amazon.com/Willwin-Measuring-Comparator-Optocoupler-Arduino/dp/B0776RHKB1/) <br /> [Breakout Board - Amazon 2](https://www.amazon.com/DAOKI-Optocoupler-Sensor-Module-Arduino/dp/B01MRELRS1) <br /> [Untested Breakout Board - Amazon 1](https://www.amazon.com/Measuring-Optocoupler-Interrupter-Detection-Arduino%EF%BC%885pcs%EF%BC%89/dp/B08977QFK5/) <br /> [Untested Breakout Board - Amazon 2](https://www.amazon.com/DAOKI-Measuring-Optocoupler-Interrupter-Detection/dp/B081W4KMHC/) |
| Old Adafruit 9DOF IMU | No longer sold. Legacy component. Gyro, accelerometer, and magnetometer<sup>&ast;</sup> combo board. | [Adafruit Store](https://www.adafruit.com/product/1714) |
| Adafruit NXP 9DOF IMU | Newer gyro, accelerometer, and magnetometer<sup>&ast;</sup> combo board. | [Adafruit Store](https://www.adafruit.com/product/3463) |
| MPU6050 IMU | 3-axis gyro + 3-axis accelerometer. Often available on an easy to use breakout board. | [Adafruit Breakout Board](https://www.adafruit.com/product/3886) <br /> [Cable for Adafruit Board](https://www.adafruit.com/product/4209) <br /> [Sparkfun Breakout Board](https://www.sparkfun.com/products/11028) |
| 4-Pin Ultrasonic Sensors | Ultrasonic sensors with trigger and echo pins. Most common is HC-SR04, which is 5V only. There are other compatible devices that work at 3.3V though. | [Standard HC-SR04](https://www.adafruit.com/product/3942)<br /> [5V/3V Compatible Device](https://www.adafruit.com/product/4007) <br /> [HC-SR04 Amazon 1](https://www.amazon.com/SainSmart-HC-SR04-Ranging-Detector-Distance/dp/B004U8TOE6/) |
| Analog Input Voltage Monitor | Using an analog input to measure motor battery voltage, usually through a votage divider. Can assemble on breadboard with through hole resistors or buy pre-assembled voltage divider. | [Voltage Divider Module - Amazon 1](https://www.amazon.com/Diymall-Voltage-Sensor-Dc0-25v-Arduino/dp/B00NK4L97Q/) <br /> [Voltage Divider Module - Amazon 2](https://www.amazon.com/Voltage-Measurement-Detection-Arduino-Geekstory/dp/B07FVVSYYH/) <br /> [Voltage Divider Module - Amazon 3](https://www.amazon.com/HiLetgo-Voltage-Detection-Arduino-Electronic/dp/B01HTC4XKY/) <br /> [Voltage Divider Module - Amazon 4](https://www.amazon.com/UMLIFE-Detection-Terminal-Measurement-Raspberry/dp/B096VCYD6M/) |
| IR Emitter / Detector | Consists of an IR LED that emits light that is reflected off a surface and seen by an IR detector. The surface determines how much light is reflected. Used for line follower robots. Can be built with an LED and a detector (photodiode or phototransistor), but can also often be found as modules | [Longer Range Module - Amazon 1](https://www.amazon.com/DIYmall-Avoidance-Transmitting-Receiving-Photoelectric/dp/B07K6PYFZS/) <br /> [Longer Range Module - Amazon 2](https://www.amazon.com/HiLetgo-Infrared-Avoidance-Reflective-Photoelectric/dp/B07W97H2WS/) <br /> [Shorter Range Module - Amazon 1](https://www.amazon.com/HiLetgo-Channel-Tracing-Sensor-Detection/dp/B00LZV1V10/) <br /> [Shorter Range Module - Amazon 2](https://www.amazon.com/OSOYOO-TCRT5000-Infrared-Reflective-Photoelectric/dp/B07C69N65P/) |


<sup>&ast;</sup>Use of the magnetometer requires custom modifications to the arduino firmware and is often not very useful due to the magnetic interference caused by motors.

#### **Motors**
*Some chassis kits come with motors and other components.*

| Item | Description/Notes | Link(s) |
| ---- | ----------------- | ------- |
| TT Motor | Most common motor option. It is cheap and well-tested. Gear ratio is 1:48. Many kits come with these. | [Adafruit](https://www.adafruit.com/product/3777) |
| Angled TT Motor | These are not often used, but some kits do use them. Gear ratio is 1:120 | [RobotShop](https://www.robotshop.com/en/dfrobot-6v-180-rpm-micro-dc-geared-motor-with-back-shaft.html), [DFRobot](https://www.dfrobot.com/product-100.html?search=tt%20motor&description=true) |
| High Torque TT Motor | Same shape as standard TT Motor, but 1:120 gear ratio. These are hard to find. | [Banggood](https://www.banggood.com/DC-3V-6V-DC-1120-Gear-Motor-TT-Motor-for-Arduino-Smart-Car-Robot-DIY-p-1260117.html?cur_warehouse=CN) |
| High Torque TT Motor w/ Quad Encoder | Same as high torque TT motor (1:120 gear ratio), but with builtin quadrature encoder. | [RobotShop](https://www.robotshop.com/en/micro-6v-160rpm-1201-dc-geared-motor-encoder.html), [DFRobot](https://www.dfrobot.com/product-1457.html?search=tt%20motor&description=true) |
| Angled TT Motor w/ Quad Encoder | Same as angled TT Motor (1:120 gear ratio), but had builtin quadrature encoder. | [RobotShop](https://www.robotshop.com/en/6v-1201-160rpm-micro-dc-geared-motor-encoder.html), [DFRobot](https://www.dfrobot.com/product-1458.html?search=tt%20motor&description=true) |

*NOTE: There is currently no software support in the arduino firmware for quadrature encoders. Support would have to be added as a custom addition to the firmware for now.*

#### **Kit bases/chassis**

*You can always make your own robot chassis. See the cardboard / clipboard robot in the [example builds](./examplebuilds.md) section.*

| Item | Description/Notes | Link(s) |
| ---- | ----------------- | ------- |
| Acrylic 2WD | Acrylic 2 wheel drive robot, with caster wheel. | [Amazon](https://www.amazon.com/perseids-Chassis-Encoder-Wheels-Battery/dp/B07DNYQ3PX/) |
| Acrylic 4WD | Acrylic 4 wheel drive robot. Dual layer. | [Amazon](https://www.amazon.com/perseids-Chassis-Encoder-Wheels-Battery/dp/B07DNXBFQN/) |
| DFRobot Pirate 4WD | Aluminum, high quality, dual layer, 4-wheel drive robot chasis. | [RobotShop](https://www.robotshop.com/en/dfrobot-4wd-arduino-mobile-platform.html),  [DFRobot](https://www.dfrobot.com/product-97.html?search=pirate&description=true) |

#### **Other Components and Tools**

| Item | Purpose | Description/Notes | Link(s) |
| ---- | ------- | ----------------- | ------- |
| Hook and Loop tape | Attach components to robot | Can be used to mount Raspberry Pi, breadboards, motor drivers, etc. | [Amazon](https://www.amazon.com/Strenco-Inch-Self-Adhesive-Hook/dp/B00FQ937NM/) |
| Twist Ties | Cable Management | Plastic ones hold up better than paper ones. | [Amazon](https://www.amazon.com/Easytle-Plastic-Black-Twist-Cable/dp/B00ZV42X1Y/ref=sr_1_4?keywords=twist+ties&qid=1570912236&sr=8-4) |
| Solder Iron Kit | Attach leads to motors, mount header pins, repair work, etc. | The linked product is a simple soldering kit. Better irons do exist, but this should be good enough for many simple tasks. | [Amazon](https://www.amazon.com/ANBES-Soldering-Iron-Kit-Electronics/dp/B06XZ31W3M/ref=sr_1_4?keywords=solder+iron&qid=1570912527&sr=8-4) |
| Multimeter | Measurement and debugging | The linked product is a simple multimeter, but will work for simple tasks. | [Amazon](https://www.amazon.com/Etekcity-Multimeter-MSR-R500-Electronic-Multimeters/dp/B01N9QW620/ref=sr_1_5?keywords=multimeter&qid=1570912789&sr=8-5) |
| Wire Stripper | Removing insulation from wires | Linked item on amazon also works as a crimper for some connectors. | [Amazon](https://www.amazon.com/dp/B000JNNWQ2/ref=twister_B01E3QOEZI?_encoding=UTF8&psc=1) |
| Jumper Wires (Preassembled) | Connections to header pins and breadboard | Assorted pack | [Amazon](https://www.amazon.com/EDGELEC-Breadboard-Optional-Assorted-Multicolored/dp/B07GD2BWPY/) |
| Wire | Motor leads, make jumpers | 24AWG is a good size for jumper wires | [Amazon](https://www.amazon.com/Stranded-Nano-Flexible-Insulated-Electrical/dp/B07DCV7BDD/ref=sxbs_sp_mra?keywords=stranded+wire&pd_rd_i=B07DCV7BDD&pd_rd_r=cbc209fd-bc69-454f-b4e2-b81a483ad1ea&pd_rd_w=Dj393&pd_rd_wg=jectf&pf_rd_p=2e675d27-2176-48bd-bab7-a89e5b36d9de&pf_rd_r=NZCQ5W3CMJZWHT8XJV26&qid=1570914425&s=toys-and-games)
| Mini Breadboard | Hold arduino and sensors. Wiring. | Mini breadboard does not leave much room for anything but arduino. | [Adafruit](https://www.adafruit.com/product/65) |
| Half Size Breadboard | Hold arduino and sensors. Wiring. | Half sized breadboard will fit on larger robots. | [Adafruit](https://www.adafruit.com/product/64) |
| Male .1" Header Pins | Use on arduino or sensor. | Often come with parts, but sometimes more are necessary. | [Adafruit](https://www.adafruit.com/product/392) |
| Logic Level Shifter | Convert between logic levels (ex 3.3V and 5V) | Can find similar cheaper in packs of 5-10 on amazon if you need multiple. | [Adafruit](https://www.adafruit.com/product/757) |
| Dupont Kit | Making jumper wires | This kit comes with many housings, male and female, pins. | [Amazon](https://www.amazon.com/QLOUNI-Housing-Connector-Adaptor-Assortment/dp/B0774NMT1S/ref=sr_1_7?keywords=dupont+pin+kit&qid=1570914779&sr=8-7) |
| Dupont Crimper | Crimp dupont connectors (ex. for jumper wire) | There are cheaper crimpers, but not all work very well. | [Amazon](https://www.amazon.com/IWISS-Professional-Compression-Ratcheting-Wire-electrode/dp/B00OMM4YUY/ref=sr_1_4?keywords=dupont+crimper&qid=1570914838&sr=8-4) |
| 5x AA holder | Hold batteries | These are hard to find | [Amazon (not prime)](https://www.amazon.com/TOOGOO-1-5V-Battery-Holder-Black/dp/B00L316FNQ/ref=sr_1_6?keywords=5x+aa+holder&qid=1570914964&s=electronics&sr=1-6), [Amazon (5 pack)](https://www.amazon.com/Vanki-Battery-Holder-Case-Leads/dp/B073XC52BG/ref=sr_1_1?keywords=5x+aa+holder&qid=1570914964&s=electronics&sr=1-1), [Adafruit (Connector on end)](https://www.adafruit.com/product/3456) |
| 6x AA holder | Hold batteries | These use a 9V style header | [Amazon (2 pack)](https://www.amazon.com/gp/product/B019P0VDRO?pf_rd_p=183f5289-9dc0-416f-942e-e8f213ef368b&pf_rd_r=YQ9STZ6D446A2YX411RA)
| Panel Mount Switch | Main power switch | These can be mounted with hot glue. | [Adafruit](https://www.adafruit.com/product/3221) |
| Breadboard compatible switch | Main power switch  | These sit in a breadboard | [Adafruit](https://www.adafruit.com/product/805) |
| Hot glue gun | Mount some (non-PCB) components. | | [Amazon](https://www.amazon.com/ccbetter-Upgraded-Removable-Anti-hot-Flexible/dp/B01178RVI2/ref=sr_1_4?keywords=hot+glue+gun&qid=1570915241&sr=8-4) |
