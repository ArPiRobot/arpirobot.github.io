
`BaseRobot` is the foundation of any ArPiRobot program. Each program must have exactly one instance of a child class of `BaseRobot` (the child class is generally called `Robot`). The functions in this child class are used to define the robot's behavior.

## Overridden Functions

The following functions must be overridden in the child class. These are run at different times and are used to define a robot's behavior. Their use will be explained in more detail later.

=== "Python"
    ```py
    def robot_started(self):
        pass
    
    def robot_enabled(self):
        pass

    def robot_disabled(self):
        pass
    
    def enabled_periodic(self):
        pass
    
    def disabled_periodic(self):
        pass
    
    def periodic(self):
        pass
    ```

=== "C++"
    ```cpp
    void robotStarted();

    void robotEnabled();

    void robotDisabled();

    void enabledPeriodic();

    void disabledPeriodic();

    void periodic();
    ```

    There are two types of functions listed above. The `periodic` functions run once every *x* milliseconds. By default this is configured for 50 milliseconds. The other functions run when some specific event occurs.

## Robot States

A robot has two basic states. The state is tracked by the underlying `BaseRobot` implementation. When stared, a robot is in the "Disabled" state. In this state the robot is unable to perform any "potentially dangerous" actions and will just sit idle. The other state is the "Enabled" state. In this state the robot is able to perform tasks.

While disabled some devices will become disabled. For example, motor controllers will become disabled when the robot is disabled and become enabled when the robot is enabled. While a motor controller is disabled, it can be configured but is unable to move its motor. Once the robot is enabled (and the motor controller becomes enabled) the motor controller is able to move its motor.

In addition to controlling the state of "potentially dangerous" devices, the state of the robot is useful for defining core robot functionality. Some of the functions listed above run only if the robot is in a specific state. Others run regardless of state.
- `robot_started` / `robotStarted` runs when the robot program is started. This will always occur while the robot is disabled. This function will only ever run once.
- `robot_enabled` / `robotEnabled` runs when the robot switches from the disabled state to the enabled state.
- `robot_disabled` / `robotDisabled` runs when the robot switches from the enabled state to the disabled state. It also runs after `robot_started` / `robotStarted` runs when the program first starts (as the robot first becomes disabled on startup).
- There are also three periodic functions. The generic `periodic` function will run regardless of state. The `enabled_periodic` / `enabledPeriodic` and `disabled_periodic` / `disabledPeriodic` functions run only in the corresponding state.
- This makes `enabledPeriodic` a common location for the bulk of the robot program's logic (unless the Action system is used as it commonly is for more complex robot programs).

Once a robot program is running its state is only able to be changed by the Drive Station. After the Drive Station connects to  a running robot program it is able to change the state of the program. The "Enable" and "Disable" buttons in the Drive Station UI send commands to the robot to change its state. Furthermore, if a drive station becomes disconnected the robot program will detect this and automatically disable itself. *This behavior is not intended to be user modifiable, however if a fully autonomous robot is required it is trivial to implement a "virtual Drive Station" that connects to the robot program and sends an enabled command. This "virtual Drive Station" could even be part of the robot program itself. The network commination protocol is documented in the comments in the CoreLib's NetworkManager source files.*


## Watchdog

In addition to robot states there is one more feature to ensure safe operation of the robot, the watchdog. The watchdog is used to automatically disable "potentially dangerous" devices if the robot program is frozen or running too slowly to safely manage devices.

If the watchdog is not "fed" for 500&ast; milliseconds, all "potentially dangerous" devices become disabled. They will be automatically re-enabled when the watchdog is next "fed".

This helps avoid scenarios where the CPU is overloaded and the robot is unable to respond to inputs in time to avoid bad scenarios. Additionally, if user code ever causes the robot program to freeze (eg infinite loops) this ensures the motors will be able to be stopped without relaunching the robot program to take control back of the motors.


&ast;Currently this duration is not configurable, however the ability to change this will be introduced soon.


The watchdog should be fed in the `periodic` function by calling the `feed_watchdog` / `feedWatchdog` function (member of `BaseRobot`). 

The `periodic` functions are run by an internal scheduler thread pool, the same thread pool which runs user code such as actions. Feeding the watchdog in `periodic` ensures that if user code ever slows down the program too much (which also slows down `periodic`) devices are disabled. Note that the watchdog is not run on the scheduler and thus is not directly able to be delayed by user code. The watchdog runs on the program's main thread in an infinite loop with a sleep to keep CPU usage from being too high. This sleep is short enough, however, that the task should not be slowed down too much by kernel scheduling.


## RobotProfile

The `RobotProfile` is a class containing only static members. The variables are used to configure various settings about how the robot is run including:

- How many threads are in the main scheduler's thread pool (used for running actions, periodic functions, etc). Defaults to 10.
- How frequently periodic functions should run (in milliseconds). Defaults to 50.
- How old gamepad data is allowed to be (in milliseconds). Data older than this age will be discarded (this prevents scenarios where network slowdowns prevent the robot from being controllable). Defaults to 100.
- How frequently action's periodic functions are run (in milliseconds). Defaults to 50.
- Which IO provider is used when the program runs. An IO provider is an underlying library which allows using GPIO pins and communication busses. Which ones are available / used by default is platform specific. Setting this to an empty string will use the default provider for the platform.
