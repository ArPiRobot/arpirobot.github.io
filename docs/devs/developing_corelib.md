# CoreLib Development

Development of the ArPiRobot-CoreLib can be done using VSCode with the same basic setup as is used for writing robot code. It is recommended to open the `cpp_library` and / or `python_bindings` folder in VSCode.

The `cpp_library` folder contains the CoreLib source. The CoreLib is implemented in C++. The Python version is simply wrappers around a C API. The C API is provided by the `bridge.cpp` and `bridge.h` layer in the `cpp_library`. The `python_bindings` folder contains the actual python code. The same organization structure (files, classes, function names, etc) is used for the python bindings as is used in the C++ library. The `bridge.py` file uses the `ctypes` module to call the C API functions from the `cpp_library`'s bridge. The object oriented API in python is built on calling these functions. As such when implementing a new object / function in C++ it is also necessary to add functions to the bridge layer in both C++ and python. Then, the object / function API must be replicated in the python bindings using the bridge layer.


## Testing Setup
A directory called `testrobot` in the `cpp_library` directory and `testrobot-py` in the `python_bindings` is used to hold a robot program used during development. This directory will be "empty" after cloning the repository. Run `setup-testrobot.py` to setup the test projects from templates. These can then be edited and will have access to all features available in the local development version of the CoreLib and bindings respectively.

When ready to test you will need to be connected to a robot via its WiFi network (or have some network connection to the Robot's Pi).

On the development PC run the `dev-deploy.py` script.in the root of the ArPiRobot-CoreLib folder. This will build the CoreLib and copy the build library, development versions of the python bindings and both testrobot projects to `/home/pi/CoreLib-Test` on the robot.

To run the `testrobot` program  login to the Pi via ssh. You can use OpenSSH (Windows 10 and newer) PuTTY on Windows. Other OSes should include OpenSSH. The commands below are OpenSSH commands which will work with OpenSSH on Windows 10, macOS, Linux, etc.

```sh
ssh -l pi 192.168.10.1
```

Then (in the SSH session after logging in as `pi`) run the following command to stop the deployed robot program (only one program can run at a time)

```sh
sudo systemctl stop arpirobot-program.service
```

Then run either of the following commands to run the C++ testrobot program or the Python testrobot program respectively.

```sh
./start-cpp.sh
./start-py.sh
```