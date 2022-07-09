## Using Sensors instead of Time

The previous section of this guide implemented `ActionSeries` that drove two shapes, a square and a triangle, using time-based drive actions. In reality, this time-based method of driving a distance or rotating to a certain angle is not ideal. There are many things that affect the relationship between time in motion and distance / angle reached including, battery level, the surface being driven on, and what robot is running the code. These factors make driving shapes using time, difficult and inconsistent.

To improve this, sensor data will be used instead. Two types of sensors will be used in this section. First, a gyroscope will be used to determine what angle the robot is at relative to where it started. Second, encoders will be used to determine how many times the wheels have rotated, which can be used to determine how far the robot has driven.

Code to use an `Mpu6050` sensor's IMU via an Arduino coprocessor was shown in the [Sensor Data & NetworkTable](./sensordata.md) section of this guide. The following code can be added to use encoders (assumed to be single channel encoders, not quadrature encoders)

=== "Python (`robot.py`)"
    ```py
    # Add with other imports
    from arpirobot.arduino.sensor import SingleEncoder

    # Create the devices in __init__
    # Assuming encoders are connected to pins 2 and 3 on the Arduino
    # Change if this is not the case
    self.lenc = SingleEncoder(2)
    self.renc = SingleEncoder(3)

    # In robot_started, before self.arduino.begin()
    self.arduino.add_device(self.lenc)
    self.arduino.add_device(self.renc)
    ```

=== "C++ (`robot.hpp`)"
    ```cpp
    // Add with other includes
    #include <arpirobot/arduino/sensor/SingleEncoder.hpp>

    // Create devices with other member variables
    // Assuming encoders are connected to pins 2 and 3 on the Arduino
    // Change if this is not the case
    SingleEncoder lenc {2};
    SingleEncoder renc {3};
    ```

=== "C++ (`robot.cpp`)"
    ```cpp
    // In robotStarted, before arduino.begin()
    arduino.addDevice(lenc);
    arduino.addDevice(renc);
    ```


### Rotate Angle Action

The following code implements an action that rotates a certain number of degrees from where it starts. This is done using the Z-Axis gyroscope on the robot.

=== "Python (`actions.py`)"
    ```py
    class RotateAngleAction(Action):
        def __init__(self, degrees: float):
            super().__init__()
            self.degrees = degrees
            self.target_angle = 0       # This will be set in begin()
        
        def locked_devices(self) -> LockedDeviceList:
            # This needs to be the only action using motors while running
            return [ main.robot.flmotor, main.robot.frmotor, 
                main.robot.rlmotor, main.robot.rrmotor ]

        def begin(self):
            # Calculate target angle
            # Action should rotate self.degrees degrees from where robot is initially facing
            self.target_angle = main.robot.imu.get_gyro_z() + self.degrees

            # Start rotating in the correct direction
            # Note: it is assumed that positive rotation speed rotates the positive direction on the IMU
            # If the robot rotates "forever" swap the positive and negative rotation directions
            if self.degrees >= 0:
                main.robot.drive_helper.update(0, 0.9)
            else:
                main.robot.drive_helper.update(0, -0.9)
        
        def process(self):
            # Nothing to do here. Just let it keep rotating.
            pass
        
        def finish(self, was_interrupted: bool):
            # Stop rotating
            main.robot.drive_helper.update(0, 0)
        
        def should_continue(self) -> bool:
            if self.degrees >= 0:
                # Rotating in positive direction
                # This means that the angle will be increasing
                # Done rotating when angle is at greater than or equal to target
                # If not done, should continue running (continue if less than target)
                return main.robot.imu.get_gyro_z() < self.target_angle
            else:
                # Rotating in negative direction
                # This means that the angle will be decreasing
                # Done rotating when angle is less than or equal to target
                # If not done, should continue running (continue if greater than target)
                return main.robot.imu.get_gyro_z() > self.target_angle
    ```

When the action starts, it calculates the target angle (the angle it should go to) which is a sum of the angle the robot starts at and the angle the action is supposed to rotate (passed as a constructor argument). The robot continues rotating at a constant speed until it reaches (or passes) this angle. The `shouldContinue` / `should_continue` function is used to stop the action once it has rotated far enough. Note that negative rotation is simply rotating in the opposite direction. As such, it is necessary to know which way the robot is rotating to determine if it has rotated far enough (hence the if statement in `shouldContinue` / `should_continue`).

### Drive Distance Action

TODO: Add support for encoders in robot program

TODO: Use encoders + brake mode to drive until a distance threshold has passed


### Using the Actions

TODO: Implement driving a square and triangle on button presses using automated routines


### Limitations

TODO: Explain limitations of this method. Drive distance can go too far. It also doesn't use the IMU to ensure a straight line. Rotate angle can overshoot and does not correct. Rotating to an angle is actually a perfect application for a PID loop, shown in the next section of the guide.
