
## Where to Buy Components

Note: These are not the only possible places to buy components. These are just some general starting points.

??? info "Sources"
    [Adafruit](https://www.adafruit.com/) - Sell many custom microcontroller boards and sensor / motor controller breakout boards. Also sell Raspberry Pi computers and various components such as motors. 

    [SparkFun](https://www.sparkfun.com/) - Similar to Adafruit, sell many custom sensor / motor controller boards. Sell Arduinos and custom microcontroller boards as well as Raspberry Pi computers, motors, and various other components.

    [Digikey](https://www.digikey.com/) - Sell various electronic chips and components. Also stock most / all Adafruit parts (and usually allow back ordering).

    [Mouser](https://www.mouser.com/) -  Sell various electronic chips and components. Also stock many Adafruit parts (and usually allow back ordering).

    [Amazon](https://www.amazon.com/) - Good source for "common" parts being used. Sometimes motors and some sensor boards can be found. Mostly good for "generic" type parts and common parts.

    [Banggood](https://www.banggood.com/) - Stocks many items to be purchased online. Similar to Amazon, but slow shipping from China. Sometimes cheaper prices than Amazon and sometimes slightly different availability.

    [DFRobot](https://www.dfrobot.com/) - Provide many robot kits and components. The kits often include electronics that are not easily usable with the ArPiRobot framework, but the robot chases can be useful. Additionally, some sensors and motors are useful.

    [Robotshop](https://www.robotshop.com/) - Stock many components, including many DFRobot motors with cheaper shipping.

    [Arduino Store](https://store.arduino.cc/usa/) - This is sometimes the only place to easily find some Arduino boards.


*Note: In many cases, the components listed below are examples of a type of product. The linked items are intended to serve as examples and are not endorsements of a specific item / specific items.*


## Mechanical Components

### Base / Frame / Chassis

Sometimes, kits are used for the robot's base / frame / chassis, however this is also frequently custom built. Some kits are linked here. This is not a complete list of kits. Amazon is a good place to look for chassis kits. DFRobot also has several custom kits available.

??? info "Kits"
    | Chassis Kit    | Description                                             | Link(s)           |
    | -------------- | ------------------------------------------------------- | ----------------- |
    | Generic 2WD Acrylic | This is a simple chassis cut out of acrylic. It is semi-decent quality (for acrylic), however acrylic will crack over time. Additionally, the space for electrical components is relatively small. This is decent for a low cost, simple build with relatively few sensors, but won't scale well to more complex robots. Often comes as a kit including motors, wheels, a AA battery pack, and mounting hardware. | [Amazon Source 1](https://www.amazon.com/perseids-Chassis-Encoder-Wheels-Battery/dp/B07DNYQ3PX/) <br /> [Amazon Source 2](https://www.amazon.com/Smart-Chassis-Motors-Encoder-Battery/dp/B01LXY7CM3/) <br /> [Amazon Source 3](https://www.amazon.com/YIKESHU-Smart-Chassis-Encoder-Battery/dp/B073VHQT6P/) |
    | Generic 4WD Acrylic | This is a simple chassis cut out of acrylic. It is semi-decent quality (for acrylic), however acrylic will crack over time. the two layers allow a little extra space for electrical components compared to the 2WD kit linked above, however space is still limited. Often comes as a kit including motors, wheels, a battery pack, and mounting hardware. However, usually comes with a 4x AA battery pack, which is barely sufficient for a 4WD robot. 5 AA batteries are highly recommended instead. There are also variations that include mecanum wheels. | [Amazon Source 1](https://www.amazon.com/perseids-Chassis-Encoder-Wheels-Battery/dp/B07DNXBFQN/) <br /> [Amazon Source 2](https://www.amazon.com/MakerFocus-Chassis-MEGA2560-MEGA1280-Microcontroller/dp/B01LYZDP9U/) <br /> [Amazon Source 3](https://www.amazon.com/Mixse-Smart-Robot-car-Raspberry/dp/B07412K5RP/) <br /> [Amazon Source 4](https://www.amazon.com/YIKESHU-Smart-Chassis-Encoder-Battery/dp/B075LD4FPN/) <br /> [Banggood Source 1](https://www.banggood.com/4WD-Smart-Robot-Chassis-Car-DIY-Kits-With-Magneto-Speed-Encoder-For-Arduino-51-p-1145101.html) <br /> [Banggood Source 2](https://www.banggood.com/DIY-4WD-Double-Deck-Smart-Robot-Car-Chassis-Kits-with-Speed-Encoder-p-1311469.html) | 
    | DFRobot 4WD (Pirate) | Good quality Aluminum robot chassis including 4 motors. Motors included are a 1:120 gear ratio as opposed to the more common 1:48 gear ratio. Provides ample space for electrical components, but not much space for more complex mechanical systems. | [DFRobot Shop](https://www.dfrobot.com/product-97.html) <br />  |


### Motors and Wheels

The list below includes commonly used motors. Any motor should be usable, provided it is usable with the motor driver and power source used. The power source will need to be of a sufficient voltage and the motor controller (motor driver) will need to be able to supply enough current for the motor.

??? info "Motors"
    | Motor            | Description                                              | Link(s)          |
    | ---------------- | -------------------------------------------------------- | ---------------- |
    | TT Motor 1:48    | Commonly available low cost motors with a plastic gearbox with a double-D output shaft. Most often, these have a 1:48 gear ratio and are constructed with plastic gears. These motors are generally listed as being 3V to 6V motors (but are often run up to 9V without issue). These motors are relatively low current, small, and not too powerful, however are sufficient for a small robot's drive system. Sometimes these motors come with wheels and / or wires pre-soldered to the motor leads. Other times, they do not. These also sometimes come in multi packs. | [Amazon Source 1](https://www.amazon.com/Electric-Magnetic-Gearbox-Plastic-Yeeco/dp/B07DQGX369/) <br /> [Amazon Source 2](https://www.amazon.com/MELIFE-Electric-Magnetic-Gearbox-Plastic/dp/B0978DGXY9/) <br /> [Amazon Source 3](https://www.amazon.com/AEDIKO-Motor-Gearbox-Shaft-200RPM/dp/B099Z85573/) <br /> [Amazon Source 4](https://www.amazon.com/ACEIRMC-Magnetic-Electric-Vibration-Products/dp/B098Q5MMSH/) <br /> [Adafruit](https://www.adafruit.com/product/3777) <br /> [Banggood Source 1](https://www.banggood.com/DC-3V-6V-Dual-Axis-Gear-Motor-TT-Motor-For-Smart-Chassis-Car-Geekcreit-for-Arduino-products-that-work-with-official-Arduino-boards-p-916210.html) <br /> [Banggood Source 2](https://www.banggood.com/5Pcs-DC-3V-6V-Dual-Axis-Gear-Reducer-TT-Motor-For-Smart-Robot-Car-p-949555.html) <br /> [Banggood Source 3](https://www.banggood.com/Small-Hammer-130-TT-DC-Gear-Motor-10cm-15cm-Dupont-Line-Male-Plug-For-Smart-Robot-Car-DIY-p-1531792.html) <br /> [Robot Shop Source 1](https://www.robotshop.com/en/dagu-65mm-wheel-pair-tt-3-6v-dc-gear-motor.html) <br / > [Robot Shop Source 2](https://www.robotshop.com/en/dc-electric-motor-3-6v-148-dual-shaft-geared-tt-magnetic-gearbox-6x.html) |
    | TT Motor 1:120   | Similar to 1:48 version, but harder to find. All gearbox components are still plastic, so despite the higher gear ratio, these cannot withstand too much torque. These are often impossible to source from Amazon or Banggood sellers. Best option is to search "TT Motor 1:120" on ebay or Aliexpress. *Links provided to a search not specific parts as sellers change often. Double check gear ratio before ordering.* | [ebay](https://www.ebay.com/sch/i.html?_nkw=tt+motor+1%3A120) <br /> [AliExpress](https://www.aliexpress.com/premium/tt-motor-1%25253A120.html) |
    | Angled TT Motor  | Similar to other TT Motors above, but output shaft is 90 degrees angle from motor input shaft. These are harder to find. These are also usually 1:120 gear ratio. | [Robot Shop](https://www.robotshop.com/en/dfrobot-6v-180-rpm-micro-dc-geared-motor-with-back-shaft.html) <br /> [DFRobot](https://www.dfrobot.com/product-100.html) |
    | Various Pololu Plastic Gearmotors | Similar in form factor and capabilities to the common "TT" motors, but have a different output shaft. | [Pololu](https://www.pololu.com/category/158/mini-plastic-gearmotors) |
    | Various Pololu Metal Gearmotors | May options including larger higher voltage / current motors capable of generating / withstanding higher torque. Output shafts vary. | [Pololu](https://www.pololu.com/category/51/pololu-metal-gearmotors) |
    | TT Motor Bi-Metal | Similar to TT Motor, but with some metal internal gears. 1:90 gear ratio. Note: these have no rear output shaft making encoders more difficult. | [Adafruit](https://www.adafruit.com/product/3801) | 
    | TT Motor All-Metal | Similar to TT Motor ,but all internal gears are metal. 1:90 gear ratio. Note: these have no rear output shaft making encoders more difficult. | [Adafruit](https://www.adafruit.com/product/3802) |
    

??? info "Wheels"
    | Wheel         | Description                                              | Link(s)           |
    | ------------- | -------------------------------------------------------- | ----------------- |
    | Generic Yellow & Black TT Motor Wheel | Commonly sold with motors or drive chassis kits. Hard to find without motors. Links provided only for wheels alone. On Amazon and Bangood motors will usually come with wheels. Searching "TT Motor Wheel" on AliExpress seems to be the best option to find the wheels without motors. | [AliExpress Source 1](https://www.aliexpress.com/item/3256802814198356.html) <br /> [AliExpress Source 2](https://www.aliexpress.com/item/3256803888132912.html) <br /> [AliExpress Source 3](https://www.aliexpress.com/item/2255801038877041.html) <br /> [AliExpress Source 4](https://www.aliexpress.com/item/2251801468961827.html) <br /> [AliExpress Source 5](https://www.aliexpress.com/item/3256802814198356.html) |
    | Orange TT Motor Wheel | Similar to generic wheel, but grippier rubber. | [Adafruit](https://www.adafruit.com/product/3766) |
    | Thin White TT Motor Wheel | Thinner TT Motor wheel. | [Adafruit](https://www.adafruit.com/product/3763) |
    | Skinny TT Motor Wheel | Even thinner TT Motor wheel. | [Adafruit](https://www.adafruit.com/product/3757) |
    | Various Pololu Wheels | Various wheels that work with Pololu Motors | [Pololu](https://www.pololu.com/category/89/pololu-wheels-and-tracks) |
    | TT Motor Mecanum Wheels | Mecanum wheels that work with TT Motors. | [Adafruit: 48mm Left](https://www.adafruit.com/product/4679) <br /> [Adafruit: 48mm Right](https://www.adafruit.com/product/4678) <br /> [Robot Shop: 48mm Set](https://www.robotshop.com/en/mecanum-wheel-sets-for-tt-motor-48mm.html) <br /> [Robot Shop: 60mm Set](https://www.robotshop.com/en/mecanum-wheel-sets-for-tt-motor-60mm.html) |


## Electrical Components

### Main Computer

Currently, only Raspberry Pi boards are officially supported. This is due to the OS image used and the IO backends in the CoreLib. Other boards could be used, but would require custom configuring the operating system (which is not a trivial process) and likely implementing a new IO provider in the CoreLib (also not a trivial process). Additionally, a board with a WiFi adapter is required for full functionality.


??? info "Raspberry Pi Boards"
    There are thee main types of Raspberry Pi boards

    - Model B boards: "Full size" Raspberry Pis. Small, but larger than the other options.
    - Model A boards: Smaller boards (size of a Pi hat). Also generally slightly less powerful than same generation Model B board.
    - Zero series boards: Very small boards (about half the size of a Model A board). Generally significantly less powerful than similarly aged Model B / Model A boards.

    There are several generations of Raspberry Pi boards. Model B and A boards share the same generation numbers (though there isn't a Model A for each generation). The zero series has its own generation numbers. Also note that there are sometimes "plus variants" (eg Model B+). Typically these are similar to the non-plus variants.

    When choosing a Raspberry Pi board there are a few things to consider:

    - Not all Raspberry Pi boards have WiFi builtin. Generation 1 and 2 boards do not. The Pi Zero (non W) also does not.
    - 64-bit capable boards are recommended, however 32-bit boards are still supported.
    - Single core boards are not good for complex software. More RAM is also beneficial.
    - Raspberry Pi 4 Model B may require heatsinks and / or a fan. As such, it is often no the best choice for robots.
    - Generally, Raspberry Pi 3A+ or Raspberry Pi Zero 2 W boards are recommended as they are smaller, but still provide adequate computational power for most robots.
    - The Raspberry Pi Zero W will work, but is generally not recommended as it is a single core board.

    The following boards are recommended for use with the ArPiRobot Framework

    | Board                   | Number of Cores    | RAM         | 64-bit | Link           |
    | ----------------------- | ------------------ | ----------- | ------ | -------------- |
    | Raspberry Pi 4 Model B  | 4                  | 1GB - 8GB   | Yes    | [Link](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/) |
    | Raspberry Pi 3 Model A+ | 4                  | 512MB       | Yes    | [Link](https://www.raspberrypi.com/products/raspberry-pi-3-model-a-plus/) |
    | Raspberry Pi 3 Model B  | 4                  | 1GB         | Yes    | [Link](https://www.raspberrypi.com/products/raspberry-pi-3-model-b/) |
    | Raspberry Pi 3 Model B+ | 4                  | 1GB         | Yes    | [Link](https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/) |
    | Raspberry Pi Zero 2 W   | 4                  | 512MB       | Yes    | [Link](https://www.raspberrypi.com/products/raspberry-pi-zero-2-w/) |
    | Raspberry Pi Zero W     | 1                  | 512MB       | No     | [Link](https://www.raspberrypi.com/products/raspberry-pi-zero-w/) |



### Computer Power Sources

Generally, it is easiest to have two power sources on the robot. This avoids scenarios where motors changing speed cause a voltage drop that reboots the computer. Often, the computer power source is a USB battery pack.

When selecting a power source, be aware that it needs to be able to supply a sufficient amount of current. The current requirement mostly depends on the main computer. In general, using a 2.4A power source is a safe option (3A is better for a Pi 4B) and is generally recommended. Depending on how many sensors and other devices are connected, lower currents may be required. The following are minimum recommendations by each computer. These minimums are lower than commonly recommended due to the fact that the computer is generally not driving a display when used on a robot.

- 2.1A for Pi 3A+, Pi 3B+, and Pi 3B
- 2.4A for Pi 4B
- 1.0A for Pi Zero W
- 1.5A for Pi Zero 2 W

*Note: Higher capacity battery packs last longer.*


??? info "USB Battery Packs"
    | Battery Pack            | Capacity             | Output Current (max) | Link(s)      |
    | ----------------------- | -------------------- | -------------------- | ------------ |
    | Anker Astro E1          | 5200 mAh / 6700mAh   | 2.1A                 | [Amazon (5200mAh)](https://www.amazon.com/Anker-bar-Sized-Portable-High-Speed-Technology/dp/B00P7N0320) <br /> [Amazon (6700mAh)](https://www.amazon.com/Anker-Upgraded-Candy-Bar-High-Speed-Technology/dp/B06XS9RMWS/) |
    | EnergyCell Compact      | 10000 mAh            | 2.4A                 | [Amazon](https://www.amazon.com/10000mAh-Ultra-Compact-High-Speed-Compatible-More-Black/dp/B09B3FSL1T) |
    | Miday Portable Charger  | 10000 mAh            | 2.4A                 | [Amazon](https://www.amazon.com/Miady-Portable-Charger-10000mAh-External/dp/B0842FCWGM) |
    | Miday Portable Charger  | 5000 mAh             | 2.4A                 | [Amazon](https://www.amazon.com/Miady-Portable-Charger-5000mAh-Lightweight/dp/B08GLYSTLZ) |

??? info "Other Options"
    Voltage regulators can be used to regulate power from the same source that powers motors (or another unregulated source). Raspberry Pi's require a 5V supply capable of the currents indicated above. Keep this in mind when selecting a regulator. Also be aware of the regulator's input voltage requirements. Some example regulators are listed below
    
    - 5V/3A UBEC from [Adafruit](https://www.adafruit.com/product/1385)
    - 5V/3A UBEC from ShareGoo ([Amazon](https://www.amazon.com/ShareGoo-Converter-Module-Quadcopter-Holder/dp/B07DYXTX9H/))


    There are also other batteries designed for use with a raspberry pi. Some are listed below.
    
    - [PiJuice Hat](https://www.amazon.com/PiJuice-HAT-Portable-Platform-Raspberry/dp/B0788B9YGW)
    - [Kuman UPS Battery Pack Board](https://www.amazon.com/Kuman-Lithium-Battery-Expansion-Raspberry/dp/B07FDBZCXG)
    - [Geekworm Raspberry Pi UPS HAT](https://www.amazon.com/Geekworm-Raspberry-X706-Function-Compatible/dp/B096FT6THL/)


### Motor Power Sources

The "easiest" power source for motors is a AA battery pack. At least 4AA batteries are recommended for two TT motors and 5 AA batteries for 4 TT motors. When using TT motors, more than 6AA batteries is not recommended (or 8 if using rechargeable). Other motors may require different voltages / current draw and may need different types or numbers of batteries.

??? info "AA Battery Packs"
    *Note: these are just holders for the batteries. Batteries are not included. If using rechargeable AA's it is recommended to use one or two more batteries than the minimum recommended above (rechargeable are 1.2V per not 1.5V per).*
    
    | Item                   | Link(s) |
    | ---------------------- | ------- |
    | 4x AA Holder           | [Amazon 1](https://www.amazon.com/LAMPVPATH-Battery-Holder-Leads-Wires/dp/B07T7MTRZX/) <br /> [Amazon 2](https://www.amazon.com/WMYCONGCONG-Battery-Standard-Connector-Plastic/dp/B07XBJLFL3/) <br /> [Amazon 3](https://www.amazon.com/LAMPVPATH-Battery-Holder-Switch-Leads/dp/B07C6T6YCZ/) <br /> [Adafruit 1](https://www.adafruit.com/product/3859) <br /> [Adafruit 2](https://www.adafruit.com/product/830) |
    | 5x AA Holder           | [Amazon 1](https://www.amazon.com/Vanki-Battery-Holder-Case-Leads/dp/B073XC52BG/) <br /> [Amazon 2](https://www.amazon.com/LampVPath-Battery-Holder-Leads-Wires/dp/B07WRQ44YK/) <br /> [Adafruit](https://www.adafruit.com/product/3456) |
    | 6x AA Holder           | [Amazon 1](https://www.amazon.com/Hilitchi-Thicken-Battery-Standard-Connector/dp/B019P0VDRO/) <br /> [Amazon 2](https://www.amazon.com/Battery-Holder-Enclosure-Connector-Cable/dp/B01N2INSBR/) <br />  [Adafruit](https://www.adafruit.com/product/248) |
    | 8x AA Holder           | [Amazon 1](https://www.amazon.com/Thicken-Battery-Holder-Standard-Connector/dp/B07WP1CYYW/) <br /> [Amazon 2](https://www.amazon.com/LAMPVPATH-Battery-Holder-Layers-Connector/dp/B093BVWX6T/) <br /> [Amazon 3](https://www.amazon.com/Thicken-Battery-Holder-Switch-QTEATAK/dp/B07WY48TFC/) <br /> [Adafruit](https://www.adafruit.com/product/875) |


Other types of batteries can also be used. Typically, NiMH batteries are the best option (from a safety standpoint), but LiPo batteries are usable too (however more caution must be used with them).

??? info "Other Batteries"
    - [7.2V 3600mAh NiMH Battery](https://www.amazon.com/Zeee-3600mAh-Battery-Traxxas-Associated/dp/B07VLKP6RJ/)
    - [6V 200mAh NiMH Battery](https://www.amazon.com/Tenergy-Rechargeable-Connector-Airplanes-Aircrafts/dp/B001BCOWLY/)
    - [2 Cell LiPo Battery 6200mAh](https://www.amazon.com/Zeee-Battery-6200mAh-Connector-Vehicles/dp/B0925GSYDN/)
    - [2 Cell LiPo battery 5200mAh](https://www.amazon.com/Zeee-Batteries-Dean-Style-Connector-Vehicles/dp/B076Z778MJ)
    - [NiMH Charger (6V-12V)](https://www.amazon.com/Tenergy-Universal-Batteries-Compatible-Connectors/dp/B003MXMJX8/)
    - [LiPo Charger](https://www.amazon.com/SKYRC-LiIon-Battery-Charger-Discharger/dp/B01MZ1ZZ7Z/)

### Supported Sensor Coprocessors

Officially supported sensor coprocessors must run the ArPiRobot Arduino Firmware. The following boards are officially supported by the firmware. However, other boards will likely work, but would require firmware modifications.

??? info "Supported Boards"
    
    Notes on Arduino Compatible Boards:

    - Official Arduino boards are open source hardware, meaning they can be easily copied. Thus, many "clones" of official boards exist on the market (often same board name with a different brand name). Clones are generally fine, however may cut corners to reduce cost. Additionally, there are some counterfeit boards which try to pass themselves off as official Arduino boards. These should be avoided (buy from reputable seller / site; if buying from Amazon, check the seller).
    - Many non-Arduino brand boards are compatible with Arduino software. Some of these are officially supported on the list below
    - Additionally, some non-Arduino brand boards are compatible with other boards (eg some boards are "Arduino Uno Compatible"). If a board is compatible with a supported model, it should work too. 
    - Also, some boards come with headers soldered, others come with loose headers (that can be soldered), and some do not come with headers. Be aware of this when buying a board. There are also sometimes variants with headers pre-soldered (Adafruit and SparkFun often sell soldered header versions that may not be linked here).

    Officially supported list:

    *Note: This list should be considered incomplete. This list includes board that have been tested / are known to work, but many other "compatible" boards should also work. For example, other boards that are "Arduino Uno Compatible" will work. Also, other clones should be compatible.*

    | Board                      | Description                                                               | Link(s)     |
    | -------------------------- | ------------------------------------------------------------------------- | ----------- |
    | Arduino Nano               | Low power board with a small size. Same chip as Arduino Uno.              | [Arduino Store](https://store-usa.arduino.cc/products/arduino-nano) <br /> [Amazon](https://www.amazon.com/Arduino-A000005-ARDUINO-Nano/dp/B0097AU5OU/) |
    | Arduino Uno                | Low power board, commonly included in kits.                               | [Arduino Store](https://store.arduino.cc/products/arduino-uno-rev3) <br /> [SparkFun](https://www.sparkfun.com/products/11021) <br /> [Amazon](https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/) |
    | Arduino Nano Every         | Slightly more powerful than original Nano; similar size and pinout.       | [Arduino Store](https://store-usa.arduino.cc/products/arduino-nano-every) <br /> [SparkFun](https://www.sparkfun.com/products/15590) <br /> [Amazon](https://www.amazon.com/Arduino-Nano-Every-Single-Board/dp/B07VX7MX27/) |
    | Teensy 3.1 / 3.2           | Small, but powerful chip. Capable of handling more sensors.               | [PJRC Store](https://www.pjrc.com/store/teensy32.html) <br /> [Adafruit](https://www.adafruit.com/product/2756) <br /> [SparkFun](https://www.sparkfun.com/products/13736) |
    | Raspberry Pi Pico          | Low cost, small, and powerful. Capable of handling many sensors.          | [Raspberry Pi Site](https://www.raspberrypi.com/products/raspberry-pi-pico/) <br /> [Adafruit](https://www.adafruit.com/product/4864) <br /> [SparkFun](https://www.sparkfun.com/products/17829) |
    | Feather S2                 | ESP32 based board with feather form factor. Small and powerful processor. Note: wireless features are not used. | [Adafruit](https://www.adafruit.com/product/4769) <br /> [Unexpected Maker Shop](https://unexpectedmaker.com/shop/feathers2-esp32-s2) |
    | Adafruit Metro 328         | Arduino Uno compatible board of a similar size to Arduino Uno.            | [Adafruit](https://www.adafruit.com/product/2488) |
    | Adafruit Metro Mini 328    | Arduino Uno compatible board of a similar size to Arduino Nano.           | [Adafruit](https://www.adafruit.com/product/2590) |


### Supported Sensors  

Most sensors are only supported using an sensor coprocessor. This is either due the how the sensors connect or requirements regarding timing when working with the sensors. However, there are a few sensors that are supported connected directly to the main computer.

??? info "Coprocessor Sensors"

    | Sensor                              | Description                                                           | Parts (Links)         |
    | ----------------------------------- | --------------------------------------------------------------------- | --------------------- |
    | Single Channel Encoders             | Used to determine how far something (such as a motor) has rotated or how fast it is rotating. Single channel encoders have only one signal channel and are capable only of determining distance or speed, but not the direction of rotation. Often constructed using a photo interrupter and a slotted disk. | [TT Motor Disk - Adafruit](https://www.adafruit.com/product/3782)<br /> [Photo Interrupter (No breakout)](https://www.adafruit.com/product/3986) <br /> [Breakout Board - Amazon 1](https://www.amazon.com/Willwin-Measuring-Comparator-Optocoupler-Arduino/dp/B0776RHKB1/) <br /> [Breakout Board - Amazon 2](https://www.amazon.com/DAOKI-Optocoupler-Sensor-Module-Arduino/dp/B01MRELRS1) <br /> [Breakout Board - Amazon 3](https://www.amazon.com/Measuring-Optocoupler-Interrupter-Detection-Arduino%EF%BC%885pcs%EF%BC%89/dp/B08977QFK5/) <br /> [Breakout Board - Amazon 4](https://www.amazon.com/DAOKI-Measuring-Optocoupler-Interrupter-Detection/dp/B081W4KMHC/) |
    | Quadrature Encoders                 | Used to determine how far something (such as a motor) has rotated or how fast it is rotating. Quadrature encoders have two signal channels and are capable of distinguishing direction of rotation. Sometimes these are magnetic and sometimes these are optical (two photo interrupters with a slotted disk and precise positioning). | [TT Motor w/ Builtin Encoder - DFRobot](https://www.dfrobot.com/product-1457.html) <br /> [TT Motor (L) w/ Builtin Encoder - DFRobot](https://www.dfrobot.com/product-1458.html) <br /> [TT Motor w/ Builtin Encoder - RobotShop](https://www.robotshop.com/en/micro-6v-160rpm-1201-dc-geared-motor-encoder.html) <br /> [TT Motor (L) w/ Builtin Encoder - RobotShop](https://www.robotshop.com/en/6v-1201-160rpm-micro-dc-geared-motor-encoder.html) <br /> [Custom Quadrature Encoder for TT Motor](../custom/quadencoder.md) |
    | Analog Voltage Monitors             | Measure a voltage on a sensor coprocessor's analog input pins. A voltage divider can be used to measure larger voltages. Voltage divider modules also exist and can be used instead of loose resistors. | [Voltage Divider Module - Amazon 1](https://www.amazon.com/Diymall-Voltage-Sensor-Dc0-25v-Arduino/dp/B00NK4L97Q/) <br /> [Voltage Divider Module - Amazon 2](https://www.amazon.com/Voltage-Measurement-Detection-Arduino-Geekstory/dp/B07FVVSYYH/) <br /> [Voltage Divider Module - Amazon 3](https://www.amazon.com/HiLetgo-Voltage-Detection-Arduino-Electronic/dp/B01HTC4XKY/) <br /> [Voltage Divider Module - Amazon 4](https://www.amazon.com/UMLIFE-Detection-Terminal-Measurement-Raspberry/dp/B096VCYD6M/) |
    | 4-Pin Ultrasonic Sensors            | Use sound to measure the distance to an object the sensor is facing. This version has 4 pins including a "trigger" and "echo" pin. | [Standard HC-SR04](https://www.adafruit.com/product/3942)<br /> [5V/3V Compatible Device](https://www.adafruit.com/product/4007) <br /> [HC-SR04 Amazon 1](https://www.amazon.com/SainSmart-HC-SR04-Ranging-Detector-Distance/dp/B004U8TOE6/) |
    | IR Reflection Detector              | Often used to detect dark lines on a light surface (line follower robots). Measure IR light reflected off a surface. | [Longer Range Module - Amazon 1](https://www.amazon.com/DIYmall-Avoidance-Transmitting-Receiving-Photoelectric/dp/B07K6PYFZS/) <br /> [Longer Range Module - Amazon 2](https://www.amazon.com/HiLetgo-Infrared-Avoidance-Reflective-Photoelectric/dp/B07W97H2WS/) <br /> [Shorter Range Module - Amazon 1](https://www.amazon.com/HiLetgo-Channel-Tracing-Sensor-Detection/dp/B00LZV1V10/) <br /> [Shorter Range Module - Amazon 2](https://www.amazon.com/OSOYOO-TCRT5000-Infrared-Reflective-Photoelectric/dp/B07C69N65P/) |
    | L3GD20H + LSM303 IMU                | 9DOF sensor (3 axis gyroscope + 3 axis accelerometer + 3 axis magnetometer). Intended to be used with Adafruit's old 9DOF IMU board | [Adafruit](https://www.adafruit.com/product/1714) |
    | FXOS8700 + FXAS21002 IMU            | 9DOF sensor (3 axis gyroscope + 3 axis accelerometer + 3 axis magnetometer). Intended to be used with Adafruit's NXP 9DOF IMU board | [Adafruit](https://www.adafruit.com/product/3463) |
    | MPU-6050 IMU                        | 6DOF sensor (3 axis gyroscope + 3 axis accelerometer). This is a common low cost IMU chip. | [Adafruit Breakout Board](https://www.adafruit.com/product/3886) <br /> [Cable for Adafruit Board](https://www.adafruit.com/product/4209) <br /> [Sparkfun Breakout Board](https://www.sparkfun.com/products/11028) |

??? info "Main Computer Sensors"

    | Sensor                              | Description                                                           | Parts (Links)         |
    | ----------------------------------- | --------------------------------------------------------------------- | --------------------- |
    | INA260 Voltage + Current Sensor     | Voltage and current sensor capable of interfacing directly with the main computer (good for robots without a sensor coprocessor). Also provides higher accuracy readings than are typically possible with an Analog Voltage Monitor on a sensor coprocessor. | [Adafruit](https://www.adafruit.com/product/4226) |



## Tools & Miscellaneous

??? info "Other Electrical Components"

    | Item               | Purpose                                                                            | Link(s)    |
    | ------------------ | ---------------------------------------------------------------------------------- | ---------- |
    | LEDs               | Indicators to user. Connect to main computer's GPIO pins. LEDs require a current limiting resistor. | [Adafruit 5mm](https://www.adafruit.com/product/4203) <br /> [Amazon 5mm - 1](https://www.amazon.com/eBoot-Pieces-Emitting-Diodes-Assorted/dp/B06XPV4CSH/) <br /> [Amazon 5mm - 2](https://www.amazon.com/MCIGICM-Circuit-Assorted-Science-Experiment/dp/B07PG84V17/) <br /> [SparkFun 5mm Builtin Resistor](https://www.sparkfun.com/products/14563) |
    | Micro SD Card      | Storage for main computer OS. Generally A1 or A2 rated cards are faster when running an OS from the card. | [SanDisk Ultra (A1) - Amazon](https://www.amazon.com/SanDisk-256GB-MicroSDXC-Memory-Adapter/dp/B08GYBBBBH/) <br /> [SanDisk Extreme (A2)](https://www.amazon.com/SanDisk-128GB-Extreme-microSD-Adapter/dp/B07FCMBLV6/) <br /> [Samsung Evo Select (A1 / A2)](https://www.amazon.com/SAMSUNG-microSDXC-Expanded-MB-ME128KA-AM/dp/B09B1F9L52/) |
    | USB Cables         | Connect devices (power to main computer, sensor coprocessor to main computer, etc) | [A to Mini B (6 in)](https://www.adafruit.com/product/899) <br /> [A to Micro B (6 in)](https://www.adafruit.com/product/898) <br /> [A to Mini B (3 ft)](https://www.adafruit.com/product/260) <br /> [A to Micro B (3 ft)](https://www.adafruit.com/product/592) |
    | USB OTG Adapter    | Used to connect a USB device to a micro-B OTG host port (eg on Pi Zero).           | [Adafruit (No Cable)](https://www.adafruit.com/product/2910) <br /> [Adafruit (Cable)](https://www.adafruit.com/product/1099) <br /> [Amazon - 1](https://www.amazon.com/UGREEN-Adapter-Samsung-Controller-Smartphone/dp/B00LN3LQKQ/) <br /> [Amazon 2](https://www.amazon.com/BSHTU-Splitter-Enhancer-Straight-90Degree/dp/B07C3JPZBC/) <br /> [Adafruit (Micro B to Micro B)](https://www.adafruit.com/product/3610) |
    | Barrel Jack Connectors | Used to attach barrel jack connectors to jumper wires. Useful for motor battery power input. | [Adafruit (Female)](https://www.adafruit.com/product/368) <br /> [Amazon (Multipack) - 1](https://www.amazon.com/Power-Connector-Female-Adapter-Camera/dp/B07C61434H/) <br /> [Amazon (Multipack) - 2](https://www.amazon.com/DAYKIT-Female-2-1x5-5MM-Adapter-Connector/dp/B01J1WZENK/) |
    | Jumper Wires           | Premade wires with "DuPont" headers. Useful for connecting sensors. Often come in packs includeing male / male wires, female / female wires, and male / female wires. | [Amazon - 1](https://www.amazon.com/EDGELEC-Breadboard-Optional-Assorted-Multicolored/dp/B07GD2BWPY/) <br /> [Amazon -2](https://www.amazon.com/Elegoo-EL-CP-004-Multicolored-Breadboard-arduino/dp/B01EV70C78/) <br /> [Adafruit](https://www.adafruit.com/product/1954) |
    | Wire 28AWG             | Useful for making custom "DuPont" jumper wires. Will need the headers, pins, and crimper (see following entries). Also useful to connect low current motors (eg TT motor). | [Amazon - 1](https://www.amazon.com/Fermerry-Silicone-Stranded-Copper-Electrical/dp/B089CP9N98/) <br /> [Amazon - 2](https://www.amazon.com/StrivedayTM-Flexible-Silicone-electronic-electrics/dp/B01KQ2LHFI/) |
    | "DuPont" Kit           | Includes pins and housings for "DuPont" connectors. Some kits come with a crimper and some wire too. Housings and pins can also be purchased separately. | [Amazon - 1](https://www.amazon.com/Haitronic-310pcs-sockets-Connector-Housing/dp/B0744CV3Y5/) <br /> [Amazon - 2](https://www.amazon.com/HJ-Garden-Connectors-Terminal-1-12Pin/dp/B07BDJ63CP/) |
    | "Dupont" Crimper       | Used to crimp "DuPont" pins to wires.                                          | [Amazon - 1](https://www.amazon.com/IWISS-SN-28B-Crimping-AWG28-18-Dupont/dp/B08D67L3YS/) <br /> [Amazon - 2](https://www.amazon.com/IWISS-SN-28B-Crimping-AWG28-18-Dupont/dp/B00OMM4YUY/) <br /> [Amazon - 3](https://www.amazon.com/Flytuo-Ratcheting-AWG24-16-0-25-1-5mm%C2%B2-Terminals/dp/B08FR38529/) |
    | Male .1 inch Header Pins | Soldered onto many boards to connect wires or breadboard. Often come with boards, but not always. | [Adafruit](https://www.adafruit.com/product/392) <br /> [Amazon - 1](https://www.amazon.com/MCIGICM-Header-2-45mm-Arduino-Connector/dp/B07PKKY8BX/) <br /> [Amazon - 2](https://www.amazon.com/Header-Lystaii-Pin-Connector-Electronic/dp/B06ZZN8L9S/) |
    | Logic Level Shifter    | Used to adjust signals between voltage levels. Necessary to connect some sensors to some boards (eg 5V sensors to 3.3V boards). Note that not all shifters support logic level frequencies (PWM, I2C, SPI, UART, etc) and may only work for lower frequency GPIOs. Parts linked here are bidirectional level shifters. | [Adafruit - 1](https://www.adafruit.com/product/1875) <br /> [Adafruit - 2](https://www.adafruit.com/product/757) <br /> [Amazon - 1](https://www.amazon.com/KeeYees-Channels-Converter-Bi-Directional-Shifter/dp/B07LG646VS/) <br /> [Amazon - 2](https://www.amazon.com/HiLetgo-Channels-Converter-Bi-Directional-3-3V-5V/dp/B07F7W91LC/) |
    | Switches               | Often used to switch motor battery on and off.                                 | [Adafruit - 1](https://www.adafruit.com/product/3221) <br /> [Adafruit - 2](https://www.adafruit.com/product/805) <br /> [Adafruit - 3](https://www.adafruit.com/product/1620) |
    | Mini Breadboard        | Used to connect electronics without soldering. Very small size.                | [Adafruit](https://www.adafruit.com/product/65) <br /> [Amazon - 1](https://www.amazon.com/HiLetgo-SYB-170-Breadboard-Colorful-Plates/dp/B071KCZZ4K/) <br /> [Amazon - 2](https://www.amazon.com/DaFuRui-tie-Points-Solderless-Breadboard-Compatible/dp/B07KGQ7H8B/) |
    | Half Size Breadboard   | Used to connect electronics without soldering. Half of standard size.          | [Adafruit](https://www.adafruit.com/product/64) <br /> [Amazon - 1](https://www.amazon.com/Pcs-MCIGICM-Points-Solderless-Breadboard/dp/B07PCJP9DY/) <br /> [Amazon - 2](https://www.amazon.com/DEYUE-breadboard-Set-Prototype-Board/dp/B07LFD4LT6/) |
    | Full Size Breadboard   | Used to connect electronics without soldering. Standard size.                  | [Adafruit](https://www.adafruit.com/product/239) <br /> [Amazon - 1](https://www.amazon.com/DEYUE-Solderless-Prototype-Breadboard-Points/dp/B07NVWR495/) <br /> [Amazon - 2](https://www.amazon.com/EL-CP-003-Breadboard-Solderless-Distribution-Connecting/dp/B01EV6LJ7G/) |
    | USB / UART Converter   | Useful to connect to main computer by UART (debugging, fixing things, etc). Not usually necessary, but useful to have at times. Often sold as a "Console Cable", having  a blue USB connector housing. DO NOT BUY THIS TYPE OF CABLE FROM AMAZON. THE PL2303 ONES SOLD THERE GENERALLY DO NOT WORK IN RECENT YEARS! Adafruit is the only source that still has a functional version (different chip). The adapters linked here do work and are 3.3V devices (suitable for use with Raspberry Pi boards). | [Adafruit](https://www.adafruit.com/product/954) <br /> [Amazon - 1](https://www.amazon.com/Electronics123-SparkFun-Serial-Basic-Breakout/dp/B07MLXS877/) <br /> [Amazon - 2](https://www.amazon.com/SparkFun-FTDI-Basic-Breakout-3-3V/dp/B079M4F7W5/) <br /> [SparkFun](https://www.sparkfun.com/products/12977) |


??? info "Tools and Assembly Components"
    
    | Item               | Purpose                                                                            | Link(s)    |
    | ------------------ | ---------------------------------------------------------------------------------- | ---------- |
    | Hook and Loop Tape | Commonly wrongly referred to as "Velcro". Useful for attaching electronics to the robot. WARNING: Do not apply this direclty to PCBs. Some adhesives are conductive, corrosive, or may become so over time. As such, a layer of electrical tape should be between any PCB and adhesive on hook and loop tape. | [Amazon -1](https://www.amazon.com/Strenco-Adhesive-Black-Hook-Loop/dp/B00H3R9S1K/) <br /> [Amazon - 2](https://www.amazon.com/Denser-Sticky-Adhesive-Fastener-Strips/dp/B07N3B36Z2/) |
    | Twist Ties         | Useful for cable management. Plastic ones hold up better than paper ones.          | [Amazon - 1](https://www.amazon.com/Twist-Plastic-Reusable-Household-Office/dp/B09GFHPJBZ/) <br /> [Amazon - 2](https://www.amazon.com/Flixall-Plastic-Twist-Ties-Reusable/dp/B08CS2LZN1/) |
    | Zip Ties           | Useful for cable management or attaching components.                               | [Amazon - 1](https://www.amazon.com/HAVE-ME-TD-Cable-Ties/dp/B08TVLYB3Q/) <br /> [Amazon - 2](https://www.amazon.com/Self-Locking-CableTies-Multi-Purpose-Management-Workshop-White/dp/B097M825XG/) |
    | Hot Glue + Glue Gun | Hot glue is useful for attaching components. It is also not conductive or corrosive so it can be placed directly on PCBs. | [Amazon - 1](https://www.amazon.com/Ad-Tech-Mini-Hi-Temp-Combo-White/dp/B01FIJPPN4/) <br /> [Amazon - 2](https://www.amazon.com/Assark-Sticks-School-Repairs-20W/dp/B09FYWQ44L/) <br /> [Amazon - 3](https://www.amazon.com/Gorilla-8401509-Hot-Glue-Sticks/dp/B07K791YRP/) |
    | Super Glue         | Useful for attaching parts when hot glue is not strong enough. Comes in gel and liquid forms. The gel is easier to use, but often more expensive. | [Amazon (Gel) - 1](https://www.amazon.com/Gorilla-Super-Glue-gram-Clear/dp/B082XGL21J/) <br /> [Amazon (Liquid) - 1](https://www.amazon.com/Gorilla-Super-Glue-Gram-Clear/dp/B00KPYB05A/) <br /> [Amazon (Gel) - 2](https://www.amazon.com/Loctite-Control-4-Gram-Bottle-1739050/dp/B00ELV2D0Y/) <br /> [Amazon (Liquid) - 2](https://www.amazon.com/Loctite-Liquid-Professional-20-Gram-1365882/dp/B004Y960MU/) |
    | Electrical Tape    | Useful for cable management and anything where adhesives need to contact electronics. | [Amazon - 1](https://www.amazon.com/Scotch-Electrical-Tape-4-Inch-66-Foot/dp/B001ULCB1O/) <br /> [Amazon - 2](https://www.amazon.com/AmazonCommercial-Electrical-4-inch-60-feet-6-Pack/dp/B07YDRY8ZS/) |
    | Ruler              | Useful for measuring for placements. Not for precision measurements.               | [Amazon - 1](https://www.amazon.com/Westcott-Clear-Flexible-Acrylic-Metric/dp/B004E3NK92/) <br /> [Amazon - 2](https://www.amazon.com/Inches-Plastic-Straight-Measuring-Student/dp/B07N777XWH/) |
    | Small Screwdriver Kit | Useful for working with small screws found in screw terminals and other parts.  | [Amazon - 1](https://www.amazon.com/Screwdriver-Eyeglass-Precision-Different-Screwdrivers/dp/B07YJG766F/) <br /> [Amazon - 2](https://www.amazon.com/XOOL-Precision-Screwdriver-Extension-Smartphone/dp/B086SQZGLJ/) <br /> [Amazon - 3](https://www.amazon.com/WORKPRO-10-Piece-Precision-Screwdriver-Phillips/dp/B091C32HLX/) |
    | Solder Iron        | Used to attach electronics, install headers on PCBs, etc. Sometimes come in kits. Solder is also required. Note: Some of the kits linked here are cheap. They should work for simple jobs, but won't work as well as a slightly more expensive option. Also note that these cheaper kits often include lead solder. Lead solder is easier to work with than leadless (and requires lower temperatures), but it is lead. | [Amazon (Cheap Kit) - 1](https://www.amazon.com/Soldering-soldering-solder-adjustable-temperature/dp/B09DY7CCW5/) <br /> [Amazon (Cheap Kit) - 2](https://www.amazon.com/Soldering-Ceramic-Adjustable-Temperature-Repairing/dp/B09DSDBC7G/) <br /> [Adafruit - Iron](https://www.adafruit.com/product/4695) <br /> [Adafruit - Lead Solder](https://www.adafruit.com/product/1886) <br /> [Adafruit - Leadless Solder](https://www.adafruit.com/product/2473) |
    | Multimeter         | Useful for debugging electrical problems. Also just a useful tool to have around.  | [Amazon - 1](https://www.amazon.com/AstroAI-Digital-Multimeter-Voltage-Tester/dp/B01ISAMUA6/) <br /> [Amazon - 2](https://www.amazon.com/AmazonCommercial-Manual-Ranging-Digital-Multimeter/dp/B07VY41YHM/) <br /> [Amazon - 3](https://www.amazon.com/DT830B-Digital-Voltmeter-Ammeter-Multimeter/dp/B005KGCI0Y/) |
    | Wire Stripper      | Used to remove insulation from wires.                                              | [Amazon - 1](https://www.amazon.com/VISE-GRIP-Stripping-Cutter-8-Inch-2078309/dp/B000JNNWQ2/) <br /> [Amazon - 2](https://www.amazon.com/WGGE-Professional-crimping-Multi-Tool-Multi-Function/dp/B073YG65N2/) <br /> [Amazon - 3](https://www.amazon.com/DOWELL-Stripper-Multi-Function-Tool%EF%BC%8CProfessional-Craftsmanship/dp/B06X9875Z7/) |