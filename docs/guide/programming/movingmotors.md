# Moving Motors

One of the most important devices on a robot are the motors. Controlling the motors allows your robot to perform many tasks, one of the most basic and essential of which is driving. This section will cover different motor controller options and writing code to control the motors.


## Motor Controllers

Many different motor controllers are supported by the ArPiRobot framework. Each one requires slightly different code to use. The ArPiRobot framework is object oriented, meaning your program will have one object for each motor on your robot, and sometimes one for the motor controller itself. The objects for the motors all have the same set of functions, but must be created differently based on the motor controller in use. This section covers creating the objects for the motors depending on which motor controller you use on your robot. The following section shows how to use the motor objects. Use of the motor objects is the same regardless of which motor type. In this section, you need only add the code for the motor controller you use on your robot.


### DRV8833 Motor Controller

![](../../img/drv8833.png){: style="height: 300px"}

*Image Credit: [Adafruit](https://www.adafruit.com/product/3297)*

TODO: Describe motor controller use and what pins are needed
TODO: Describe motor controller object and getting motor objects


### TB6612 Motor Controller

![](../../img/tb6612.png){: style="height: 300px"}

*Image Credit: [Adafruit](https://www.adafruit.com/product/2448)*


### L298N Motor Controller

![](../../img/l298n.png){: style="height: 300px"}

*Image Credit: [Link](https://www.instructables.com/L298N-MOTOR-DRIVER-MODULE/)*

TODO: Code & Explanation


### Adafruit Motor Hat / Bonnet

*Note: The Geekworm motor hat uses the same code as the Adafruit Motor Hat.*

![](../../img/motorhat.png){: style="height: 300px"}

*Image Credit: [Adafruit](https://www.adafruit.com/product/2348)*

TODO: Code & Explanation


## Moving the Motors

TODO: Note that only works if robot enabled


### Motor Speed & Direction

TODO: Explain inverting directions both in hardware and software. Explain process for getting all motors to spin such that positive is forward.


### Brake Mode & Coast Mode

TODO: Default to coast. Show code to put in brake mode and show effects.


