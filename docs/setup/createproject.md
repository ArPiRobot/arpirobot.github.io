# Create a Simple Robot Project

## Using Visual Studio Code
If the ArPiRobot Visual Studio Code extension is installed it is easy to create an ArPiRobot project. Open the command pallete (the default shortcut for this is `Ctrl+Shift+P` on a PC or `Command+Shift+P` on a mac). Search for ArPiRobot. Choose "Create ArPiRobot Project". In the first dialog that shows up choose the folder to create the project in (it will have its own subfolder). Then you will be prompted to enter the name of the project. VSCode will then create the project and open it. By default there will be one python file called `robot.py`. Open it.

Once opened VSCode will (if the python extension is installed) have a few options along the bottom bar, one of which allows selecting a python installation. Click it and choose the python install that was setup for ArPiRobot. You may be prompted (by a message in the bottom right corner) to install pylint. Do so as it allows error detection in VSCode.

To deploy the code to the robot the Deploy Tool is used. First, connect your computer to the Robot's WiFi network. Open the Deploy Tool and connect to the robot. Under the Robot Program tab there are two file selectors. First you must select a project folder (this is the folder created by VSCode that has `robot.py` in it). Then, you need to select a main script. The main script is the python file that is run on the robot. In this case, there is only one python file: `robot.py` so choose it. Note that the main script must be located in the selected project folder. Once everything is selected click Deploy to Robot and wait for the deploy operation to finish (the Deploy Tool will show a dialog when complete).

To make sure the program is running select the Log tab. You should see something in the text box. If nothing is visible wait about 30 seconds. There should either be log info indicating that the robot code is running, or if it failed there will be a description of the error. If the default project does not run it is likely that you did not install any ArPiRobot update on the robot.

Once the program is running open the drive station and connect it to the robot. 

## Not Using Visual Studio Code
If you are using another IDE (or no IDE), there is nothing special about the project. You must have a root project folder containing one or more python files (optionally in subfolders).

## Default Project Structure and Conventions
See the [programming models](../reference/programming_models.md#project-structure-and-conventions) page.