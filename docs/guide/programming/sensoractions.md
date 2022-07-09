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

            # Use brake mode to reduce how much the robot continues rotating after the action stops
            main.robot.flmotor.set_brake_mode(True)
            main.robot.frmotor.set_brake_mode(True)
            main.robot.rlmotor.set_brake_mode(True)
            main.robot.rrmotor.set_brake_mode(True)

            # Start rotating in the correct direction
            # Note: It is assumed that positive rotation in the drive helper 
            #       results in positive change in IMU angle. If this is not the
            #       case, swap the +0.9 and -0.9 in the if / else below.
            #       If these signs are incorrect, the robot will rotate forever
            #       as it never reaches its target angle.
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

=== "C++ (`robot.hpp`)"
    ```cpp
    class RotateAngleAction : public Action{
    public:
        RotateAngleAction(double degrees);

    protected:
        LockedDeviceList lockedDevices() override;
        void begin() override;
        void process() override;
        void finish(bool wasInterrupted) override;
        bool shouldContinue() override;
    
    private:
        double degrees;
        double targetAngle = 0;     // Will be set in begin()
    };
    ```

=== "C++ (`robot.cpp`)"
    ```cpp
    RotateAngleAction::RotateAngleAction(double degrees) : degrees(degrees){

    }

    LockedDeviceList RotateAngleAction::lockedDevices(){
        // This needs to be the only action using motors while running
        return { Main::robot->flmotor, Main::robot->frmotor, 
            Main::robot->rlmotor, Main::robot->rrmotor };
    }

    void RotateAngleAction::begin(){
        // Calculate target angle
        // Action should rotate this->degrees degrees from where the robot is initially facing
        targetAngle = degrees + Main::robot->imu.getGyroZ();

        // Use brake mode to reduce how much the robot continues rotating after the action stops
        Main::robot->flmotor.setBrakeMode(true);
        Main::robot->frmotor.setBrakeMode(true);
        Main::robot->rlmotor.setBrakeMode(true);
        Main::robot->rrmotor.setBrakeMode(true);

        // Start rotating in the correct direction
        // Note: It is assumed that positive rotation in the drive helper 
        //       results in positive change in IMU angle. If this is not the
        //       case, swap the +0.9 and -0.9 in the if / else below.
        //       If these signs are incorrect, the robot will rotate forever
        //       as it never reaches its target angle.
        if(degrees >= 0){
            Main::robot->driveHelper.update(0, 0.9);
        }else{
            Main::robot->driveHelper.update(0, -0.9);
        }
    }

    void RotateAngleAction::process(){
        // Nothing to do here. Just let it keep rotating.
    }

    void RotateAngleAction::finish(bool wasInterrupted){
        // Stop rotating
        Main::robot->driveHelper.update(0, 0);
    }

    bool RotateAngleAction::shouldContinue(){
        if(degrees >= 0){
            // Rotating in positive direction
            // This means that the angle will be increasing
            // Done rotating when angle is at greater than or equal to target
            // If not done, should continue running (continue if less than target)
            return Main::robot->imu.getGyroZ() < targetAngle;
        }else{
            // Rotating in negative direction
            // This means that the angle will be decreasing
            // Done rotating when angle is less than or equal to target
            // If not done, should continue running (continue if greater than target)
            return Main::robot->imu.getGyroZ() > targetAngle;
        }
    }
    ```

When the action starts, it calculates the target angle (the angle it should go to) which is a sum of the angle the robot starts at and the angle the action is supposed to rotate (passed as a constructor argument). The robot continues rotating at a constant speed until it reaches (or passes) this angle. The `should_continue` / `shouldContinue` function is used to stop the action once it has rotated far enough. Note that negative rotation is simply rotating in the opposite direction. As such, it is necessary to know which way the robot is rotating to determine if it has rotated far enough (hence the if statement in `should_continue` / `shouldContinue`).

### Drive Distance Action

A drive distance action can be implemented similarly. Since `SingleEncoders` are used, it is not possible to determine the direction the robot is moving using encoders, only how far it has moved (the encoder count will always be positive). In other words, rotating the wheel one revolution forward increases the encoder count and rotating the wheel one revolution backward increases the encoder count (no rotation decreases the encoder count). As such, to move in the negative direction, the motors move in reverse (negative speed), but the encoder count still grows. The action below is implemented to handle this. Note that if your robot has Quadrature encoders, this action will not work without modifications. You could either adapt this action to handle directions (similar to `RotateAngleAction` above), or use `SingleEncoder` objects in code instead of `QuadEncoder` objects (just pick one signal wire per encoder).

=== "Python (`robot.py`)"
    ```py
    class DriveDistanceAction(Action):
        def __init__(self, distance_ticks: int):
            super().__init__()

            # Absolute value of distance_ticks = distance to travel with no sign
            self.distance_ticks = abs(distance_ticks)

            # True if robot is moving forward (positive direction). Otherwise False.
            self.forward = (distance_ticks >= 0)
        
        def locked_devices(self) -> LockedDeviceList:
            # This needs to be the only action using motors while running
            return [ main.robot.flmotor, main.robot.frmotor, 
                main.robot.rlmotor, main.robot.rrmotor ]

        def begin(self):
            # Reset encoder counts to zero (starting from zero ticks)
            main.robot.lenc.set_position(0)
            main.robot.renc.set_position(0)

            # Use brake mode to reduce how much the robot continues rotating after the action stops
            main.robot.flmotor.set_brake_mode(True)
            main.robot.frmotor.set_brake_mode(True)
            main.robot.rlmotor.set_brake_mode(True)
            main.robot.rrmotor.set_brake_mode(True)

            # Move the robot in the correct direction
            if(self.forward):
                main.robot.drive_helper.update(0.8, 0)
            else:
                main.robot.drive_helper.update(-0.8, 0)

        
        def process(self):
            # Nothing to do here. Just let it keep driving.
            pass
        
        def finish(self, was_interrupted: bool):
            # Stop driving
            main.robot.drive_helper.update(0, 0)
        
        def should_continue(self) -> bool:
            # Average the distance traveled by left and right encoders
            # Treat this as the distance traveled
            distance_traveled = (main.robot.lenc.get_position() + main.robot.renc.get_position()) / 2.0

            # Continue if robot has not traveled far enough (distance traveled is less than target distance)
            # Target distance = self.distance_ticks since initial position is always zero
            return distance_traveled < self.distance_ticks
    ```

=== "C++ (`robot.hpp`)"
    ```cpp
    class DriveDistanceAction : pubic Action{
    public:
        DriveDistanceAction(int distanceTicks);
    
    protected:
        LockedDeviceList lockedDevices() override;
        void begin() override;
        void process() override;
        void finish(bool wasInterrupted) override;
        bool shouldContinue() override;
    
    private:

        // Set in constructor initializer list.
        // Always positive number = distance to travel without sign
        int distanceTicks;

        // Set in constructor initializer list
        // true if robot is moving forward (positive direction). Otherwise false.
        bool forward;
    };
    ```

=== "C++ (`robot.cpp`)"
    ```cpp
    DriveDistanceAction::DriveDistanceAction(int distanceTicks) :
            distanceTicks(std::abs(distanceTicks)), forward(distanceTicks >= 0){
        
    }

    LockedDeviceList DriveDistanceAction::lockedDevices(){
        // This needs to be the only action using motors while running
        return { Main::robot->flmotor, Main::robot->frmotor, 
            Main::robot->rlmotor, Main::robot->rrmotor };
    }

    void DriveDistanceAction::begin(){
        // Reset encoder counts to zero (starting from zero ticks)
        Main::robot->lenc.setPosition(0);
        Main::robot->renc.setPosition(0);
        
        // Use brake mode to reduce how much the robot continues rotating after the action stops
        Main::robot->flmotor.setBrakeMode(true);
        Main::robot->frmotor.setBrakeMode(true);
        Main::robot->rlmotor.setBrakeMode(true);
        Main::robot->rrmotor.setBrakeMode(true);

        // Move the robot in the correct direction
        if(forward){
            Main::robot->driveHelper.update(0.8, 0);
        }else{
            Main::robot->driveHelper.update(-0.8, 0);
        }
    }

    void DriveDistanceAction::process(){
        // Nothing to do here. Just let it keep driving.
    }

    void DriveDistanceAction::finish(bool wasInterrupted){
        // Stop driving
        Main::robot->driveHelper.update(0, 0);
    }

    bool DriveDistanceAction::shouldContinue(){
        // Average the distance traveled by left and right encoders
        // Treat this as distance traveled
        int distanceTraveled = (Main::robot->lenc.getPosition() + Main::robot->renc.getPosition()) / 2.0;

        // Continue if robot has not traveled far enough (distance traveled is less than target distance)
        // Target distance = this->distanceTicks since initial position is always zero
        return distanceTraveled < distanceTicks;
    }
    ```

When the action starts, it resets encoder counts to zero. This makes detecting when the robot has driven far enough easy, as both encoders always start from zero. The robot will then begin driving (forward if distance given to the action in the constructor was positive, reverse if negative). The robot continues driving while the action is running. The `should_continue` / `shouldContinue` function is used to determine if the robot has driven far enough. If so, the function returns false stopping the action. The function uses both encoder values by averaging them to determine how far the robot has driven.


### Using the Actions

<!--The main challenge with this action is converting a distance in inches to a number of encoder "ticks". An encoder "tick" is one count from the encoder. `SingleEncoder`s count rising and falling edges, so when using a [standard 20-slot disk](https://www.adafruit.com/product/3782), there are 40 "ticks" per wheel revolution. Converting wheel revolutions to linear distance traveled requires the wheel radius. If using [standard black and yellow TT motor wheels](https://www.onemakergroup.com/store/products/tt-motor-wheels) the radius is 2.5 inches. The following equation is used to calculate number of ticks from linear distance

`ticks` = `distance_inches` / `wheel_radius` * `ticks_per_revolution`-->


TODO: Implement driving a square and triangle on button presses using automated routines


### Limitations

TODO: Explain limitations of this method. Drive distance can go too far. It also doesn't use the IMU to ensure a straight line. Rotate angle can overshoot and does not correct. Rotating to an angle is actually a perfect application for a PID loop, shown in the next section of the guide.
