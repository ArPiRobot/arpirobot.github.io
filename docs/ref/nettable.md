
The network table is a way of transferring information between a running robot program and the Drive Station. The network table can be thought of as a set of values shared between the Drive Station and robot program. 


## Network Table Key / Value Pairs

The network table is a collection of key / value pairs. Both keys and values are strings only. Other data types are not supported. A key is a unique identifier. Each key has a value associated with it. In some sense, a key is like a "name" of a "variable" in the network  table. Each key has an associated value. Multiple keys can have the same value.

This creates a "table" like structure. For example, 

| NetTable Key | NetTable Value |
| ------------ | -------------- |
| speed        | 90             |
| direction    | -1             |
| action       | drive          |

Each key's value can be written (set) or read (get) by either the robot program or the Drive Station. Changes on one side are sent to the other side (thus values on the network table are kept in sync).


## Network Table Sync

Before the Drive Station connects to the robot program, the Network Table will be out of sync. It is possible for both the robot and the Drive Station to have key / value pairs before connecting. When connecting the following actions are performed (in this order):

- The robot program sends all key / value pairs to the Drive Station
- The Drive Station updates its network table with the data it received from the robot
- For any keys the Drive Station has that the robot did not (keys that did not get sent previously) the Drive Station will send this set of keys to the robot.
- The robot will update its network table with the data it received from the robot.

In effect, this is a "merge" of the two network tables when a Drive Station connects. If both the robot program and the Drive Station have a key, the robot program's value is kept.


## Network Table Functions

*This set of functions is used in a robot program to manipulate the network table. For information on using the Network Table from the Drive Station, see the [Drive Station Manual](./manuals/drive_station.md#networktable-indicators) or the [Mobile Drive Station Manual](./manuals/android_drive_station.md#network-table-indicators).*

The `ArPiRobot CoreLib` includes a `NetworkTable` class with several static functions to manipulate or access the network table.

=== "Python"
    ```py
    from arpirobot.core.network import NetworkTable
    ```
=== "C++"
    ```cpp
    #include <arpirobot/core/network/NetworkTable.hpp>
    ```

The `NetworkTable` class includes the following functions

- `set(key, value)`: Sets the given key / value pair. If the key did not previously exist, it will be crated (and initialized with the provided value). Both `key` and `value` are strings.
- `get(key)`: Gets the current value of a given key. This function returns the value (a string). If the requested key does not exist, it returns an empty string (`""`).
- `has(key)`: Checks if the network table has a value for the given key. Return a boolean (true / false) indicating if the network table has the given key.
- `changed(key)`: Checks if the given key's value was changed *by the drive station* since the last time `get` was called for that key. This will **not** return true if the robot changes the key's value (ie `set` is called).


## Example Robot Program

=== "Python (`robot.py`)"
    ```py
    from arpirobot.core.robot import BaseRobot
    from arpirobot.core.network import NetworkTable
    from arpirobot.core.log import Logger


    class Robot(BaseRobot):
        def __init__(self):
            super().__init__()
        
        def robot_started(self):
            NetworkTable.set("test1", "")
            NetworkTable.set("test2", "")

        def robot_enabled(self):
            NetworkTable.set("test1", "enabled")

        def robot_disabled(self):
            NetworkTable.set("test1", "disabled")

        def enabled_periodic(self):
            pass

        def disabled_periodic(self):
            pass

        def periodic(self):
            if NetworkTable.changed("test2"):
                Logger.log_info("DS changed test2 to '{}'".format(NetworkTable.get("test2")))
            self.feed_watchdog()
    ```
=== "C++ (`robot.cpp`)"
    ```cpp
    #include <robot.hpp>

    #include <arpirobot/core/log/Logger.hpp>
    #include <arpirobot/core/action/ActionManager.hpp>
    #include <arpirobot/core/network/NetworkTable.hpp>

    using namespace arpirobot;


    void Robot::robotStarted(){
        NetworkTable::set("test1", "");
        NetworkTable::set("test2", "");
    }

    void Robot::robotEnabled(){
        NetworkTable::set("test1", "enabled");
    }

    void Robot::robotDisabled(){
        NetworkTable::set("test1", "disabled");
    }

    void Robot::enabledPeriodic(){

    }

    void Robot::disabledPeriodic(){

    }

    void Robot::periodic(){
        if(NetworkTable::changed("test2")){
            Logger::logInfo("DS changed test2 to '" + NetworkTable::get("test2") + "'");
        }
        feedWatchdog();
    }
    ```