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

TODO: Explain locking devices and how that works (note specifically that it interrupts any previous owner)

TODO: Example code locking devices

TODO: Explain why this is beneficial

TODO: Explain that you must be careful with using periodic code if an action may lock the device you are using

TODO: Example code of an issue and show use of isLocked method to address in some cases



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
