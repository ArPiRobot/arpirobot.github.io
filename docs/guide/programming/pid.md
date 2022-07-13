
## About PID Controllers

*Note: This is a very high-level overview of PID controllers as they exist in and pertain to the ArPiRobot Core Library. This is not intended to be a heavily mathematical discussion. For more details on PID controllers it is recommended to read [Wikipedia's Page](https://en.wikipedia.org/wiki/PID_controller) on the topic. Additionally, this guide often oversimplifies or generalizes some concepts to how they often apply in the ArPiRobot Core Library or in robotics applications.*


PID controllers are a common way of using sensor data to make a robot perform some measurable action. In very oversimplified terms, a PID controller is given a target (or setpoint). This setpoint is a target sensor value to reach. The PID controller is then repeatedly given the current sensor value. Using this value and some parameters (which will be discussed later) it calculates a value. This value is to be used to make the robot progress towards having the sensor reach its setpoint. The exact value depends on several factors, but by default it is between `-1.0` and `1.0` allowing it to be easily passed to a drive helper to move motors. The exact value returned depends on the "error" (or the difference between setpoint and current sensor value) and the values of parameters described below.


A PID controller uses three parameters (or gains).
- Proportional Gain (kP): The proportional gain is typically used to drive the behavior of the controller. The kP value is multiplied by the error, meaning the output is larger when the error is large, and becomes smaller as the error shrinks. Generally too large of a kP term will cause the robot to oscillate about the setpoint (and never settle at the setpoint) and too small of a kP may result in a significant "steady state error" (which is when the robot stops moving, but is not at the setpoint).
- Integral Gain (kI): An integral gain is used to reduce steady state error. It is multiplied by error over time. Note that the kI value will often be very small and should generally be much smaller than the kP value. Increasing KI also increases oscillation before the robot settles at the setpoint. Generally, too large of a kI will cause oscillation that never settles at the setpoint and too small of a kI will result in steady state error taking "too long" to eliminate.
- Derivative Gain (kD): This term is used to reduce how quickly the error changes (and thus has the effect of reducing oscillation). Increasing kD is commonly a way of reducing oscillation due to kP and / or kI. However, too high of a kD will cause unpredictable results. Generally, kD is much smaller than kP.


Generally speaking, the following combinations of parameters may be used
- P alone is sometimes sufficient (typically if steady state error is not really an issue)
- PI is good enough in some cases (typically if small amounts of oscillation before reaching the setpoint)
- PD is rarely used, but has some uses (typically if there is just a little too much oscillation). However, it is often better to reduce kP (allowing some steady state error) and increase kI (to correct the error) before adding a kD term.


Additionally, the PID controller object in the ArPiRobot Core Library has one more term. An "F" or feed-forward term. This is a very simple term that is only useful in certain scenarios. The kF value is multiplied by the setpoint to produce a "baseline" output. This is useful with velocity control for example. The kF can get close to the target and a small kP and / or kI (and sometimes kD) can correct for error / changes in resistance.


## Code for a PID Controller

The ArPiRobot Core Library includes a `PID` object. This object supports setting all four gains previously described, as well as a min and max output (defaulting to -1.0 and 1.0 respectively). The gains can be changed at any time.

The `set_setpoint` / `setSetpoint` function is used to assign a setpoint for the PID controller. The `get_output` / `getOutput` function is used to calculate the current output. This function is passed the current sensor value as an argument and returns the output. For mathematical reasons, this function should be called at fairly regular intervals. As such, this makes `Action`s a good candidate to use PID objects. However, it is generally recommended to keep all PID object instances in the `Robot` class. This makes it easier to use the network table to help with tuning and allows multiple actions to reuse the same PID (without having to use the same gains in multiple places in the code).

=== "Python (`actions.py`)"
    ```py
    class PIDAction(Action):
        # Assumes my_pid is defined as a member of Robot class
        # Assumes tuning is somewhere in robot_started or elsewhere in Robot class
        # For example, it could be hard coded during instantiation
        # Ex: In Robot __init__
        #       self.my_pid = PID(1.0, 0.0, 0.0)

        def begin(self):
            # Reset before each use (clears previous state info)
            main.robot.my_pid.reset()

            # Use the setpoint for this action
            main.robot.my_pid.set_setpoint(setpoint_for_this_action)
        
        def process(self):
            # Get sensor value
            current_pv = main.robot.sensor.get_value()
            
            # Calculate output from PID
            out = main.robot.my_pid.get_output(current_pv)
            
            # Do something with PID output
            # Maybe move motors
        
        def finish(self, was_interrupted: bool):
            # Stop any motors or anything the PID was moving
            pass
        
        def should_continue(self) -> bool:
            # Once setpoint is reached, stop
            # Note: determining if setpoint is reached is not always trivial
            #       due to possible oscillations
            if pid_done:
                return False
            return True
            
    ```

=== "C++ (`actions.hpp`)"
    ```cpp
    // Assumes myPid is defined as a member of Robot class
    // Assumes tuning is somewhere in robot_started or elsewhere in Robot class
    // For example, it could be hard coded during instantiation
    // Ex: In Robot class declaration
    //       PID myPid { 1.0, 0.0, 0.0 };
    class PIDAction : public Action {
    protected:
        void begin() override;
        void process() override;
        void finish(bool wasInterrupted) override;
        bool shouldContinue() override;
    };
    ```

=== "C++ (`actions.cpp`)"
    ```cpp
    void PIDAction::begin(){
        // Reset before each use (clears previous state info)
        Main::robot->myPid.reset();

        // Use the setpoint for this action
        Main::robot->myPid.setSetpoint(setpointForThisAction);
    }

    void PIDAction::process(){
        // Get sensor value
        double currentPv = Main::robot->sensor.getValue();
        
        // Calculate output from PID
        double out = Main::robot.myPid.getOutput(currentPv);
        
        // Do something with PID output
        // Maybe move motors
    }

    void PIDAction::finish(bool wasInterrupted){
        // Stop any motors or anything the PID was moving
    }

    bool PIDAction::shouldContinue(){
        // Once setpoint is reached, stop
        // Note: determining if setpoint is reached is not always trivial
        //       due to possible oscillations
        if(pidDone){
            return false;
        }
        return true;
    }
    ```


## Rotate Angle Action using PID

To improve the `RotateAngleAction` from the previous section, a PID can be used. This will help reduce overshooting the angle and will improve consistency of arriving at the correct angle after rotation actions complete.

Before implementing the action, a rotation PID controller needs to be added to the `Robot` class.

=== "Python (`robot.py`)"
    ```py
    # Add with other imports
    from arpirobot.core.control import PID

    # in Robot's __init__ function
    # Arguments: kp, ki, kd, kf, min, max
    # If arguments are omitted, they default to 0.0 for gains or -1.0 / 1.0 for min / max
    # min / max define range of PID controller's output values
    # -1.0 to 1.0 is used as this is the range of rotation speeds accepted by a drive helper
    self.rotate_pid = PID(1.0, 0.0, 0.0, 0.0, -1.0, 1.0)
    ```
=== "C++ (`robot.hpp`)"
    ```cpp
    // Add with other includes
    #include <arpirobot/core/control/PID.hpp>

    // With other member variables in Robot class
    // Arguments: kp, ki, kd, kf, min, max
    // If arguments are omitted, they default to 0.0 for gains or -1.0 / 1.0 for min / max
    // min / max define range of PID controller's output values
    // -1.0 to 1.0 is used as this is the range of rotation speeds accepted by a drive helper
    PID rotatePid { 1.0, 0.0, 0.0, 0.0, -1.0, 1.0 };
    ```

The initial gains and output range for the PID controller are passed as arguments to the constructor, but they can all be changed later using set functions.

An action to rotate using the PID controller can be implemented as shown below

=== "Python (`actions.py`)"
    ```py
    class RotatePIDAction(Action):
        def __init__(self, degrees: float):
            super().__init__()
            
            # Store the angle this action instance should rotate
            self.degrees = degrees

            # This will be used to detect when the robot has reached its target
            self.correct_counter = 0

        
        def begin(self):
            # Reset correct counter to zero
            self.correct_counter = 0

            # Put motors in brake mode (often better for automated actions)
            main.robot.flmotor.set_brake_mode(True)
            main.robot.frmotor.set_brake_mode(True)
            main.robot.rlmotor.set_brake_mode(True)
            main.robot.rrmotor.set_brake_mode(True)

            # Configure rotate PID controller for use
            # Reset it to a clean state and set a new setpoint
            # Setpoint is target angle, which is self.degrees away from where the robot is currently facing
            main.robot.rotate_pid.reset()
            main.robot.rotate_pid.set_setpoint(main.robot.imu.get_gyro_z() + self.degrees)

        def process(self):
            # Calculate the rotation speed using the PID
            rot = main.robot.rotate_pid.get_output(main.robot.imu.get_gyro_z())

            # Update the current rotation speed
            # Note: it is possible that rotating the robot with positive power makes the IMU angle more negative
            # If this is the case, the PID will rotate forever. To fix, use "-rot" instead of "rot" below
            main.robot.drive_helper.update(0, rot)
        
        def finish(self, was_interrupted: bool):
            # Stop moving motors
            main.robot.drive_helper.update(0, 0)
        
        def should_continue(self) -> bool:
            # This action should stop when the robot has been "close enough" to the target angle for "long enough"
            # For this action, allow within 3 degrees of target angle
            # Require 10 iterations of being at the correct angle
            # 10 iterations at 50ms between each iteration = 0.5 second
            # Requiring the robot to be within 3 degrees of the correct angle for 0.5 seconds
            # prevents the action from stopping if the robot is oscillating through the correct angle
            angle_error = abs(main.robot.imu.get_gyro_z() - main.robot.rotate_pid.get_setpoint())

            if angle_error <= 3.0:
                # Within 3 degrees. Angle is correct
                self.correct_counter += 1
            else:
                # Not within 3 degrees. Reset correct counter
                # Must be correct for 10 consecutive iterations before exit is valid
                self.correct_counter = 0
            
            # Stop if correct_counter is at least 10, therefore continue if less than 10
            return self.correct_counter < 10

    ```
=== "C++ (`actions.hpp`)"
    ```cpp
    class RotatePIDAction : public Action {
    public:
        RotatePIDAction(double degrees);

    protected:
        void begin() override;
        void process() override;
        void finish(bool wasInterrupted) override;
        bool shouldContinue() override;
    
    private:
        // Store angle passed to action (angle to rotate)
        double degrees;

        // Used to detect when robot has reached its target
        int correctCounter = 0;
    };
    ```
=== "C++ (actions.cpp`)"
    ```cpp
    RotatePIDAction::RotatePIDAction(double degrees) : degrees(degrees){

    }

    void RotatePIDAction::begin(){
        // Reset correct counter to zero
        correctCounter = 0;

        // Put motors in brake mode (often better for automated actions)
        Main::robot->flmotor.setBrakeMode(true);
        Main::robot->frmotor.setBrakeMode(true);
        Main::robot->rlmotor.setBrakeMode(true);
        Main::robot->rrmotor.setBrakeMode(true);

        // Configure rotate PID controller for use
        // Reset it to a clean state and set a new setpoint
        // Setpoint is target angle, which is self.degrees away from where the robot is currently facing
        Main::robot->rotatePid.reset();
        Main::robot->rotatePid.setSetpoint(Main::robot->imu.getGyroZ() + degrees);
    }

    void RotatePIDAction::process(){
        // Calculate the rotation speed using the PID
        double rot = Main::robot->pid.get_output(Main::robot->imu.getGyroZ());

        // Update the current rotation speed
        // Note: it is possible that rotating the robot with positive power makes the IMU angle more negative
        // If this is the case, the PID will rotate forever. To fix, use "-rot" instead of "rot" below
        Main::robot->driveHelper.update(0, rot);
    }

    void RotatePIDAction::finish(){
        // Stop moving motors
        Main::robot->driveHelper.update(0, 0);
    }

    bool RotatePIDAction::shouldContinue(){
        // This action should stop when the robot has been "close enough" to the target angle for "long enough"
        // For this action, allow within 3 degrees of target angle
        // Require 10 iterations of being at the correct angle
        // 10 iterations at 50ms between each iteration = 0.5 second
        // Requiring the robot to be within 3 degrees of the correct angle for 0.5 seconds
        // prevents the action from stopping if the robot is oscillating through the correct angle
        double angleError = std::abs(Main::robot->imu.getGyroZ() - Main::robot->rotatePid.getSetpoint());

        if(angleError <= 3.0){
            // Within 3 degrees. Angle is correct
            correctCounter++;
        }else{
            // Not within 3 degrees. Reset correct counter
            // Must be correct for 10 consecutive iterations before exit is valid
            correctCounter = 0;
        }

        // Stop if correctCounter is at least 10, therefore continue if less than 10
        return correctCounter < 10;
    }
    ```

To make tuning the PID easier, it is recommended to create an instance of the action that rotates 90 degrees and add a trigger to run this action when a button is pressed. This will allow testing just this one action while tuning.

=== "Python (`robot.py`)"
    ```py
    # In Robot class's __init__ method
    self.ROTATE_TEST_BTN = 2

    # In robot_started method
    ActionManager.add_trigger(ButtonPressedTrigger(self.gp0, self.ROTATE_TEST_BTN, RotatePIDAction(90)))
    ```
=== "C++ (`robot.hpp`)"
    ```cpp
    // Add with other constants (member variables)
    const int ROTATE_TEST_BTN = 2;
    ```
=== "C++ (`robot.cpp`)"
    ```cpp
    // In robotStarted method
    ActionManager::addTrigger(std::make_shared<ButtonPressedTrigger>(gp0, ROTATE_TEST_BTN, std::make_shared<RotatePIDAction>(90)))
    ```

The PID is then ready to be tuned. It is advised to read the following section on tuning fully before tuning the PID if you are not familiar with tuning PID controllers. Additionally, if you wish to avoid having to rebuild / redeploy the robot program each time a gain is changed for the PID controller see the section on adding PID info to the Network Table below the tuning section.

## Tuning a PID

Tuning a PID is not a trivial process. The method described here is a basic "guess and check" method. There are mathematical methods of tuning, however a guess and check approach is often used in real world applications.

To tune a PID, choose a reasonable setpoint. The setpoint should be realistic for the use of the PID controller, but large enough to "see" what is happening if possible. For the rotation example above, 90 degrees is a good setpoint to tune with.

Generally, it is recommended to start with either only a kP (optionally with a kF). A kF is useful if the output should vary proportionally to the setpoint. For example, if the PID were to attempt to achieve a certain speed, the motor power (output of PID) would grow if speed grew. However, with rotation this is not the case (output of PID goes to motors, but a higher angle does not mean higher motor power forever).

If no kF is used, start with kP = 1.0, kI = 0.0, and kD = 0.0. If a kF is used, use 0.1 as the initial kP value. Note that an initial kF value can be selected by applying an output power and measuring the sensor value. Then *kF* = *sensor_measurement* ÷ *output_power*. Note that for the rotate PID, no kF should be used.

Once the initial values have been set, the following process is recommended for tuning.

- Multiply or divide by 10 for "coarse" tuning (larger changes)
- Multiply of divide by 2 for "fine" tuning (smaller changes)
- Finally, add and subtract small amounts for final tweaks.

Tune parameters in the following order. Note that specific value recommendations are just guidelines, not rules that must be followed.

- Start with kP. If there is oscillation around the setpoint, reduce kP. If it never reaches the setpoint, increase kP. In a lot of cases with robotics, a small steady state error (slightly too small kP) is preferable to oscillation (slightly too large kP). Steady state error will be corrected with kI anyway.
- Then tune kI. Start with 1 / 1000 of kP or 0.000001 (whichever is smaller). If steady state error is too large (or is corrected for too slowly) increase kI. If oscillation occurs and prevents the robot from ever settling at the target decrease kI. Some oscillation is ok as long as the robot reaches its target eventually (oscillation needs to decay and die off).
- Finally tune kD. Start with 1 / 100 of kP or 0.0001 (whichever is smaller). If the oscillation is still too large, increase kD. If the robot behaves "erratically" decrease kD. Too large of a kD will cause unpredictable results. In many cases a kD of larger than 0.01 is a very bad idea.


## Extra: Adding PID Info / Gains to Network Table

TODO
