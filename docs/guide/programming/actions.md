# Using Actions

Up until now, this guide has used the *periodic* programming model. This is the programming model where robot functionality is implemented using the `enabled_periodic` / `enabledPeriodic` and `disabled_periodic` / `disabledPeriodic` functions in the `Robot` class. While this has worked fine so far, it becomes limiting for more complex robot programs.


## Challenges with Periodic Programming Model

The main issue with the periodic programming model is switching between tasks. Suppose you sometimes want the robot to respond to gamepad controls, at other times you want it to drive some distance on its own. How do you implement both in `enabled_periodic` / `enabledPeriodic`? You would need some variable that indicates what task you are currently performing. This could be a simple integer where zero means human drive and one means autonomous drive, however this quickly becomes difficult to work with. What happens if you want your robot to drive a triangle instead of just a straight line? Task zero is still human drive, task one drives an edge, task two rotates, task three drives the next edge and so on. Once done, it goes back to task zero. This could look something like the following

=== "Python"
    ```py
    # In enabled_periodic
    if task == 0:
        # Respond to gamepad controls
        # Drive using drive helper
        # If a button is pressed switch to task 1
        # Store start time of task 1
    elif task == 1:
        # Drive straight
        # if enough time has passed, move to task 2
        # Store start time of task 2
    elif task == 2:
        # Rotate
        # if enough time has passed, move to task 3
        # Store start time of task 3
    elif task == 3:
        # Drive straight
        # if enough time has passed, move to task 4
        # Store start time of task 4
    elif task == 4:
        # Rotate
        # if enough time has passed, move to task 5
        # Store start time of task 5
    elif task == 5:
        # Drive straight
        # if enough time has passed, move to task 6
        # Store start time of task 6
    elif task == 6:
        # Rotate
        # if enough time has passed, move to task 0
    ```

=== "C++"
    ```cpp
    // In enabledPeriodic
    if (task == 0){
        // Respond to gamepad controls
        // Drive using drive helper
        // If a button is pressed switch to task 1
        // Store start time of task 1
    }}elif (task == 1){
        // Drive straight
        // if enough time has passed, move to task 2
        // Store start time of task 2
    }elif (task == 2){
        // Rotate
        // if enough time has passed, move to task 3
        // Store start time of task 3
    }elif (task == 3){
        // Drive straight
        // if enough time has passed, move to task 4
        // Store start time of task 4
    }elif (task == 4){
        // Rotate
        // if enough time has passed, move to task 5
        // Store start time of task 5
    }elif (task == 5){
        // Drive straight
        // if enough time has passed, move to task 6
        // Store start time of task 6
    }elif (task == 6){
        // Rotate
        // if enough time has passed, move to task 0
    }
    ```

Looking at the above outline, it has a few issues. First, it is hard to read what the program is supposed to be doing. Without comments looking at code structured like the above outline would be very difficult to interpret. Second, it is hard to modify the sequence of tasks. Say you wanted to introduce a delay between task 0 and 1. You now need to change many places in the code. Finally, there is a lot of duplicated code. Three different tasks drive straight. Three rotate. Reusing code in this format is very difficult. It becomes even more difficult if you wanted to the ability to drive many different shapes. The rotate code for a triangle could not be reused for a square, so you would have seven rotate task. The code is not modular and reusable and is also difficult to follow. This is clearly a non-ideal way to create complex sequences of tasks. The action system helps to address these issues.


## What is an Action

Object oriented programming makes reusing code easy. The action system takes advantage of this. Each action is an object of a certain type. That type might be a rotate action or a drive action. If you need to rotate multiple times, the same action just has multiple instances (possibly with different parameters during construction). For example, this would allow you to define an action that rotates for some amount of time and create instances of that action to rotate for 1 second, 2 seconds, 3 seconds, etc. This allows the same code to be used for all three rotations.

This approach helps solve the code reusability / organization and readability issues, but what exactly is an action? In simple terms an actions is like one of the tasks defined previously. An action is a class that defines how to perform a specific type of task. This task could be driving with the gamepad, driving straight for some amount of time, or rotating for some amount of time. Once that class is defined instances of that class are created to represent specific tasks. Those objects (instances) can be started and stopped independently of each other.

In other words, an action is a class that defines how to perform a task. An instance of an action is an object that represents a specific task. Each instance can be started and stopped when needed to create complex robot programs.


### Action Structure

An action is a class that inherits from the builtin `Action` class (part of ArPiRobot CoreLib). Child classes must implement four functions to define the behavior of an action.

=== "Python"
    - `begin()`: Run each time an action is started. Any setup for the action should occur here including locking devices (explained later).
    - `process()`: Run periodically while an action is running. Every 50ms (by default) this function is run. This is where the main functionality of the action is often implemented.
    - `finish(was_interrupted: bool)`: Called when the action is stopped / stops. There are two ways an action can stop. First, it's `should_continue` function returns false (used to allow the action to stop itself when it finishes). Second, it can be interrupted (stopped early) for a number of reasons (explained later). The `was_interrupted` argument is used to inform the action why it is stopping. Any cleanup for the action should be done here. Anything the action was doing should also be stopped here. Sometimes, what is done depends on whether the action completed (based on `was_interrupted`).
    - `should_continue() -> bool`: This function is called after each time `process` is run. If this function returns false, this action will stop (`process` will not run again). If this function returns true, this action will continue running (`process` will be called again later unless this action is interrupted). The only way an action can finish without being interrupted is if this function returns false.


    ```py
    class MyAction(Action):
        def begin(self):
            # Setup for the action to run
            pass
        
        def process(self):
            # Implement the action
            pass

        def finish(self, was_interrupted: bool):
            # Stop anything / do any cleanup
            pass
        
        def should_continue(self) -> bool:
            # Return False when action is done
            return True
    ```

=== "C++"
    - `void begin()`: Run each time an action is started. Any setup for the action should occur here including locking devices (explained later).
    - `void process()`: Run periodically while an action is running. Every 50ms (by default) this function is run. This is where the main functionality of the action is often implemented.
    - `void finish(bool wasInterrupted)`: Called when the action is stopped / stops. There are two ways an action can stop. First, it's `shouldContinue` function returns false (used to allow the action to stop itself when it finishes). Second, it can be interrupted (stopped early) for a number of reasons (explained later). The `wasInterrupted` argument is used to inform the action why it is stopping. Any cleanup for the action should be done here. Anything the action was doing should also be stopped here. Sometimes, what is done depends on whether the action completed (based on `wasInterrupted`).
    - `bool shouldContinue()`: This function is called after each time `process` is run. If this function returns false, this action will stop (`process` will not run again). If this function returns true, this action will continue running (`process` will be called again later unless this action is interrupted). The only way an action can finish without being interrupted is if this function returns false.

    === "Header (.hpp)" 
        ```cpp
        class MyAction : public Action {
        protected:
            void begin() override;
            void process() override;
            void finish(bool wasInterrupted) override;
            bool shouldContinue() override;
        }
        ```
    === "Source (.cpp)"
        ```cpp
        void MyAction::begin(){
            // Setup for action to run
        }

        void MyAction::process(){
            // Implement the action
        }

        void MyAction::finish(bool wasInterrupted){
            // Stop anything / do any cleanup
        }

        bool MyAction::shouldContinue(){
            // Return false when action is done
            return true;
        }
        ```

TODO: Mention the actions files in robot code

TOOD: Explain using constructor to pass arguments to an action

TODO: Example code with empty "template" for an action that can be pasted into robot code files (including ctor)


### Using Actions

TODO: Explain action manager and how using actions works (manual method)

TODO: Example code

TODO: Explain action triggers

TODO: Example code w/ gamepad triggers


### Ownership of Devices

TODO: Explain locking devices and how that works (note specifically that it interrupts any previous owner)

TODO: Example code locking devices

TODO: Explain why this is beneficial

TODO: Explain that you must be careful with using periodic code if an action may lock the device you are using

TODO: Example code of an issue and show use of isLocked method to address in some cases


### Running Actions Sequentially

TODO: Explain ActionSeries and give an example

TODO: ActionSeries example code


## Making Drive Code into an Action

TODO: Move existing drive code into its own action

TODO: Start the action in robot started let it run forever


## Automated Routines using Actions

TODO: Explain the goal of this section. Note that not using sensors is not the best way to do this. Conceptually, using encoders to do a DriveDistance is better and IMU to do a RotateAngle is better than using time

### Drive Time Action

TODO: Implement this


### Rotate Time Action

TODO: Implement this


### Using the Actions

TODO: Implement an action series showing how this works. Drive a "square"

TODO: Comment on tuning drive times and rotate times to drive the square correctly


## Using Sensors Instead of Time

TODO: Explain goal. Improve time-based drive to allow easier automated routines.

### Rotate Angle Action

TODO: Replace rotate time with rotate angle that uses no PID and just stops at the given angle


### Drive Distance ACtion

TODO: Add support for encoders in robot program

TODO: Use encoders + brake mode to drive until a distance threshold has passed

### Using the Actions

TODO: Implement driving a square and triangle on button presses using automated routines

### Limitations

TODO: Explain limitations of this method. Drive distance can go too far. It also doesn't use the IMU to ensure a straight line. Rotate angle can overshoot and does not correct. Rotating to an angle is actually a perfect application for a PID loop, shown in the next section of the guide.
