# Starting With C++

## Creating a Project (Using VSCode)

If you have the VSCode extension installed, there will be an "ArPiRobot" button on the bottom toolbar when you open VSCode. If you click this button a set of ArPiRobot commands will show up (top middle of the screen).

![Drive Station Screenshot](../../img/vscode_arpirobot.png){: style="height:300px"}

After clicking this, choose "Create ArPiRobot Project". Then choose the "cpp" template.

Enter a project name, then you will be prompted to select a parent folder. In this parent folder, another folder for the project will be created (with the same name as you entered for the project).

Once selected, the project will be created and opened in VSCode. Once opened, if you have the proper extensions installed, you will be prompted to select a CMake kit. Choose "Raspberry Pi Toolchain" (should be on the bottom of the list).


## Project Structure

The C++ project uses a CMake build system. There are two folders for code files, `include` for headers and `src` for source files. 

By default, there are three pairs of files (header and source for each).

- `main.hpp` & `main.cpp`: Generally, you should not modify this file. This simply creates an instance of the `Robot` class (from `robot.hpp`) and starts it. The instance is stored and can be accessed using `Main::robot`. This can be useful when you need to access devices instantiated in the robot instance from other places in your code.

- `robot.hpp` & `robot.cpp`: The `Robot` class, inheriting from `BaseRobot` is defined here. In addition to having the periodic programming model functions, you should instantiate and initialize devices used on your robot here. You can implement custom functions to work with devices here as well.

- `actions.hpp` & `actions.cpp`: Define custom actions here (for action-based programming model).


## Building and Deploying

C++ code must be compiled (built) before it can run (unlike python and other interpreted languages). If you have the correct extensions installed, VSCode can build using the "Build" button in the bottom bar of VSCode. Any errors will be shown in the build output. Once the built is successful, you can use the deploy tool to deploy the program to the robot.

Open the deploy tool, connect it to the robot, and go to the robot program tab (make sure you have installed an ArPiRobot-CoreLib update package before deploying). In the robot program tab, select the project folder, and click deploy.

Once deployed, the program will be automatically started on the robot. Any errors, crashes, or other log info can be seen using the robot program log tab in the deploy tool.
