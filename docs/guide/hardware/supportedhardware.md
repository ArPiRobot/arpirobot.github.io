
## Where to Buy Components

Note: These are not the only possible places to buy components. These are just general recommendations.

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

    TODO: Metal TT Motor options from Adafruit / Others

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

Currently, only Raspberry Pi boards are officially supported. This is due to the OS image used. Other boards could be used, but would require custom configuring the operating system (which is not a trivial process). Additionally, a board with a WiFi adapter is required for full functionality.

??? info "Raspberry Pi Boards"

    There are three main "models" of raspberry Pi boards.

    - Model B: Traditional size with multiple USB ports, ethernet, more display options, etc. Slightly larger than a hat.
    - Model A: Same size as a hat (smaller than model B). Has one USB port and no ethernet. Typically has same processor, but less ram than corresponding model B.
    - Zero series: A smaller raspberry pi board with reduced size connectors. Uses micro USB OTG instead of a USB A port. Often weaker processor than similar aged model B and A boards and less RAM than model B boards.

    In addition to the "models" listed above, there are different generations of each. The Model A and B typically share generation numbers. The Zero series has its own generation numbers. There is not a Pi of each model for each generation. The generation numbers used below are for the model A and B boards. The Pi zero is associated with a generation based on release date. Additionally, there are sometimes "+" models released (eg Model B+, Model A+). Typically, this is just due to a different SoC (processor) being used. Not much tends to change with the board functionally.

    | Generation         | Model B Board              | Model A Board              | Zero series Board                       |
    | ------------------ | -------------------------- | -------------------------- | --------------------------------------- |
    | 1                  | Raspberry Pi 1 Model B (+) | Raspberry Pi 1 Model A (+) |                                         |
    | 2                  | Raspberry Pi 2 Model B     |                            |                                         |
    | 3                  | Raspberry Pi 3 Model B (+) | Raspberry Pi 3A+           | Raspberry Pi Zero / Raspberry Pi Zero W |
    | 4                  | Raspberry Pi 4 Model B     |                            | Raspberry Pi Zero 2 W                   |
    
    Things to consider when choosing a board: 

    - At this time, 32-bit arm board (armv6 and armv7) are supported, but at a later date only 64-bit arm (armv8+aarch64) boards may be supported. As such, these boards are generally recommended. 
    - For more complex robots more RAM and CPU cores are useful. Avoiding single core boards with less than 1GB RAM is a good idea if you plan to build a more complex robot (more concurrent tasks, image processing, etc). However, less powerful boards are often ok for simpler robots.
    - Only boards with builtin WiFi are recommended. While other boards can be used with a USB WiFi adapter, this is not a tested / well supported configuration.
    - Typically model A and Zero series boards are recommended for robots due to the reduced size. Often, additional connectors on the larger model B boards are not necessary for robots.

    The following boards are generally recommended
    
    - [Raspberry Pi 3 Model A+](https://www.raspberrypi.com/products/raspberry-pi-3-model-a-plus/): Quad core (aarch64 capable) with 512MB RAM. Good for many mid range robots.
    - [Raspberry Pi 3 Model B+](https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/): Quad core (aarch64 capable) with 1G RAM. Good for scenarios where a little more RAM is needed than a 3A+ has. [Model B](https://www.raspberrypi.com/products/raspberry-pi-3-model-b/) is also about the same.
    - [Raspberry Pi 4 Model B](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/): Quad core (aarch64 capable) and more powerful than a 3B+. Also has options with much more RAM (up to 8GB). However, these "overkill" for many robots. Cooling is also sometimes something that needs to be considered.
    - [Raspberry Pi Zero 2 W](https://www.raspberrypi.com/products/raspberry-pi-zero-2-w/): Quad core (aarch64 capable) with 512MB RAM. Small size and low cost. Good for low to mid range robots.
    - [Raspberry Pi Zero W](https://www.raspberrypi.com/products/raspberry-pi-zero-w/): Single core (**not** aarch64 capable) with 512MB RAM. Small size and low cost. Advisable only if you cannot get a Pi Zero 2 W and only for simple robots.

    The following boards are generally **not** recommended:

    - Raspberry Pi 1 Model B or Model A: No WiFi and only a 26-pin header (not compatible with most hats).
    - Raspberry Pi 1 Model B+ or A+: No WiFi and single core (not aarch64 capable)
    - Raspberry Pi 2 Model B: No WiFi and not aarch64 capable
    - Raspberry Pi Zero: No WiFi (W variant has WiFi).

    For more information see [Wikipedia's page](https://en.wikipedia.org/wiki/Raspberry_Pi#Series_and_generations)


## Computer Power Sources

Generally, it is easiest to have two power sources on the robot. This avoids scenarios where motors changing speed cause a voltage drop that reboots the computer. Often, the computer power source is a USB battery pack.

When selecting a power source, be aware that it needs to be able to supply a sufficient amount of current. The current requirement mostly depends on the main computer. In general, using a 2.4A power source is a safe option (3A is better for a Pi 4B) and is generally recommended. Depending on how many sensors and other devices are connected, lower currents may be required. The following are minimum recommendations by each computer. These minimums are lower than commonly recommended due to the fact that the computer is generally not driving a display when used on a robot.
- 2.1A for Pi 3A+, Pi 3B+, and Pi 3B
- 2.4A for Pi 4B
- 1.0A for Pi Zero W
- 1.5A for Pi Zero 2 W

??? info "USB Battery Packs"
    | Battery Pack            | Description             | Current Capacity    | Link(s)      |
    | ----------------------- | ----------------------- | ------------------- | ------------ |
    
    TODO

??? info "Other Options"
    TODO: Regulator and battery hats

## Tools

TODO