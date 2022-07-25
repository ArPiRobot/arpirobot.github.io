
## Mini Clipboard Robot

A four wheel differential drive robot build on a small clipboard (6" by 9"). This robot build uses common "TT" motors with a 1:48 gear ratio and does not require any soldering or 3D printed parts to assemble. This is intended to be a low-cost easy to build reference robot design for beginners. It includes various sensors such as encoders, an IMU, an ultrasonic sensor, a voltage monitor, and IR reflector sensors for line following.

*This is the robot example code in the [ArPiRobot-Examples](https://github.com/ArPiRobot/ArPiRobot-Examples/) repository is designed for.*

![](../../img/4wd_mini_render.png)


??? info "Parts List"

    **Mechanical Parts:**

    - 1x [6"x9" Mini Clipboard with Low Profile Clip]()
    - 4x [TT Motor with Wires and Wheels]()
        - Alternate: [Motors with Wires]()
        - Alternate: [Wheels]()
    - 1x [Pack of Bingo Chips]()
    - 2x [TT Motor Encoder Disk]()

    **Electrical Parts:**

    - 1x [Raspberry Pi Zero W w/ Headers](https://www.adafruit.com/product/5291)
        - Alternate: [Raspberry Pi Zero 2 W](https://www.adafruit.com/product/3708) *no headers, must buy and solder separately*
        - Alternate: [Raspberry Pi 3A+](https://www.adafruit.com/product/4027)
    - 1x [Arduino Nano](https://www.amazon.com/Arduino-A000005-ARDUINO-Nano/dp/B0097AU5OU/)
        - Alternate: [Arduino Nano Every](https://www.amazon.com/Arduino-Nano-Every-Single-Board/dp/B07VX7MX27/)
        - Alternate: [Metro Mini 328](https://www.adafruit.com/product/2590)
    - 1x [MPU-6050 IMU Breakout](https://www.adafruit.com/product/3886)
    - 1x [Voltage Monitor Module](https://www.amazon.com/Youliang-Voltage-Detection-DC0-25v-Arduino/dp/B07TV3N46V/)
        - [Alternate](https://www.amazon.com/HiLetgo-Voltage-Detection-Arduino-Electronic/dp/B01HTC4XKY/)
    - 1x [HC-SR04 Ultrasonic Sensor](https://www.adafruit.com/product/3942)
        - [Alternate](https://www.amazon.com/Ultrasonic-HC-SR04-Distance-Measuring-Transducer/dp/B077P72HG7/)
        - [Alternate](https://www.adafruit.com/product/4007)
    - 2x [IR Reflector Module](https://www.amazon.com/HiLetgo-Infrared-Avoidance-Reflective-Photoelectric/dp/B07W97H2WS/)
        - [Alternate](https://www.amazon.com/Infrared-Obstacle-Avoidance-Sensor-Arduino/dp/B07T91JXHW/)
    - 2x [Photo-Interrupter Module](https://www.amazon.com/Willwin-Measuring-Comparator-Optocoupler-Arduino/dp/B0776RHKB1/)
        - [Alternate](https://www.amazon.com/DAOKI-Optocoupler-Sensor-Module-Arduino/dp/B01MRELRS1)
    - 1x [5AA Holder](https://www.adafruit.com/product/3456)
        - [Alternate](https://www.amazon.com/LampVPath-Battery-Holder-Leads-Wires/dp/B07WRQ44YK/) plus male [screw terminal to barrel jack adapter](https://www.adafruit.com/product/369)
    - 1x [Mini Breadboard](https://www.adafruit.com/product/65)
    - 1x [Compact USB Battery Pack](https://www.amazon.com/Anker-Upgraded-Candy-Bar-High-Speed-Technology/dp/B0744HYN4M/)
        - [Alternate](https://www.amazon.com/Miady-Portable-Charger-5000mAh-Lightweight/dp/B083VRD7CX/)
    - 1x [Pack of Jumper Wires](https://www.amazon.com/EDGELEC-Breadboard-Optional-Assorted-Multicolored/dp/B07GD2BWPY/)
        - [Alternate](https://www.amazon.com/Elegoo-EL-CP-004-Multicolored-Breadboard-arduino/dp/B01EV70C78/)
    - 1x Short USB Cable for Arudino
        - [A to Mini B](https://www.adafruit.com/product/899) (for Arduino Nano & Clones)
        - [A to Micro B](https://www.adafruit.com/product/898) (for Arduino Nano Every & Metro Mini 328)
    - 1x [USB OTG Adapter for Pi Zero](https://www.adafruit.com/product/2910)
        - [Alternate](https://www.adafruit.com/product/1099)
    - 1x [Female Barrel Jack screw terminal adapter](https://www.adafruit.com/product/368)
    - 1x [Cable for MPU-6050 Breakout](https://www.adafruit.com/product/4209)

??? info "Assembly Instructions"

    1. Motor Assemblies

        - The motor assembly consists of the motor, wheels, bingo chip spacers, and encoder disks.

        - The outside face of the motor is the face with an axel that has a "bump" on it. The front of the motor is the face opposite the actual motor.

        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](../../img/4wdmini_motor_faces.png){: style="height:150px;"}

        - Bingo chips are used to construct a spacer (approx 0.25 in thick) to space the motor away from the clipboard it will eventually be mounted on. This is typically between 4 and 6 bingo chips.

        - Bingo chip spacers should be constructed using super glue. Glue a stack of bingo chips to each other to result in a 0.25 in tall stack. Eight of these stacks are necessary in total.

        - Two motors of each of two orientations are required. The first orientation requires bingo chips on the "top" face (as shown in the image above). Two require bingo chips on the bottom face. If the motors are arranged in the layout shown below, all bingo chip stacks go on the top faces.

        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](../../img/4wdmini_bingo_chip_sides.png){: style="height:300px;"}

        - To attach the bingo chip stacks to the motor do **not** use super glue. Instead use hot glue. Ideally, clean each surface using isopropyl alcohol before applying hot glue (let it dry before applying glue) to ensure best adhesion. Use a decent amount of glue to ensure the motor remains attached to the bingo chips. Hot glue is used as isopropyl alcohol can be used later to easily remove it allowing the motor to be opened or replaced easily. *When placing the bingo chips, be careful with alignment. They will be slightly wider than the motor's face. Make sure the inside edge is flush. The bingo chips can overhang the outside edge slightly.*

        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](../../img/4wdmini_chip_placmenet.png){: style="height:150px;"}

        - Once all assembled, the motors can be arranged as follows. The clipboard is then essentially placed on top.

        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](../../img/4wdmini_motors_with_chips.png){: style="height:300px;"}

        - However, it is easier to place the motors on the clipboard one at a time. Turn the clipboard clip-side up and place each motor at the distances shown below. The outside face of each motor should be flush with the edge of the clipboard to ensure proper movement. The motors should be attached to the clipboard using hot glue. It is again recommended to clean the bingo chip faces before applying hot glue. Super glue is not recommended as it is difficult to reposition if you mess up alignment.

        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](../../img/4wdmini_motor_on_board.png){: style="height:300px;"}

        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](../../img/4wdmini_motor_on_board_side.png){: style="height:150px;"}

        - Finally, add the wheels to all motors (outside shaft) and add an encoder disk to the inside shaft of the front two motors (front is side without the clip).

        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](../../img/4wdmini_with_wheels.png){: style="height:300px;"}

        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![](../../img/4wdmini_encoder_disks.png){: style="height:300px;"}

    2. Electronics
    

