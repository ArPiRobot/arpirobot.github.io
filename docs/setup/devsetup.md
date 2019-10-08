# Setup for Development

There are several tools used to write ArPiRobot on your computer, deploy code to and configure the robot, and control the robot.

## Writing ArPiRobot Code

ArPiRobot code is written in python using the ArPiRobot python library. There are several tools for writing python code, but the one we will focus on is Visual Studio Code (VSCode).

First, a specific python version must be installed. Multiple versoins of python can be installed on a system, but it is important to make sure that the same version as the robot is using is installed on your computer. The versoin of python used on the robot depends on the Raspberry Pi image. This can be found in the information table for each ArPiRobot Version on the [versions page](../versions.md).

Download the correct version of python from [python.org](https://www.python.org/downloads/). Run the installer. Make sure to add python to the path and to install pip.

Next download and install [Visual Studio Code](https://code.visualstudio.com/). After installing it run it and open the extensions panel (icon on the left side panel). Search for and install the python extension.

Next install the ArPiRobot VSCode Extension. Download the latest version from [GitHub](https://github.com/MB3hel/ArPiRobot-VSCodeExtension/releases). Open a terminal or command prompt in the folder where you downloaded the `.vsix` file for the extension and run 

```
code --install-extension .\arpirobot-0.0.1.vsix
```


## Creating an ArPiRobot project

Open VSCode. Type Ctrl+Shift+P and search for "Create ArPiRobot Project". Run the command. First select the folder to create the project in (the project will be in its own subfolde in the selected folder). Then enter a name for the Project. VSCode will create and open the project folder. The project will consist of one file called `robot.py`. This file contains a simple robot program. Open it so we can make sure the python settings get configured properly.