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
        };
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

In robot programs, actions are typically created in the `actions` files (`actions.py` or `actions.hpp` and `actions.cpp`). All action classes are defined in these files. In contrast, the `robot` files (`robot.py` or `robot.hpp` and `robot.cpp`) are used only for the `Robot` class. Instances of actions are often created in the `Robot` class. For example, the following could be added to the `actions` files to create an action (this action does nothing, it's just an example).

=== "Python (`actions.py`)"
    ```py
    class MyAction(Action):
        def begin(self):
            pass
        
        def process(self):
            pass

        def finish(self, was_interrupted: bool):
            pass

        def should_continue(self) -> bool:
            return False
    ```

=== "C++ (`actions.hpp`)"
    ```cpp
    class MyAction : public Action{
    protected:
        void begin() override;
        void process() override;
        void finish(bool wasInterrupted) override;
        bool shouldContinue() override;
    };
    ```

=== "C++ (`actions.cpp`)"
    ```cpp
    void MyAction::begin(){

    }

    void MyAction::process(){

    }

    void MyAction::finish(bool wasInterrupted){

    }

    bool MyAction::shouldContinue(){
        return false;
    }
    ```

Then an instance of the action is created in the robot class. This instance can be used as described in later sections.

=== "Python (`robot.py`)"
    ```py
    # Add somewhere in __init__
    self.my_instance = MyAction()
    ```

=== "C++ (`robot.hpp`)"
    ```cpp
    // Add somewhere in Robot class declaration
    MyAction myInstance;
    ```

Often, you will only have one instance of an action (or multiple instances that all behave the same way), but in other cases you may have multiple instances that all work differently. Consider an action that waits a certain amount of time (why this is useful will be seen later). It is likely that you would want to wait for different amounts of time (eg 3 seconds, 5 seconds, 10 seconds). Instead of making three actions that all wait for an amount of time, the you can create an action that takes an argument in its constructor for the amount of time to wait. Then, you only need three instances of the action for 3, 5, and 10 seconds.

=== "Python (`actions.py`)"
    ```py
    # Note: Add "import time" at top of file

    class WaitAction(Action):

        # __init__ function is constructor run when creating instance
        # Add arguments after "self"
        # In this case one argument for wait time (in seconds) is added
        def __init__(self, wait_time_sec: float):
            # ALWAYS CALL PARENT CLASS INIT!!!
            super().__init__()

            # Store the argument's value in a member variable for use later
            self.wait_time_sec = wait_time_sec

            # Another member variable to store when the action starts
            # Just store some value for now. This value won't be used
            self.start_time: float = 0.0

        def begin(self):
            # Action is started when begin() runs. Store current time
            self.start_time = time.time()
        
        def process(self):
            # Nothing to do. Just waiting
            pass

        def finish(self, was_interrupted: bool):
            # Action is done. Done waiting, so nothing needs to be done here
            pass

        def should_continue(self) -> bool:
            # Should continue waiting if (now - start) < wait_time
            return (time.time() - self.start_time) < self.wait_time_sec
    ```

=== "C++ (`actions.hpp`)"
    ```cpp
    // Note: Import "chrono" at top of file

    class WaitAction : public Action {
    public:
        // Add constructor taking argument
        // Constructor is run when creating instance of action
        WaitAction(double waitTimeSec);

    protected:
        void begin() override;
        void process() override;
        void finish(bool wasInterrupted) override;
        bool shouldContinue() override;
    
    private:
        // Member variables to store wait time and start time
        double waitTimeSec;
        std::chrono::time_point<std::chrono::steady_clock> startTime;
    }
    ```

=== "C++ (`actions.cpp`)"
    ```
    // Using initializer list to assign member varible waitTimeSec to argument value
    WaitAction::WaitAction(double waitTimeSec) : waitTimeSec(waitTimeSec) {
        
    }

    void WaitAction::begin(){
        // Action is started when begin runs. Store start time.
        startTime = std::chrono::steady_clock::now();
    }

    void WaitAction::process(){
        // Nothing to do. Just waiting.
    }

    void WaitAction::finish(bool wasInterrupted){
        // Action is done. Done waiting, so nothing needs to be done here
    }

    bool WaitAction::shouldContinue(){
        // Should continue waiting if (now - start) < wait_time
        auto now = std::chrono::steady_clock::now();
        return std::chrono::duration_cast<std::chrono::seconds>(now - startTime).count() < waitTimeSec;
    }
    ```

Then in the robot class, several instances are created as shown below

=== "Python (`robot.py`)
    ```py
    # In __init__
    self.wait3s = WaitAction(3)
    self.wait5s = WaitAction(5)
    self.wait10s = WaitAction(10)
    ```

=== "C++ (`robot.hpp`)"
    ```cpp
    // In class declaration
    WaitAction wait3s {3};
    WaitAction wait5s {5};
    WaitAction wait10s {10};
    ```

This method allows the same code to be used for waiting regardless of how long the robot is waiting for.


### Using Actions

The `ActionManager` provides two functions `start_action` / `startAction` and `stop_action` / `stopAction` to start and stop actions respectively. These functions can be used in your robot program when needed to manage actions. For instance, you could start an action in `robot_enabled` / `robotEnabled` and stop it in `robot_disabled` / `robotDisabled`.

=== "Python"
    ```py
    # Add with imports if not there
    from arpirobot.core.action import ActionManager

    # Use these where needed
    ActionManager.start_action(self.my_instance)
    ActionManager.stop_action(self.my_instance)
    ```

=== "C++"
    ```cpp
    // Add with includes if not there
    #include <arpirobot/core/action/ActionManager.hpp>

    ActionManager::startAction(myInstance);
    ActionManager::stopAction(myInstance);
    ```

You can also start an action without holding a reference to the instance, but you will not be able to stop it using the `stop_action` / `stopAction` function. This is often useful in `robot_started` / `robotStarted` for actions that should always remain running.

=== "Python"
    ```py
    # This is ok because the action manager will keep the action object in scope even if your code does not
    ActionManager.start_action(MyAction())
    ```

=== "C++"
    ```cpp
    // When passing dynamically allocated objects the C++ library uses shared_ptrs
    // As such it is necessary to use std::make_shared
    // Statically allocated objects are intended to be passed by reference (as shown previously)
    // Since shared_ptrs are used there is no need to handle deletion of the dynamically allocated action
    // The action manager holds a reference to it while it is running.
    ActionManager::startAction(std::make_shared<MyAction>());
    ```

Often, it is useful to be able to determine if an action is already running. before attempting to start / stop it. This can be done using the action's `is_running` / `isRunning` function.

=== "Python"
    ```py
    running = self.my_instance.is_running()
    ```

=== "C++"
    ```cpp
    bool running = myInstance.isRunning();
    ```

This can be used to prevent restarting an action that is already running. By default if you attempt to start an action that is already running it will be restarted. This means that it will be stopped and started again. This behavior can be disabled by using the second (optional) argument of `start_action` / `startAction`. If this argument is false, the action will not be restarted if running. It will continue running, but a warning will be added to the robot program log.

=== "Python"
    ```py
    ActionManager.start_action(self.my_instance, False)
    ```

=== "C++"
    ```cpp
    ActionManger::startAction(myInstance, false);
    ```

Note that the `start_action` / `startAction` function also returns a bool indicating if an action was started successfully. This will only return false if the action is already running and is not allowed to be restarted (or if you attempt to start an action before the robot has started which should never happen).


<br />

Using the `ActionManager` functions is an easy way to manually start and stop actions, however there is also a way to start actions when some even occurs. The `ActionManager` can have triggers that start a specified action when something occurs. What that event is depends on the type of trigger. The most common trigger is a gamepad related trigger. There are two such triggers `ButtonPressedTrigger` and `ButtonReleasedTrigger`. These triggers are used to start an action when a button on the gamepad is pressed or released respectively. Once a trigger is added to the action manager, the action associated with it will be started (or restarted) whenever the event occurs. This will remain the case until the trigger is removed from the `ActionManager`.

=== "Python"
    ```py
    # Starts action instance my_instance when button 0 on gp0 is pressed
    ActionManager.add_trigger(ButtonPressedTrigger(self.gp0, 0, self.my_instance))

    # If you need to remove later use ActionManager.remove_trigger, but hold an instance of the trigger
    self.trigger = ButtonPressedTrigger(self.gp0, 0, self.my_instance)
    ActionManager.add_trigger(self.trigger)

    # And later
    ActionManager.remove_trigger(self.trigger)
    ```

=== "C++"
    ```
    // Starts action instance myInstance when button 0 on gp0 is pressed
    ActionManager::addTrigger(std::make_shared<ButtonPressedTrigger>(gp0, 0, myInstance));

    // If you need to remove later use ActionManager::removeTrigger, but hold an instance of the trigger
    // Add "trigger" variable as a member in class declaration
    ButtonPressedTrigger trigger {gp0, 0, myInstance};

    // Then in a function somewhere
    ActionManager::addTrigger(trigger);

    // And later
    ActionManager::removeTrigger(trigger);
    ```

As with `start_action` / `startAction`, an trigger will restart an action by default. If this behavior is not desired use the optional argument after the action instance.

=== "Python"
    ```py
    self.trigger = ButtonPressedTrigger(self.gp0, 0, self.my_instance, False)
    ```

=== "C++"
    ```cpp
    ButtonPressedTrigger trigger {hp0, 0, myInstance, false};
    ```

### Running Actions Sequentially

Any actions started will run concurrently, meaning that starting a second action will not wait for the currently running action to finish. While this parallel execution of actions is useful for developing complex routines and maximizing code sharing, it is equally useful to be able to run a set of actions sequentially (one after the other).

This is achieved using an `ActionSeries`. An `ActionSeries` is itself an action that can be started and stopped with the action manager, however it's task is to manage other actions. When creating an `ActionSeries` a list of actions is used. This list of actions is run sequentially. The first action is started. When it finishes, the second, then when it finishes the third and so on. The `ActionSeries` (being an `Action` itself) remains running until all of its actions finish (or until it is interrupted).

=== "Python"
    ```py
    self.series = ActionSeries(
        # List of actions to be run as a part of the series
        [
            action1,
            action2,
            action3,
            ...
            action_n
        ], 

        # finished_action
        # An action to start when the action series is done
        # This action is not part of the action series so the action series
        # will not continue running while this action is running
        None
    )
    ```

=== "C++"
    ```cpp
    ActionSeries series{
        // List of actions to be run as a part of the series
        {
            action1,
            action2,
            action3,
            ...
            actionN
        },

        // finishedAction
        // An action to start when the action series is done
        // This action is not part of the action series so the action series
        // will not continue running while this action is running
        nullptr
    };
    ```

There is also an optional "finished action" that can be automatically started when the action series finishes. This is often useful to avoid having actions that run "forever" be a part of the action series so you can determine when an action series completes its primary task. Consider the following example. The action series is used to drive a square. By making the human drive action the "finished action" this ensures that when the square driving is done, human control will resume. However, once human control resumes the "drive square" action is no longer running, which makes sense as the square has been driven.

=== "Python"
    ```py
    self.drive_square_series = ActionSeries(
        # List of actions to be run as a part of the series
        [
            DriveDistanceAction(10),
            RotateAngleAction(90),
            DriveDistanceAction(10),
            RotateAngleAction(90),
            DriveDistanceAction(10),
            RotateAngleAction(90),
            DriveDistanceAction(10),
            RotateAngleAction(90)
        ], 

        # finished_action
        # An action to start when the action series is done
        # This action is not part of the action series so the action series
        # will not continue running while this action is running
        HumanDriveAction()
    )
    ```

=== "C++"
    ```cpp
    ActionSeries driveSquareSeries{
        // List of actions to be run as a part of the series
        {
            std::make_shared<DriveDistanceAction>(10),
            std::make_shared<RotateAngleAction>(90),
            std::make_shared<DriveDistanceAction>(10),
            std::make_shared<RotateAngleAction>(90),
            std::make_shared<DriveDistanceAction>(10),
            std::make_shared<RotateAngleAction>(90),
            std::make_shared<DriveDistanceAction>(10),
            std::make_shared<RotateAngleAction>(90)
        },

        // finishedAction
        // An action to start when the action series is done
        // This action is not part of the action series so the action series
        // will not continue running while this action is running
        std::make_shared<HumanDriveAction>()
    };
    ```

Since `ActionSeries` are actions, multiple can run in parallel too. This allows for complex routines to be developed using the action system.


### Ownership of Devices

One final important concept with actions is the ownership of devices. Devices are many of the "objects" created on the robot. More specifically, a "Device" is anything connected directly to the Raspberry Pi on the robot that has its own object in code (such as motors). Note that currently, devices attached to the arduino (such as sensors) are "ArduinoDevices" and are not able to be owned by actions.

Often, multiple actions in a robot program control the same device. In many cases, this can cause unintended behaviors. Consider a scenario where one action allows a gamepad to move motors, and another uses motors to drive a square. Which action would "win"? It is likely the actions would "fight" each other causing the robot to rapidly switch between the tasks (rapidly meaning 20+ times per second).

This clearly would prevent either action from working as intended. The solution is to ensure that only one action that controls the motors is ever active at a time. In general, the action that should remain active is the newer one. For example, if the robot is being driven with the gamepad using one action, and another action to drive a square is triggered by a button press, the square should be driven stopping gamepad drive control.

To achieve this, it is necessary to know what devices an action uses (or equivalently which action is currently controlling a device). The action controlling a device is said to "own" or "lock" a device. If a newer action is started, it can take ownership of the device. When this happens, the previous action is automatically stopped.

An `Action` can optionally have a function called `lockedDevices` / `locked_devices`. This function returns a list of devices that should be locked when the action starts (`LockedDeviceList`). When an action with this function is started, any devices in the list returned from `lockedDevices` / `locked_devices` are locked by the action. If another action is running that had previously locked any device now locked by the new action, the old action will be stopped to allow the newer action to take control.

Additionally, if you have code in `enabled_periodic` / `enabledPeriodic` or `disabled_periodic` / `disabledPeriodic` that controls a device, you will need to be careful not to control it while an action has locked the device. The device has a function called `is_locked` / `isLocked` which can be used to determine if the device is locked by an action. **Unlike actions, the periodic functions cannot lock a device. As such, it is highly recommended to either control a device using actions or the periodic functions, not both.** However, it is not a problem to control some devices using actions and others from the `Robot`'s periodic functions.


## Making Drive Code into an Action

Before attempting to create complex setups with actions, it is a good idea to convert the gamepad drive code into an action. Later, actions will be implemented that are used to drive shapes. Since these actions will control the motors, it is a bad idea to also have `enabled_periodic` / `enabledPeriodic` controlling the motors with the gamepad.

To start, create a new action in `actions.py` or `actions.hpp` and `actions.cpp`

=== "Python (`actions.py`)"
    ```py
    class JSDriveAction(Action):
        def locked_devices(self) -> LockedDeviceList:
            pass
        
        def begin(self):
            pass
        
        def process(self):
            pass
        
        def finish(self, was_interrupted: bool):
            pass
        
        def should_continue(self) -> bool:
            pass
    ```

=== "C++ (`actions.hpp`)"
    ```cpp
    class JSDriveAction : public Action{
    protected:
        LockedDeviceList lockedDevices() override;
        void begin() override;
        void process() override;
        void finish(bool wasInterrupted) override;
        bool shouldContinue() override;
    };
    ```

=== "C++ (`actions.cpp`)"
    ```cpp
    LockedDeviceList JSDriveAction::lockedDevices(){

    }

    void JSDriveAction::begin(){

    }

    void JSDriveAction::process(){

    }

    void JSDriveAction::finish(bool wasInterrupted){

    }

    bool JSDriveAction::shouldContinue(){

    }
    ```

Then, in `process` move the existing drive code from `enabled_periodic` / `enabledPeriodic`. You will have to change a few variable names. The drive helper object and gamepad object belong to the `Robot` instance, not the `Action` instance the code is now in. As such, you have to refer to them using the `Robot` instance. This is done using `main.robot` in python or `Main::robot` in C++.

=== "Python (`actions.py`)"
    ```py
    def process(self):
        # Get value for the speed axis
        speed = main.robot.gp0.get_axis(main.robot.SPEED_AXIS, main.robot.DEADBAND)
        
        # Get value for the rotation axis
        rotation = main.robot.gp0.get_axis(main.robot.ROTATE_AXIS, main.robot.DEADBAND)

        # Update speed of all motors using drive helper
        main.robot.drive_helper.update(speed, rotation)
    ```

=== "C++ (`actions.cpp`)"
    ```cpp
    void JSDriveAction::process(){
        // Get value for speed axis
        double speed = Main::robot->gp0.getAxis(Main::robot->SPEED_AXIS, Main::robot->DEADBAND);
        
        // Get value for the rotation axis
        double rotation = Main::robot->gp0.getAxis(Main::robot->ROTATE_AXIS, Main::robot->DEADBAND);

        // Update speed of all motors using drive helper
        Main::robot->driveHelper.update(speed, rotation);
    }
    ```

Then, in `locked_devices` / `lockedDevices`, add the following to lock the motors. All four motors are locked as this action (indirectly) controls all four using the drive helper. *Note: You cannot lock the drive helper. It is not a device.* Locking the motors is not important yet, but having this will be important later.

=== "Python (`actions.py`)"
    ```py
    def locked_devices(self) -> LockedDeviceList:
        # If your robot only has two motors, only reference those motors
        return [ main.robot.flmotor, main.robot.frmotor, 
                main.robot.rlmotor, main.robot.rrmotor ]
    ```

=== "C++ (`actions.cpp`)"
    ```cpp
    LockedDeviceList JSDriveAction::lockedDevices(){
        // If your robot only has two motors, only reference those motors
        return { Main::robot->flmotor, Main::robot->frmotor, 
            Main::robot->rlmotor, Main::robot->rrmotor };
    }
    ```

Next, in the action's `finish` function add the following line to ensure that motors stop moving if the action is ever stopped. When an action is stopped it should (almost) always make sure anything it was controlling is returned to a safe state. For motors, stopped is a safe state.

=== "Python (`actions.py`)"
    ```py
    def finish(self, was_interrupted: bool):
        main.robot.drive_helper.update(0, 0)
    ```

=== "C++ (`actions.cpp`)"
    ```cpp
    void JSDriveAction::finish(){
        Main::robot->driveHelper.update(0, 0);
    }
    ```

Then, make `should_continue` / `shouldContinue` always return true so this action runs forever.

=== "Python (`actions.py`)"
    ```py
    def should_continue(self) -> bool:
        return True
    ```

=== "C++ (`actions.cpp`)"
    ```cpp
    bool shouldContinue(){
        return true;
    }
    ```

Finally, start the action in `robot_started` / `robotStarted`. Since the action never finishes, it will run forever. The action actually remains running whether the robot is enabled or disabled, however the robot's motors are disabled when the robot is disabled so you will still be unable to drive the robot while it is disabled.

=== "Python (`robot.py`)"
    ```py
    # Import action at top of file (with other imports)
    from actions import JSDriveAction

    # Add at the end of robot_started
    ActionManager.start_action(JSDriveAction())
    ```

=== "C++ (`robot.cpp`)"
    ```cpp
    // No need to include anything for C++
    // actions.hpp should already be included

    // Add at the end of robotStarted
    ActionManager::startAction(std::make_shared<JSDriveAction>());
    ```

After building and deploying this program to the robot, driving with the gamepad will still work as it previously had, but since it is now implemented as an action it is easy to start and stop as needed to allow other actions to run.

## Automated Routines using Actions

Actions can be used as building blocks for complex tasks. In this section two tasks will be implemented using actions. The first task will drive a circle. The second will drive a square. When neither task is active, the robot will be able to be controlled using the gamepad (using the `JSDriveAction` implemented previously).

While it would be possible to make an action to drive each shape, this is not the easiest way to do so and does not allow much code reuse. Instead, driving shapes is broken down into simpler actions that can be chained together to make shapes. This also allows the same actions to be used to drive multiple different shapes.

Driving both a square and a triangle can be accomplished using two basic actions. First, the robot needs to be able to drive a straight line. Second, it needs to be able to rotate. These two actions can be repeated as many times as necessary to drive the desired shape.

Ideally, driving a straight line would be done using encoders on the robot to drive some specific distance and rotation would use a gyroscope to rotate to some angle, however for the sake of simplicity, time will be used instead in this section. This means that instead of an action that drives some number of inches we will have an action that drives for some amount of time. For rotation, it will rotate for some amount of time. *Timed driving is not a precise way to create shapes. You may need to change some times (especially for the rotations) to drive the correct shape. The code in this section will behave differently on different robots.*


### Drive Time Action

The first action to be implemented needs to drive a straight line for some amount of time (this will be passed as an argument in the constructor so the same action can be used for different amounts of time). This action will control motors, so they should be locked in `begin`. Additionally, the robot will drive a constant speed for the given amount of time. As such, the robot can start driving in `begin` and stop in `finish` leaving `process` empty. Finally, `should_continue` / `shouldContinue` needs to return true until enough time has passed, when it should return false stopping the action. This requires tracking when the action is started by storing some value in `begin`. This is all implemented in the code shown below.

=== "Python (`actions.py`)"
    ```py
    # Add this import at the top of the file (with other imports)
    import time

    class DriveTimeAction(Action):
        # Each instance of the action can drive for a different duration
        # This allows reusing the same action for various purposes
        def __init__(self, duration_sec: float):
            super().__init__()

            # Store the duration this action should run for
            self.duration_sec = duration_sec / 1000

            # Will be used to store the time when the action starts (begin called)
            self.start_time = 0.0

        def locked_devices(self) -> LockedDeviceList:
            # Lock motors as this action controls them
            # This stops whatever action currently controls the motors (if any)
            return [ main.robot.flmotor, main.robot.frmotor, 
                main.robot.rlmotor, main.robot.rrmotor ]

        def begin(self):
            # Store the time this action started
            self.start_time = time.time()

            # Start moving the motors forward at 70% speed, no rotation
            main.robot.drive_helper.update(0.7, 0)
        
        def process(self):
            # Nothing to do here. Just let motors keep running
            pass
        
        def finish(self, was_interrupted: bool):
            # Stop the motors
            main.robot.drive_helper.update(0, 0)

        def should_continue(self) -> bool:
            now = time.time()
            if (now - self.start_time) >= self.duration_sec:
                return False
            return True
    ```

=== "C++ (`robot.hpp`)"
    ```cpp
    // Add with other imports
    #include <chrono>

    class DriveTimeAction : public Action {
    public:
        // Each instance of the action can drive for a different duration
        // This allows reusing the same action for various purposes
        DriveTimeAction(double durationSec);

    protected:
        LockedDeviceList lockedDevices() override;
        void begin() override;
        void process() override;
        void finish(bool wasInterrupted) override;
        bool shouldContinue() override;
    
    private:
        // Store the duration this action should run for
        double durationSec;
        
        // Will be used to store the time when the action starts (begin called)
        std::chrono::time_point<std::chrono::steady_clock> startTime;
    };
    ```

=== "C++ (`robot.cpp`)"
    ```cpp
    DriveTimeAction::DriveTimeAction(double durationSec) : durationSec(durationSec){

    }

    LockedDeviceList DriveTimeAction::lockedDevices(){
        // Lock motors as this action controls them
        // This stops whatever action currently controls the motors (if any)
        return { Main::robot->flmotor, Main::robot->frmotor, 
            Main::robot->rlmotor, Main::robot->rrmotor };
    }

    void DriveTimeAction::begin(){        
        // Store the time this action started 
        startTime = std::chrono::steady_clock::now();

        // Start moving the motors forward at 70% speed, no rotation
        Main::robot->driveHelper.update(0.7, 0);
    }

    void DriveTimeAction::process(){
        // Nothing to do here. Just let motors keep running
    }

    void DriveTimeAction::finish(bool wasInterrupted){
        // Stop the motors
        Main::robot->driveHelper.update(0, 0);
    }

    bool DriveTimeAction::shouldContinue(){
        double elapsedSec = std::chrono::duration_cast<std::chrono::seconds>(now - startTime).count();
        if(elapsedSec >= durationSec)
            return false;
        return true;
    }

    ```


### Rotate Time Action

The rotate time action is implemented almost the same way as the drive time action. Instead of driving forward at 70% speed though, the action rotates at 90% speed.

=== "Python (`actions.py`)"
    ```py
    # Add this import at the top of the file (with other imports)
    import time

    class RotateTimeAction(Action):
        # Each instance of the action can drive for a different duration
        # This allows reusing the same action for various purposes
        def __init__(self, duration_sec: float):
            super().__init__()

            # Store the duration this action should run for
            self.duration_sec = duration_sec / 1000

            # Will be used to store the time when the action starts (begin called)
            self.start_time = 0.0

        def locked_devices(self) -> LockedDeviceList:
            # Lock motors as this action controls them
            # This stops whatever action currently controls the motors (if any)
            return [ main.robot.flmotor, main.robot.frmotor, 
                main.robot.rlmotor, main.robot.rrmotor ]

        def begin(self):            
            # Store the time this action started
            self.start_time = time.time()

            # Start moving the motors at 90% rotation (no forward / reverse speed)
            main.robot.drive_helper.update(0, 0.9)
        
        def process(self):
            # Nothing to do here. Just let motors keep running
            pass
        
        def finish(self, was_interrupted: bool):
            # Stop the motors
            main.robot.drive_helper.update(0, 0)

        def should_continue(self) -> bool:
            now = time.time()
            if (now - self.start_time) >= self.duration_sec:
                return False
            return True
    ```

=== "C++ (`robot.hpp`)"
    ```cpp
    // Add with other imports
    #include <chrono>

    class RotateTimeAction : public Action {
    public:
        // Each instance of the action can drive for a different duration
        // This allows reusing the same action for various purposes
        RotateTimeAction(double durationSec);

    protected:
        LockedDeviceList lockedDevices() override;
        void begin() override;
        void process() override;
        void finish(bool wasInterrupted) override;
        bool shouldContinue() override;
    
    private:
        // Store the duration this action should run for
        double durationSec;
        
        // Will be used to store the time when the action starts (begin called)
        std::chrono::time_point<std::chrono::steady_clock> startTime;
    };
    ```

=== "C++ (`robot.cpp`)"
    ```cpp
    RotateTimeAction::RotateTimeAction(double durationSec) : durationSec(durationSec){

    }

    LockedDeviceList RotateTimeAction::lockedDevices(){
        // Lock motors as this action controls them
        // This stops whatever action currently controls the motors (if any)
        return { Main::robot->flmotor, Main::robot->frmotor, 
            Main::robot->rlmotor, Main::robot->rrmotor };
    }

    void RotateTimeAction::begin(){        
        // Store the time this action started 
        startTime = std::chrono::steady_clock::now();

        // Start moving the motors at 90% rotation (no forward / reverse speed)
        Main::robot->driveHelper.update(0, 0.9);
    }

    void RotateTimeAction::process(){
        // Nothing to do here. Just let motors keep running
    }

    void RotateTimeAction::finish(bool wasInterrupted){
        // Stop the motors
        Main::robot->driveHelper.update(0, 0);
    }

    bool RotateTimeAction::shouldContinue(){
        double elapsedSec = std::chrono::duration_cast<std::chrono::seconds>(now - startTime).count();
        if(elapsedSec >= durationSec)
            return false;
        return true;
    }
    ```


### Brake Mode

Generally, coast mode is better when driving the robot with a gamepad. It results in more natural control of the robot and prevents sudden stops when changing directions. However, when performing automated driving, it is often better to use brake mode. This ensures the robot stops quicker when the action is finished driving and makes driving more consistent.

As such, the motors should be in brake mode for the drive time and rotate time actions, but in coast mode for the joystick drive action. This can be achieved by setting the motors to brake / coast mode as needed in an action's `begin` function.

Add the following lines to the end of each action's `begin` function as indicated. For robots with fewer motors, only set the brake mode for the motors the robot has (and change motor object names as needed).

=== "Python (`actions.py`)"
    ```py
    # Add to DriveTimeAction's begin
    main.robot.flmotor.set_brake_mode(True)
    main.robot.frmotor.set_brake_mode(True)
    main.robot.rlmotor.set_brake_mode(True)
    main.robot.rrmotor.set_brake_mode(True)

    # Add to RotateTimeAction's begin
    main.robot.flmotor.set_brake_mode(True)
    main.robot.frmotor.set_brake_mode(True)
    main.robot.rlmotor.set_brake_mode(True)
    main.robot.rrmotor.set_brake_mode(True)

    # Add to JSDriveAction's begin
    main.robot.flmotor.set_brake_mode(False)
    main.robot.frmotor.set_brake_mode(False)
    main.robot.rlmotor.set_brake_mode(False)
    main.robot.rrmotor.set_brake_mode(False)
    ```

=== "C++ (`robot.cpp`)"
    ```cpp
    // Add to DriveTimeAction's begin
    Main::robot->flmotor.setBrakeMode(true);
    Main::robot->frmotor.setBrakeMode(true);
    Main::robot->rlmotor.setBrakeMode(true);
    Main::robot->rrmotor.setBrakeMode(true);

    // Add to RotateTimeAction's begin
    Main::robot->flmotor.setBrakeMode(true);
    Main::robot->frmotor.setBrakeMode(true);
    Main::robot->rlmotor.setBrakeMode(true);
    Main::robot->rrmotor.setBrakeMode(true);

    // Add to JSDriveAction's begin
    Main::robot->flmotor.setBrakeMode(false);
    Main::robot->frmotor.setBrakeMode(false);
    Main::robot->rlmotor.setBrakeMode(false);
    Main::robot->rrmotor.setBrakeMode(false);
    ```


### Using the Actions

The `DriveTimeAction`, `RotateTimeAction` and `WaitAction` (shown earlier) can be used to create `ActionSeries` to drive a square and triangle. These action series will look something like the following, however *you will likely need to change the drive and rotate times to suit your specific robot and the surface you are driving on*.

=== "Python (`robot.py`)"
    ```py
    # Add with other imports
    from actions import DriveTimeAction, RotateTimeAction, JSDriveAction, WaitAction

    ################################################################################
    # Add in __init__
    ################################################################################

    # Constants for button mappings on gamepad
    self.SQUARE_BTN = 0
    self.TRIANGLE_BTN = 1

    # Action and ActionSeries instances
    self.js_drive_action = JSDriveAction()
    self.drive_square_series = ActionSeries(
        # This is a list of actions to run sequentially
        [
            DriveTimeAction(2),         # First edge
            RotateTimeAction(3.5),      # Rotate 90 (adjust time as needed)

            WaitAction(0.5),            # Delay before driving next edge

            DriveTimeAction(2),         # Second edge
            RotateTimeAction(3.5),      # Rotate 90 (adjust time as needed)

            WaitAction(0.5),            # Delay before driving next edge

            DriveTimeAction(2),         # Third edge
            RotateTimeAction(3.5),      # Rotate 90 (adjust time as needed)

            WaitAction(0.5),            # Delay before driving next edge

            DriveTimeAction(2),         # Fourth edge
            RotateTimeAction(3.5),      # Rotate 90 (adjust time as needed)

            WaitAction(0.1)             # Small delay so brake mode has time to work

        ],

        # Start JSDriveAction instance when this action series finishes
        self.js_drive_action
    )
    self.drive_triangle_series = ActionSeries(
        # This is a list of actions to run sequentially
        [
            DriveTimeAction(2),         # First edge
            RotateTimeAction(4.5),      # Rotate 120 (adjust time as needed)

            WaitAction(0.5),            # Delay before driving next edge

            DriveTimeAction(2),         # Second edge
            RotateTimeAction(4.5),      # Rotate 120 (adjust time as needed)

            WaitAction(0.5),            # Delay before driving next edge

            DriveTimeAction(2),         # Third edge
            RotateTimeAction(4.5),      # Rotate 120 (adjust time as needed)

            WaitAction(0.1)             # Small delay so brake mode has time to work
        ],

        # Start JSDriveAction instance when this action series finishes
        self.js_drive_action
    )


    ################################################################################
    # In robot_started
    ################################################################################
    
    # Remove this line
    ActionManager.start_action(JSDriveAction())

    # And replace it with
    ActionManager.start_action(self.js_drive_action())

    # Finally, add two button triggers to run the drive shape actions
    ActionManager.add_trigger(ButtonPressedTrigger(self.gp0, self.SQUARE_BTN, self.drive_square_series))
    ActionManager.add_trigger(ButtonPressedTrigger(self.gp0, self.TRIANGLE_BTN, self.drive_triangle_series))
    ```

=== "C++ (`robot.hpp`)"
    ```cpp
    ////////////////////////////////////////////////////////////////////////////////
    // Add with other member variables at the bottom of the Robot class declaration
    ////////////////////////////////////////////////////////////////////////////////

    // Constants for button mappings on gamepad
    const int SQUARE_BUTTON = 0;
    const int TRIANGLE_BUTTON = 1;

    // Action and ActionSeries instances
    JSDriveAction jsDriveAction;
    ActionSeries driveSquareSeries{
        // This is a list of actions to run sequentially
        {
            std::make_shared<DriveTimeAction>(2),         // First edge
            std::make_shared<RotateTimeAction>(3.5),      // Rotate 90 (adjust time as needed)

            std::make_shared<WaitAction>(0.5),            // Delay before driving next edge

            std::make_shared<DriveTimeAction>(2),         // Second edge
            std::make_shared<RotateTimeAction>(3.5),      // Rotate 90 (adjust time as needed)

            std::make_shared<WaitAction>(0.5),            // Delay before driving next edge

            std::make_shared<DriveTimeAction>(2),         // Third edge
            std::make_shared<RotateTimeAction>(3.5),      // Rotate 90 (adjust time as needed)

            std::make_shared<WaitAction>(0.5),            // Delay before driving next edge

            std::make_shared<DriveTimeAction>(2),         // Fourth edge
            std::make_shared<RotateTimeAction>(3.5),      // Rotate 90 (adjust time as needed)

            std::make_shared<WaitAction>(0.1)             // Small delay so brake mode has time to work

        },

        // Start JSDriveAction instance when this action series finishes
        jsDriveAction
    };
    ActionSeries driveTriangleSeries {
        // This is a list of actions to run sequentially
        {
            std::make_shared<DriveTimeAction>(2),         // First edge
            std::make_shared<RotateTimeAction>(4.5),      // Rotate 120 (adjust time as needed)

            std::make_shared<WaitAction>(0.5),            // Delay before driving next edge

            std::make_shared<DriveTimeAction>(2),         // Second edge
            std::make_shared<RotateTimeAction>(4.5),      // Rotate 120 (adjust time as needed)

            std::make_shared<WaitAction>(0.5),            // Delay before driving next edge

            std::make_shared<DriveTimeAction>(2),         // Third edge
            std::make_shared<RotateTimeAction>(4.5),      // Rotate 120 (adjust time as needed)

            std::make_shared<WaitAction>(0.1)             // Small delay so brake mode has time to work
        },

        // Start JSDriveAction instance when this action series finishes
        jsDriveAction
    };
    ```
=== "C++ (`robot.cpp`)"
    ```cpp
    ////////////////////////////////////////////////////////////////////////////////
    // In robotStarted
    ////////////////////////////////////////////////////////////////////////////////
    
    // Remove this line
    ActionManager::startAction(std::make_shared<JSDriveAction>());

    // And replace it with
    ActionManager::startAction(jsDriveAction);

    // Finally, add two button triggers to run the drive shape actions
    ActionManager.add_trigger(ButtonPressedTrigger(self.gp0, self.SQUARE_BTN, self.drive_square_series))
    ActionManager.add_trigger(ButtonPressedTrigger(self.gp0, self.TRIANGLE_BTN, self.drive_triangle_series))
    ```

If built and deployed, the buttons 0 and 1 on the gamepad (see drive station to identify which button is which) will start the drive square series or drive triangle series respectively.

Additionally, if one series is started while the other is running the older series will be stopped by the newer one. This behavior is caused by the device locking discussed earlier. When an `ActionSeries` starts, it locks all devices that its child actions would otherwise lock. The devices are not actually locked by the child actions, only by the action series.

In the above example, the "drive square" series uses an action that locks the robot's motors. The same is true of the "drive triangle" series. As such, if the "drive square" series is started the series locks the motors (since they will be used sometime while the series is running). Then, if the "drive triangle" series is started while the "drive square" series is still running, the "drive triangle" series will lock the motors. In doing so, it will stop whatever locked them last, which in this case was the "drive square" series.

This behavior can be used to ensure that `ActionSeries` can interrupt (stop) each other to allow the newest one to run, just like individual actions.

### Fixing the Shapes

As previously mentioned, when using time-based motions, the shapes will likely not be correct on your robot. Each robot will have different physical characteristics and different environments that will affect how long it needs to drive or rotate to make the correct shape.

The next section of this guide re-implements the shape `ActionSeries` using sensor based actions instead, however if you do want to fix the time-based shapes the recommended approach is as follows.

1. Deploy the code to your robot
2. Press button 0 (which starts driving a square)
3. Observe how far the robot drives before it starts to rotate. A good distance would be between 2 and 5 feet (this partially depends on how much space you have). If it seems too far, reduce the time for drive actions (arguments to constructor of `DriveTimeAction`). If it seems too short, increase the time. Remember to rebuild (if applicable) and deploy before testing new values.
4. Once you have a good drive distance, it is recommended to use that time for all `DriveTimeActions` in both `ActionSeries` instances.
5. Next, the rotation angle needs to be adjusted for the square. This is relatively simple. Run the action, and allow it to reach the first rotation action. Ideally, it would rotate 90 degrees. It doesn't need to be perfect, just close enough. Increase / decrease the rotation time (argument in constructor of `RotateTimeAction`) until the rotation distance is generally approximately 90 degrees. Remember to rebuild (if applicable) and deploy before testing new values.
6. Use the time determined above for all rotations in the square `ActionSeries`.
7. The triangle rotations will need to be approximately 120 degrees. A good starting point would be to take the time used for rotation actions, multiply it by 4, then divide by 3. Start with this time (round to two decimal places).
8. Build (if applicable) and deploy the program. Test the triangle action (start running this action by pressing button 1 on the gamepad). If the rotate seems approximately correct, leave it as is. If the rotation time seems very wrong, adjust it in the direction it needs to go. The same time should generally be used for all `RotateTimeAction`s used in the triangle `ActionSeries`.
