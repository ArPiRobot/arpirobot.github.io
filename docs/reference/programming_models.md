# Programming Models

## Periodic Robot Model
The periodic programming model uses certain functions that run every so often. For example, a function may run every 50 milliseconds (20 times per second) while the robot is enabled. There are three of these functions in the `BaseRobot` class.

In these functions data from devices (such as gamepads) can be polled and used for various purposes such as driving the robot.

| BaseRobot function     | When it runs            | Default period (runs once each period) |
| ---------------------- | ----------------------- | -------------------------------------- |
| periodic               | Any robot state         | 50ms                                   |
| enabled_periodic       | When robot is enabled   | 50ms                                   |
| disabled_periodic      | When robot is disabled  | 50ms                                   |

**Example**

```python3
from testrobot.robot_objects import gamepad
from arpirobot.core.logger_internal import log_info
from arpirobot.core.robot import BaseRobot
from arpirobot.core import event
from arpirobot.devices.gamepad import Gamepad

# Enable the event system
event.enable_event_system = True

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

if __name__ == "__main__":
    robot = Robot()
    robot.start()
```

## Action-Based Model
The action based model is similar to the periodic model in that it relies on functions that run once per a period of time. However, actions are self-contained objects that can be started and stopped independently of each other. Each action has four functions that must be implemented:

| Function         | When it runs                      | Purpose                                    |
| ---------------- | --------------------------------- | ------------------------------------------ |
| begin()          | When an action is first started   | Allow actions to lock devices (see below) and perform actions when they are first started |
| process()        | Runs once every period (default is 50ms) | Allows the action to poll devices/sensors and use their devices (ex. driving motors) |
| should_continue() | Each time after process()        | Return False from this function when the Action is done and should be stopped. |
| finish(interrupted) | When an action is stopped      | Allow actions to perform some action upon ending. interrupted is False if the action was stopped because `should_continue` returned False. Otherwise it is True indicating that the action was stopped before completion. |

An action can take "control" of a device, which will prevent other actions from using it. An action can do so by using `Action`'s `lock_device(device)` method with the device to lock as it's argument. Devices should be locked by an action in it's `beign` method. If an action locks a device that is already locked by another action the *older* action will be stopped and the new action will take control of the device. When this happens the old action's `finish` method runs before `lock_device` for the new action returns. It is important to note that locking a device *only* affects other actions and does *not* prevent code outside of an action (such as in a Robot class's `enabled_periodic`) method from using a device.

An unlimited number of `Action`s can be running at the same time, however there can never be two actions that lock the same device running at the same time.

When creating an action you create a class that inherits form the `Action` class.

**Example Action**

```python3
from arpirobot.core.actions import Action
from robot_objects import *

class DriveAction(Action):
    def begin(self):
        # Lock any devices this action will use
        self.lock_device(flmotor)
        self.lock_device(rlmotor)
        self.lock_device(frmotor)
        self.lock_device(rrmotor)
    
    def process(self):
        # Get the values from the gamepad using a deadband
        speed = -1 * gamepad.get_axis(DRIVE_AXIS, AXIS_DEADBAND)
        rotation = gamepad.get_axis(ROTATION_AXIS, AXIS_DEADBAND)

        # Use the drive helper to set the motor speeds
        drive_helper.update(speed, rotation)

    def finish(self, was_interrupted: bool):
        # Stop the motors when this action finishes
        drive_helper.update(0, 0)

    def should_continue(self) -> bool:
        # This action should continue running until interrupted
        return True
```

**ActionManager**

The action manager is used to start and stop actions. Stopping an action this way is rarely necessary as actions can stop themselves when finished (by using the `should_continue` function) or will be stopped automatically if a newer action locks a device the action is using. Instances of an action class can be reused, meaning after starting then stopping `action` in the example below `action` could be restarted by using `ActionManager.start_action` again. If you try to start an already running action nothing will change (the action does **not** restart).

```python3
from arpirobot.core.actions import ActionManager
action = MyAction()
ActionManager.start_action(action)
ActionManager.stop_action(action)
```

**ActionSeries**

Sometimes it's useful to run a set of actions one after the other. An action series can be used to do just that. It works just like a normal action so you can use `ActionManger` to start/stop an ActionSeries.

When creating an action series the constructor takes two arguments: 
1. A list of actions to run one after the other
2. An action to run once the action series finishes

The difference between the finished action (argument #2) and other actions is that the ActionSeries will show it's status as running when running any of the actions in the list (argument #1) whereas the finished action (argument #2) will just be started by the ActionSeries after it ends. This is useful to transfer control to a different action series or to an action that runs indefinitely such as the `DriveAction` in the example above so that the action series could be restarted (remember that ActionManager.start_action does nothing to an already running action).

```python3
# Will run a DriveForward(10) action then a DriveReverse(10) action. Then the action series is stopped.
# After it is stopped a DriveAction is started.
routine = ActionSeries([DriveForward(10), DriveReverse(10)], DriveAction())

ActionManager.start_action(routine)
```

## Event-Based Model
*This model is a work in progress and may be drastically changed and/or removed at any point in time. It is also currently disable by default and must be enabled before use.*

The event based model relies on running a function when something occurs or some value changes. For example a function can be run when a gamepad axis is moved or when a button's state changes. 

All event handler callback functions are run asynchronously to prevent user code from being able to slow down core functionality, however this means that this model does add significant overhead on single-core systems (such as the Raspberry Pi Zero).

Event handler callbacks are added to specific device objects (such as Gamepads) by using `on_` functions. The event callback must be a function that takes one argument: the event object containing information about the event. Each event type has a different `Event` subclass with information about the specific type of event.

**Events Information**

| Event Type               | Object Type         | Callback Registration Function                  | Event Class                |
| ------------------------ | ------------------- | ----------------------------------------------- | -------------------------- |
| Gamepad Axis Moved       | Gamepad             | Gamepad.on_axis_moved(axis_num, callback)       | Gamepad.AxisMovedEvent     |
| Gamepad Button Changed   | Gamepad             | Gamepad.on_button_changed(button_num, callback) | Gamepad.ButtonChangedEvent |
| Gamepad Dpad Changed     | Gamepad             | Gamepad.on_dpad_changed(dpad_num, callback)     | Gamepad.DpadChangedEvent   |
| Arduino Ready            | ArduinoInterface    | ArduinoInterface.on_arduino_ready(callback)     | ArduinoInterface.ArduinoReadyEvent |
| Arduino Communication Lost | ArduinoInterface  | ArduinoInterface.on_communication_lost(callback) | ArduinoInterface.CommunicationLossEvent |


**Example**

```python3
from arpirobot.core.logger_internal import log_info
from arpirobot.core.robot import BaseRobot
from arpirobot.core import event
from arpirobot.devices.gamepad import Gamepad

# Enable the event system
event.enable_event_system = True

class Robot(BaseRobot):
    def __init__(self):
        super().__init__()
        self.gamepad = Gamepad(0)

    def robot_started(self):
        # Run the drive function when axis 0 moves on this gamepad
        self.gamepad.on_axis_moved(0, self.drive)

    def robot_enabled(self):
        pass

    def robot_disabled(self):
        pass

    def periodic(self):
        self.feed_watchdog()

    def enabled_periodic(self):
        pass

    def disabled_periodic(self):
        pass 

    def drive(self, event: Gamepad.AxisMovedEvent):
        log_info("Axis moved!")

if __name__ == "__main__":
    robot = Robot()
    robot.start()

```

## Mixing of Programming Models
It is possible to mix proramming models and it is often useful to do so. For example on may use an event based or periodic model to handle driving the robot and watch for gamepad button presses. When a button is pressed control may be handed over to an action.

The biggest thing to be careful of is that you never have two sections of code using a device (eg. a motor) at the same time. For example, if an action is started to control the motors you need to make sure that periodic or event based code does not also attempt to control the motors while an action is running.

## Project Structure and Conventions
The structure of a typical ArPiRobot project consists of three files:
- `robot_objects.py`
- `robot_actions.py`
- `robot.py`

The `robot_objects.py` file is where devices and settings (such as constants) should be defined. 

```python3
from arpirobot.devices.adafruitmotorhat import AdafruitMotorHatMotor

# Constants
DRIVE_AXIS = 1
AXIS_DEADBAND = 0.1

motor = AdafruitMotorHatMotor(1)
```

The `robot_actions.py` file is where custom `Action`s should be defined (if using `Action`s). Devices can be accessed by importing everything form robot_objects.

```python3
from arpirobot.core.actions import Action
from robot_objects import *

class DriveAction(Action):
    def begin(self):
        # Lock any devices this action will use
        self.lock_device(motor)
    
    def process(self):
        speed = gamepad.get_axis(DRIVE_AXIS, AXIS_DEADBAND)
        motor.speed = speed

    def finish(self, was_interrupted: bool):
        motor.speed = 0

    def should_continue(self) -> bool:
        # This action should continue running until interrupted
        return True
```

The `robot.py` file is where the Robot class is defined, instantiated, and started. Devices (sensors, motors, etc) should be configured in `robot_started` and can be accessed by importing everything form robot_objects. Actions can be accessed and started by importing them from robot_actions.

```python3
from arpirobot.core.robot import BaseRobot
from robot_objects import *

class Robot(BaseRobot):
    def __init__(self):
        super().__init__()

    def robot_started(self):
        motor.inverted = True
        motor.brake_mode = True

    def robot_enabled(self):
        pass

    def robot_disabled(self):
        pass

    def periodic(self):
        # If this method is not run often enough motors will be disabled
        self.feed_watchdog()

    def enabled_periodic(self):
        pass

    def disabled_periodic(self):
        pass 

# Create an instance of Robot and start it.
if __name__ == "__main__":
    robot = Robot()
    robot.start()
```
