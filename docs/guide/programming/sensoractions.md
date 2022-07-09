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

When the action starts, it calculates the target angle (the angle it should go to) which is a sum of the angle the robot starts at and the angle the action is supposed to rotate (passed as a constructor argument). The robot continues rotating at a constant speed until it reaches (or passes) this angle. The `shouldContinue` / `should_continue` function is used to stop the action once it has rotated far enough. Note that negative rotation is simply rotating in the opposite direction. As such, it is necessary to know which way the robot is rotating to determine if it has rotated far enough (hence the if statement in `shouldContinue` / `should_continue`).

### Drive Distance Action

A drive distance action can be implemented similarly. Since `SingleEncoders` are used, it is not possible to determine the direction the robot is moving using encoders, only how far it has moved. However, it is reasonable to assume that if a positive power is applied the robot is moving forward and if a negative power is applied, the robot is moving backward. This assumption can be made since this is how motor directions were setup previously (see [Moving Motors](./movingmotors.md)). As such, if a positive distance is passed in, the action will drive forward (positive power), otherwise it will drive in reverse.

TODO: Code

This action functions similarly to the `RotateAngleAction` previously implemented. When it starts, it calculates a target distance to travel. Then, the robot starts moving. In `shouldContinue` / `should_continue` it is checked if the robot has driven far enough. If so, the action stops.


### Using the Actions

<!--The main challenge with this action is converting a distance in inches to a number of encoder "ticks". An encoder "tick" is one count from the encoder. `SingleEncoder`s count rising and falling edges, so when using a [standard 20-slot disk](https://www.adafruit.com/product/3782), there are 40 "ticks" per wheel revolution. Converting wheel revolutions to linear distance traveled requires the wheel radius. If using [standard black and yellow TT motor wheels](https://www.onemakergroup.com/store/products/tt-motor-wheels) the radius is 2.5 inches. The following equation is used to calculate number of ticks from linear distance

`ticks` = `distance_inches` / `wheel_radius` * `ticks_per_revolution`-->


TODO: Implement driving a square and triangle on button presses using automated routines


### Limitations

TODO: Explain limitations of this method. Drive distance can go too far. It also doesn't use the IMU to ensure a straight line. Rotate angle can overshoot and does not correct. Rotating to an angle is actually a perfect application for a PID loop, shown in the next section of the guide.
