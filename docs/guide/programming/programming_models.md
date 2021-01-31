# Programming Models

## Periodic Robot Model
The periodic programming model uses certain functions that run every so often. For example, a function may run every 50 milliseconds (20 times per second) while the robot is enabled. There are three of these functions in the `BaseRobot` class.

In these functions data from devices (such as gamepads) can be polled and used for various purposes such as driving the robot.

| BaseRobot Function (Python) | BaseRobot Function (C++/Java)     | When it runs            | Default period |
| --------------------------- | -----------------------------     | ----------------------- | -------------- |
| periodic                    | periodic                          | Any robot state         | 50ms           |
| enabled_periodic            | enabledPeriodic                   | When robot is enabled   | 50ms           |
| disabled_periodic           | disabledPeriodic                  | When robot is disabled  | 50ms           |

**Example**

```c++ fct_label="C++ Header (.hpp)"
#pragma once

#include <arpirobot/core/robot.hpp>
#include <arpirobot/devices/gamepad.hpp>

using namespace arpirobot;

class Robot : public BaseRobot{
public:
    void robotStarted();

    void robotEnabled();

    void robotDisabled();

    void enabledPeriodic();

    void disabledPeriodic();

    void periodic();

    Gamepad gamepad {0};
};
```

```c++ fct_label="C++ Source (.cpp)"
#include <robot.hpp>

using namespace arpirobot;


void Robot::robotStarted(){
}

void Robot::robotEnabled(){

}

void Robot::robotDisabled(){

}

void Robot::enabledPeriodic(){
    double speed = gamepad.getAxis(1);
    double rotation = gamepad.getAxis(2);
    // Use speed and rotation to move motors
}

void Robot::disabledPeriodic(){

}

void Robot::periodic(){
    feedWatchdog();
}
```

```python
from arpirobot.core.robot import BaseRobot
from arpirobot.devices.gamepad import Gamepad


class Robot(BaseRobot):
    def __init__(self):
        super().__init__()
        self.gamepad = Gamepad(0)

    def robot_started(self):
        pass

    def robot_enabled(self):
        pass

    def robot_disabled(self):
        pass

    def periodic(self):
        self.feed_watchdog()

    def enabled_periodic(self):
        speed = self.gamepad.get_axis(1)
        rotation = self.gamepad.get_axis(2)
        # Use speed and rotation to move motors

    def disabled_periodic(self):
        pass 
```

```java
import arpirobot.core.robot.BaseRobot;
import arpirobot.devices.gamepad.Gamepad;

public class Robot extends BaseRobot {
    public Gamepad gamepad = new Gamepad(0);

    @Override
    protected void robotStarted() {

    }

    @Override
    protected void robotEnabled() {

    }

    @Override
    protected void robotDisabled() {

    }

    @Override
    protected void periodic() {
        feedWatchdog();
    }

    @Override
    protected void enabledPeriodic() {
        double speed = gamepad.getAxis(1);
        double rotation = gamepad.getAxis(2);
        // Use speed and rotation to move motors
    }

    @Override
    protected void disabledPeriodic() {

    }
}
```

## Action-Based Model
The action based model is similar to the periodic model in that it relies on functions that run once per a period of time. However, actions are self-contained objects that can be started and stopped independently of each other. Each action has four functions that must be implemented:

| Function (Python) |  Function (C++/Java) | When it runs                      | Purpose                                    |
| ----------------- | -----------------| --------------------------------- | ------------------------------------------ |
| begin()           | begin()          | When an action is first started   | Allow actions to lock devices (see below) and perform actions when they are first started |
| process()         | process()        | Runs once every period (default is 50ms) | Allows the action to poll devices/sensors and use their devices (ex. driving motors) |
| should_continue() | shouldContinue() | Each time after process()        | Return false from this function when the Action is done and should be stopped. |
| finish(interrupted) | finish(interrupted) | When an action is stopped      | Allow actions to perform some action upon ending. interrupted is False if the action was stopped because `should_continue` returned False. Otherwise it is True indicating that the action was stopped before completion. |

An action can take "control" of a device, which will prevent other actions from using it. An action can do so by using `Action`'s `lock_device` / `lockDevice` with the device to lock as it's argument. There is also a `lock_devices` / `lockDevices` method that accepts an array/list of devices (to lock multiple in one line of code). Devices should be locked by an action in it's `beign` method. If an action locks a device that is already locked by another action the *older* action will be stopped and the new action will take control of the device. When this happens the old action's `finish` method runs before `lock_device` for the new action returns. It is important to note that locking a device *only* affects other actions and does *not* prevent code outside of an action (such as in a Robot class's periodic functions) from using a device.

An unlimited number of `Action`s can be running at the same time, however there can never be two actions that lock the same device running at the same time.

When creating an action you create a class that inherits form the `Action` class.

#### Example Action

```cpp fct_label="C++ Header (.hpp)"
#include <arpirobot/core/action.hpp>
#include <main.hpp>

using namespace arpirobot;

class DriveAction : public Action {
protected:
    void begin();

    void process();

    void finish(bool wasInterrupted);

    bool shouldContinue();
};

```

```cpp fct_label="C++ Source (.cpp)"
#include <actions.hpp>

void DriveAction::begin(){
    // Lock any devices this action will use
    lockDevice(Main::robot->flmotor);
    lockDevice(Main::robot->rlmotor);
    lockDevice(Main::robot->frmotor);
    lockDevice(Main::robot->rrmotor);
}

void DriveAction::process(){
    // Get the values from the gamepad using a deadband
    double speed = -1 * Main::robot->gamepad.getAxis(1, 0.1);
    double rotation = Main::robot->gamepad.getAxis(2, 0.1);

    // Use the drive helper to set the motor speeds
    Main::robot->driveHelper.update(speed, rotation);
}

void DriveAction::finish(bool wasInterrupted){
    // Stop the motors when this action finishes
    Main::robot->driveHelper.update(0, 0);
}

bool DriveAction::shouldContinue(){
    // This action should continue running until interrupted
    return true;
}

```

```python
from arpirobot.core.action import Action
import main

class DriveAction(Action):
    def begin(self):
        # Lock any devices this action will use
        self.lock_device(main.robot.flmotor)
        self.lock_device(main.robot.rlmotor)
        self.lock_device(main.robot.frmotor)
        self.lock_device(main.robot.rrmotor)
    
    def process(self):
        # Get the values from the gamepad using a deadband
        speed = -1 * main.robot.gamepad.get_axis(1, 0.1)
        rotation = main.robot.gamepad.get_axis(2, 0.1)

        # Use the drive helper to set the motor speeds
        main.robot.drive_helper.update(speed, rotation)

    def finish(self, was_interrupted: bool):
        # Stop the motors when this action finishes
        robot_objects.drive_helper.update(0, 0)

    def should_continue(self) -> bool:
        # This action should continue running until interrupted
        return True
```

```java
import arpirobot.core.action.Action;

public class DriveAction extends Action{
    @Override
    protected void begin() {
        // Lock any devices this action will use
        lockDevice(Main.robot.flmotor);
        lockDevice(Main.robot.rlmotor);
        lockDevice(Main.robot.frmotor);
        lockDevice(Main.robot.rrmotor);
    }

    @Override
    protected void process() {
        // Get the values from the gamepad using a deadband
        double speed = -1 * Main.robot.gamepad.getAxis(1, 0.1);
        double rotation = Main.robot.gamepad.getAxis(2, 0.1);

        // Use the drive helper to set the motor speeds
        Main.robot.driveHelper.update(speed, rotation);
    }

    @Override
    protected void finish(boolean wasInterrupted) {
        // Stop the motors when this action finishes
        Main.robot.driveHelper.update(0, 0);
    }

    @Override
    protected boolean shouldContinue() {
        // This action should continue running until interrupted
        return true;
    }
}
```

#### ActionManager

The action manager is used to start and stop actions. Stopping an action this way is rarely necessary as actions can stop themselves when finished (by using the `should_continue` function) or will be stopped automatically if a newer action locks a device the action is using. Instances of an action class can be reused, meaning after starting then stopping `action` in the example below `action` could be restarted by using `ActionManager.start_action` again (in Java this is `ActionManager.startAction`). If you try to start an already running action nothing will change (the action does **not** restart).

```c++
#include <arpirobot/core/action.hpp>

using namespace arpirobot;

// Somewhere in your code
Action *action = new Action();
ActionManager::startAction(action);
std::this_thread::sleep_for(std::chrono::seconds(1));
```

```python
from arpirobot.core.action import ActionManager
import time

# Somewhere in your code...
action = MyAction()
ActionManager.start_action(action)
time.sleep(1)
ActionManager.stop_action(action)
```

```java
import arpirobot.core.action.ActionManager;

// Somewhere in your code...
MyAction action = new MyAction();
ActionManager.startAction(action);
try{ Thread.sleep(1000); }catch(InterruptedException ignored){}
ActionManager.stopAction(action);
```

#### ActionSeries

Sometimes it's useful to run a set of actions one after the other. An action series can be used to do just that. It works just like a normal action so you can use `ActionManger` to start/stop an ActionSeries.

When creating an action series the constructor takes two arguments: 
1. A list of actions to run one after the other
2. An action to run once the action series finishes

The difference between the finished action (argument #2) and other actions is that the ActionSeries will show it's status as running when running any of the actions in the list (argument #1) whereas the finished action (argument #2) will just be started by the ActionSeries after it ends. This is useful to transfer control to a different action series or to an action that runs indefinitely such as the `DriveAction` in the example above so that the action series could be restarted (remember that ActionManager.start_action does nothing to an already running action).

```c++
// Will run a DriveForward(10) action then a DriveReverse(10) action. 
// Then the action series is stopped.
// After it is stopped a DriveAction is started.
ActionSeries *routine = new ActionSeries(
    {new DriveForward(10), new DriveReverse(10)},
    new DriveAction()
);

ActionManager.startAction(routine);
```

```python
# Will run a DriveForward(10) action then a DriveReverse(10) action. 
# Then the action series is stopped.
# After it is stopped a DriveAction is started.
routine = ActionSeries([DriveForward(10), DriveReverse(10)], DriveAction())

ActionManager.start_action(routine)
```

```java
// Will run a DriveForward(10) action then a DriveReverse(10) action. 
// Then the action series is stopped.
// After it is stopped a DriveAction is started.
ActionSeries routine = new ActionSeries(
    new Action[]{new DriveForward(10), new DriveReverse(10)},
    new DriveAction()
);

ActionManager.startAction(routine);
```

## Mixing of Programming Models
It is possible to mix proramming models and it is often useful to do so. For example on may use a periodic model to handle driving the robot and watch for gamepad button presses. When a button is pressed control may be handed over to an action.

The biggest thing to be careful of is that you never have two sections of code using a device (eg. a motor) at the same time. For example, if an action is started to control the motors you need to make sure that periodic code does not also attempt to control the motors while an action is running.
