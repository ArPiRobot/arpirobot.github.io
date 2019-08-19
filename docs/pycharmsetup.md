# Setup for Development

This guide documents how to install all required software and libraries to develop ArPiRobot code on a PC using PyCharm.
Currently this guide is written only for windows. All of the software is cross platform, but the guide may not line up perfectly.

## Install Development Tools

### Install Python

It is recommended to install the same version of python used by the robot image (currently python 3.5). Download the latest version (3.5.4) from [here](https://www.python.org/downloads/release/python-354/). Check "Add Python 3.5 to PATH" then choose "Install Now".

### Install Python Libraries

In order to get proper error messages while writing code on your PC it is necessary to install the same libraries that are used on the robot. To do so download the zip for the ArPiRobot-PythonLib [repository](https://github.com/MB3hel/ArPiRobot-PythonLib). Extract the zip and open a cmd or powershell window in the folder (as admin if python was installed for all users). Run the following commands

```
pip install -r requirements.txt
python setup.py install
```

### Install PyCharm IDE

Downalod [PyCharm Community Edition](https://www.jetbrains.com/pycharm/download/). Run the installer and install the program.

## Create a New Project

TODO

## Open an Existing Project

For example projects clone the ArPiRobot-Robots repository. It is one large project. Each subfolder contains multiple different programs for one of my example robot builds.

Choose the folder for the project to open in PyCharm. Next configure a python interpreter. Open the File menu and choose Settings. Go down to the Project: *PROJECT_NAME* tab and select Project interpreter. Beside the dropdown click the gear icon and choose add. Choose System Interpreter and select the version of python that was just installed.

## Deploying Code to the Robot

Robot programs cannot generally be tested on a PC. They must be tested on the robot itself. The [Deploy Tool](https://github.com/MB3hel/ArPiRobot-DeployTool/releases) is used to deploy code to the robot running an ArPiRobot image. Run the tool (Note: if using the jar file java 11 must be installed from https://adoptopenjdk.net/). After launching the deploy tool connect to the robot's wifi network. Click the connect button in the deploy tool. Once connected the other tabs at the top will be usable. Choose the Robot Code tab. Select the folder the python program is in for the project folder. Select a python program in that folder as the main script (the main script is the program that will be run on the robot). Click deploy and the code should run on the robot. After about 3-5 seconds (for the Pi 3A+) or 10-15 seconds (Pi Zero W) the program should be running and the [Drive Station](https://github.com/MB3hel/ArPiRobot-DriveStation/releases) can be connected.

The Deploy tool can also be used to pull a program launch log (other tab). If the drive station cannot connect within 30 seconds the program almost certainly crashed. Look at the error output in the launch log. Most of the time when a program crashes it is because the libraries on the Pi are not up to date. To update the libraries download the latest update zip (NOTE THESE DO NOT EXIST YET) and install the zip as an update using the update tab of the Deploy Tool.