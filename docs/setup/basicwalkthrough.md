# Basic Walkthrough

## Update Development Environment and Robot

### Updating Development Environment
The first thing to do after setting up the development environment is to update the ArPiRobot python library. *This version should always be the same version as is installed on the robot*. Download the latest update from the [downloads page](../downloads.md). The update will be a `.zip` file, which contains multiple `.zip` files and an install script. Only one part of the update is necessary for the development environment - the python library (called `pythonlib.zip` in the update zip). To install it in the development environment extract the `pythonlib.zip` file from the update zip. Then extract the `pythonlib.zip` folder called `pythonlib`. Then run the update script (`update.bat` or `update.sh`).


### Apply Update To Robot
The ArPiRobot Deploy Tool is used to apply an update to the robot. Run the deploy tool, connect to the robot, select the update tab, choose the update `.zip` file. Finally apply the update and wait for it to complete.


## Create a Project
The development environment's workspace contains a folder called `project_template`. Copy this folder and rename it (make sure it is still in the `workspace` folder). This new folder is the project folder. It contains a sample `robot.py` with a basic robot class implemented and a hidden folder called `.vscode` with configurations for the project in VSCode.


## Writing Code
To open a project launch VSCode, select File, Open Folder. Choose a project folder inside the development environment's `workspace` folder.


## Using Deploy tool 
The deploy tool works by connecting to the robot via SSH (over the network). The robot generates its own WiFi network. Conect to this network (default SSID="ArPiRobot-Robot", default password="arpirobot123"). Open the Deploy Tool and click the connect button. If asked about an SSH key, approve it. Once connected the other tabs at the top of the window will be visible.

### Deploy Code To Robot
The `Robot Code` tab contains two file selectors. The first, labeled "Project Folder" is the folder containing the robot code. Select one of the project folders in the development environment's `workspace` folder. The "Main Script" selector is used to choose a single file (in the project folder) that should be run on the robot. This is the python file that is the "main" python script. In a default project, containing only one python script (`robot.py`), `robot.py` is the main script.

### Pulling Logs
The deploy tool can be used to pull a "launch log" from the robot. This log file will (mostly) contain the same information as is visible in the Drive Station's robot log viewer (see the drive station section later), but there are times where the drive station cannot be used to read logs. The drive station connects to a running robot program, not to the robot itself as the deploy tool does. Thus, if the robot code crashes the drive station cannot connect, but the log will exist and the deploy tool can load it by using the button in the "Other" panel.

### Configuring WiFi
The Robot generates a WiFi network (as mentioned earlier). The SSID and password can be changed in the WiFi panel of the Deploy Tool. The robot can also (at the same time) connect to an existing WiFi network. The network to connect to can be configured in the same panel. If connected to another network with internet access the robot's network will also have internet access.

### Other Useful Controls
In the other panel there is more than just pulling a log. The robot's hostname can be changed and there are buttons along the bottom to do several things to the robot.

## Using Drive Station
Make sure your PC is connected to the Robot's WiFi network. Run the drive station. First, click File > Settings. The robot address should not change (unless using an external network to connect to the robot). The "Main Battery Voltage" is the voltage of the motor batteies on the robot. This is 1.5V per AA so a robot with 4 AA batteries is 6V. This may need to change based on your robot. These settings are saved and will only need to chagne once. Note that it is not required to change this voltage in the settings, but it determines the colors of the battery indicator on the bottom right corner of the drive station.

### Gamepads
Make sure the "Controllers" tab is selected.

Gamepads connected to the PC will be sown in a list on the left side of the drive station. Selecting the checkbox beside a controller enables it (meaning the robot will get data from it). Clicking on the name of the controller will highlight it. While highlighted, a controller's status will be sown in the indicators to the right of the controller list (still under the controller tab). The axis, button, and dpad indicators show the numbers and positions of different axes, buttons, and the dpad.


### Log Panels
Beside the "Controllers" panel there are two log panels. "Drive Station Log" and "Robot Log" show two different logs. The drive station's log will show information about drive station problems and what the drive station is doing. The robot log will, while the drive station is connected to the robot, show the robot's log.


### Robot Control Buttons
At the bottom of the drive station there are three buttons. The first connects to or disconnects from the robot. Once connected the enable and disable buttons enable or disable the robot. Note that disconnecting form the robot automatically disables it (this is done by the robot, so this is true even if the disconnect occurs unintentionally).


### Indicators Panel
The top two thirds of the drive station appear to be empty, however indicators can be added to read or change network table values on the robot. The "Add" menu in the toolbar at the top is used to do this. Under the "Existing Keys" any network table keys on the robot will be sown. A new indicator can also be created (this will create a key on the robot). Indicators can be dragged around and resized in the indicators panel. The "File" menu also contains several options that deal with the indicators panel. Clear removes all indicators. Save (or save as) save the layout of indicators as a `.json` file. This file can be loaded the next time the drive station is run with the Open menu option. Finally, unchecking the "Editable" option fixes the size and position of indicators. Each indicator also has a few options. Right clicking on the label (key) will show them: Delete (this one is obvious) and Editable (this one makes it so the value in the indicator can be changed by the drive station).

