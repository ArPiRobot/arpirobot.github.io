# CoreLib Development

Development of the ArPiRobot-CoreLib can be done using VSCode with the same basic setup as is used for writing robot code. It is recommended to open the `cpp_library` and / or `python_bindings` folder in VSCode.

The `cpp_library` folder contains the CoreLib source. The CoreLib is implemented in C++. The Python version is simply wrappers around a C API. The C API is provided by the `bridge.cpp` and `bridge.hpp` layer in the `cpp_library`. The `python_bindings` folder contains the actual python code. The same organization structure (files, classes, function names, etc) is used for the python bindings as is used in the C++ library. The `bridge.py` file uses the `ctypes` module to call the C API functions from the `cpp_library`'s bridge. The object oriented API in python is built on calling these functions. As such when implementing a new object / function in C++ it is also necessary to add functions to the bridge layer in both C++ and python. Then, the object / function API must be replicated in the python bindings using the bridge layer.

## Testing

### Testing on a Robot

A directory called `testrobot` in the `cpp_library` directory and `testrobot-py` in the `python_bindings` is used to hold a robot program used during development. This directory will be "empty" after cloning the repository. Run `setup-testrobot.py` to setup the test projects from templates. These can then be edited and will have access to all features available in the local development version of the CoreLib and bindings respectively.

When ready to test you will need to be connected to a robot via its WiFi network (or have some network connection to the Robot's Pi).

On the development PC run the `dev-deploy.py` script.in the root of the ArPiRobot-CoreLib folder. This will build the CoreLib and copy the build library, development versions of the python bindings and both testrobot projects to `/home/arpirobot/CoreLib-Test` on the robot.

To run the `testrobot` program  login to the Pi via ssh. You can use OpenSSH (Windows 10 and newer) PuTTY on Windows. Other OSes should include OpenSSH. The commands below are OpenSSH commands which will work with OpenSSH on Windows 10, macOS, Linux, etc.

```sh
ssh -l arpirobot 192.168.10.1
```

Then (in the SSH session after logging in as the user `arpirobot`) run the following command to stop the deployed robot program (only one program can run at a time)

```sh
sudo systemctl stop arpirobot-program.service
```

Change to the directory where the program was deployed

```sh
cd /home/arpirobot/CoreLib-Test/
```

Then run either of the following commands to run the C++ testrobot program or the Python testrobot program respectively.

```sh
./start-cpp.sh
./start-py.sh
```

### Testing on the Development PC

For features that do not require specific hardware, some testing can be done on the development pc itself. This is supported on any system with a GNU toolchain (`gcc` compiler). The CoreLib may build with other toolchains, but are not explicitly supported. On windows, use mingw-w64. On macOS, install gcc using homebrew.

The CoreLib can be built as a standard cmake project using the system's compiler. Run the following in the cloned `ArPiRobot-CoreLib` directory.

**Powershell (Windows):**

```ps1
cd cpp_library
rm -r -Force .\build\
mkdir build
cd build
cmake .. -G "Unix Makefiles"
cmake --build . -j
cd ..\..
```

**Bash / Zsh (macOS & Linux):**

```sh
cd cpp_library
rm -rf build/
mkdir build
cd build
cmake .. -G "Unix Makefiles"
cmake --build . -j
cd ../..
```

After building the CoreLib, the C++ and python testrobot programs can be run using one of the following commands (in the root directory of the corelib repository).

```sh
# Runs C++ test program
python3 run.py cpp

# Runs Python test program
python3 run.py python
```

When the program is running, it will likely not use the same IO provider as on the robot. This limits what devices are supported. Typically, the `SerialIoProvider` will be used. This IO provider only supports UART operations. This provider supports Windows, macOS, and Linux. However, on Linux systems, the `LibsocIoProvider` will likely be used by default (though non-uart devices will still likely not be usable).

This means that most devices (other than Arduino coprocessors) will likely not be usable, however this can still be a way to do some testing during development.



## CoreLib General Structure

TODO

- Robot classes
- Scheduler
- NetworkManager (include network protocol, order of connections DS must make, single client enforcement, etc)
- NetworkTable (include sync procedure)
- ControllerData
- Action API
- Arduinos
- Logging (and how it integrates with networking)
    - Also prints to stdout so ArPiRobot-Tools arpirobot-program service can log to file
- Use of references for statically allocated objects (user must keep in scope) and shared_ptrs for dynamically allocated that the user does not need to manage the lifecycle of. Also document why some places such as BaseDevice::action and ArduinoDevice::arduino use raw pointers (these should not keep what they point to alive and the object that sets these vars clears these vars on its deletion).
- Document bridge lifecycle management
    - Bridge creates objects using make_shared and keeps a map of raw ptr to shared_ptr
    - Bridge returns raw ptr. When passed a raw ptr it looks up the shared_ptr and passes that to c++ functions. The c++ funcs may keep a copy of the shared_ptr to keep objects in scope even if the bridge's calling language (eg python) has its object leave scope and call destroy. Destroy just removes the shared_ptr copy from the bridge's map. If a copy is held elsewhere the object is not deallocated. If it is not held elsewhere, the object is deallocated immediately. BaseRobot does not use this method as the robot is started using its own blocking start method. No need to handle scenarios where a reference is not kept by user code for the duration of the program. Additionally, the bridge calling language must make sure objects with callbacks (eg Actions) are not cleaned up in the calling language (eg python) while C++ may call functions that are members (pointers to member functions). To this end, Actions are always kept in scope once instantiated by the python bindings.

## CoreLib Devices

TODO

**Devices**

Things controlled entirely by the Pi.

- Gamepad
- INA260PowerSensor
- AdafruitMotorHatMotor
- DRV8833Motor and DRV8833Module
- TB6612Motor and TB6612Module
- L298NMotor and L298NModule

**ArduinoDevices**

Things controlled by the arduino that provide data to the Pi.

## CoreLib Arduino Interface and Communication Protocol
- Document general communication protocol, start byte, escape byte, stop byte
- CRC16, intent behind it, what happens if the Pi gets invalid data
- Default settings for the firmware
- Why not use arduino for motors: no ACK of message receipt so robot won't know state reliably
