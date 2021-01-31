# Starting With Python

## Creating a Project (Using VSCode)

If you have the VSCode extension installed, there will be an "ArPiRobot" button on the bottom toolbar when you open VSCode. If you click this button a set of ArPiRobot commands will show up (top middle of the screen).

![Drive Station Screenshot](../../img/vscode_arpirobot.png){: style="height:300px"}

After clicking this, choose "Create ArPiRobot Project". Then choose the "python" template.

Enter a project name, then you will be prompted to select a parent folder. In this parent folder, another folder for the project will be created (with the same name as you entered for the project).

Once selected, the project will be created and opened in VSCode. Make sure the correct python interpreter is selected (see the python version in use by the Python extension in the bottom bar).


## Project Structure

All python source files will be in the `src` folder.

By default, there are three files:

- `main.py`: Generally, you should not modify this file. This simply creates an instance of the `Robot` class (from `robot.py`) and starts it. The instance is stored and can be accessed using `main.robot`, after importing the `main` module. This can be useful when you need to access devices instantiated in the robot instance from other places in your code.

- `robot.py`: The `Robot` class, inheriting from `BaseRobot` is defined here. In addition to having the periodic programming model functions, you should instantiate and initialize devices used on your robot here. You can implement custom functions to work with devices here as well.

- `actions.py`: Define custom actions here (for action-based programming model).


## Building and Deploying

Python is an interpreted language, therefore there is nothing needed to "build" the robot program.

Open the deploy tool, connect it to the robot, and go to the robot program tab (make sure you have installed an ArPiRobot-CoreLib update package before deploying). In the robot program tab, select the project folder, and click deploy.

Once deployed, the program will be automatically started on the robot. Any errors, crashes, or other log info can be seen using the robot program log tab in the deploy tool.
