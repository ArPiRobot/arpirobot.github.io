# Setup for Development

This guide documents how to install all required software and libraries to develop ArPiRobot code your computer.

## Install the Basic Development Environment
The basic development environment can be downloaded from the [downloads page](../downloads.md). The development environment includes a python install and workspace folder to contain Visual Studio Code projects, as well as a script to update ArPiRobot libraries for the development environment. Once the package has been downloaded extract the zip file. There will be a folder called `arpirobot-dev`. This folder contains the development environment. The development environment will contain several folders and files.

`python##` - This is the ArPiRobot-specific python distribution used by ArPiRobot projects. The version will be a two digit number without decimal point (for example python 3.5.4 would be `python35`).

`workspace` - This folder should contain project folders for each ArPiRobot project in the workspace.

`update.bat or update.sh` - A windows batch or unix bash script to update the python library version installed in the ArPiRobot python distribution.


## Install and Configure Visual Studio Code
Download [Visual Studio Code](https://code.visualstudio.com/) and install it. Launch Visual Studio Code (VSCode). On the left side bar select the extensions icon (5th from the top). Search for "python" (without the quotes). Select Microsoft's Python extension and install it.


## Install the Drive Station
Download the ArPiRobot Drive Station from the [downloads page](../downloads.md). For windows download and run the `.exe` installer. For macOS extract the `.zip` file and move the `.app` file to `/Applications`. For Linux (or *BSD or other systems where Java 11+ is installed) download the `.jar`.


## Install the Deploy Tool
Download the ArPiRobot Deploy Tool from the [downloads page](../downloads.md). For windows download and run the `.exe` installer. For macOS extract the `.zip` file and move the `.app` file to `/Applications`. For Linux (or *BSD or other systems where Java 11+ is installed) download the `.jar`.



## Next Steps

It is recommended to complete the [Basic Walkthrough](basicwalkthrough.md). At the very least make sure to download the latest ArPiRobot update package and apply it to the robot and to the development environment.