
Unlike the physical construction of the robot, the requirements for a robot's electrical system are often very similar. While there will be minor differences depending on specifically what a robot does, the main types of components needed often do not change.


## Major Components

- Main Computer: The main computer is what runs the robot software. This is an embedded computer running a Linux operating system. Often, Raspberry Pi boards are used.
- Motor Power Source: The power source used to power motors. Often this is a set of AA batteries, or a rechargeable battery pack.
- Computer Power Source: The power source used to power the computer (and other devices powered from the computer). In many cases, this is a second battery in the form of a USB battery pack (rechargeable). However, this can also sometimes be regulated from the same source powering motors.
- Motion controllers: These components are used to control the robot's motion. This includes devices such as motor controllers used to move motors on the robot.
- Sensor Coprocessor: A microcontroller (or microcontroller dev board) capable of interfacing with sensors on the robot. Communicates important information back to the main computer. Often, Arduino development boards are used.
- Sensors: Any device on the robot used to provide the software information about the robot or its environment. Includes encoders, IMUs (gyro, accelerometer, magnetometer), voltage sensors, ultrasonic sensor, IR reflection detectors, etc
- Indicators: Any device used to convey information to the robot's user. Typically, these are just LEDs.


## Example Block Diagram

The following is a block diagram of an electrical system for a 4 wheel drive robot including several sensors.


