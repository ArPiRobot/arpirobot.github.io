
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

TODO: Rotate action based on a PID controller

TODO: Create instance of action that rotates 90 deg when active.

TODO: Explain method of detecting steady state


## Tuning a PID

Tuning a PID is not a trivial process. The method described here is a basic "guess and check" method. There are mathematical methods of tuning, however a guess and check approach is often used in real world applications.

To tune a PID, choose a reasonable setpoint. The setpoint should be realistic for the use of the PID controller, but large enough to "see" what is happening if possible. For the rotation example above, 90 degrees is a good setpoint to tune with.

Generally, it is recommended to start with either only a kP (optionally with a kF). A kF is useful if the output should vary proportionally to the setpoint. For example, if the PID were to attempt to achieve a certain speed, the motor power (output of PID) would grow if speed grew. However, with rotation this is not the case (output of PID goes to motors, but a higher angle does not mean higher motor power forever).

If no kF is used, start with kP = 1.0, kI = 0.0, and kD = 0.0. If a kF is used, divide by 100 as the initial kP value. Note that an initial kF value can be selected by applying an output power and measuring the sensor value. Then *kF* = *sensor_measurement* ÷ *output_power*.

Once the initial values have been set, the following process is recommended for tuning.
- Multiply or divide by 10 for "coarse" tuning (larger changes)
- Multiply of divide by 2 for "fine" tuning (smaller changes)
- Finally, add and subtract small amounts for final tweaks.

Tune parameters in the following order. Note that specific value recommendations are just guidelines, not rules that must be followed.
- Start with kP. If there is oscillation around the setpoint, reduce kP. If it never reaches the setpoint, increase kP. In a lot of cases with robotics, a small steady state error (slightly too small kP) is preferable to oscillation (slightly too large kP). Steady state error will be corrected with kI anyway.
- Then tune kI. Start with 1 / 1000 of kP or 0.000001 (whichever is smaller). If steady state error is too large (or is corrected for too slowly) increase kI. If oscillation occurs and prevents the robot from ever settling at the target decrease kI. Some oscillation is ok as long as the robot reaches its target eventually (oscillation needs to decay and die off).
- Finally tune kD. Start with 1 / 100 of kP or 0.0001 (whichever is smaller). If the oscillation is still too large, increase kD. If the robot behaves "erratically" decrease kD. Too large of a kD will cause unpredictable results. In many cases a kD of larger than 0.01 is a very bad idea.
