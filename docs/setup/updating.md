# Installing Updates

## About Updates
It is *always* necessary to install an ArPiRobot Robot update after flashing the image to the SD Card to ensure that the latest version of the ArPiRobot Python Library is installed on the robot. When [downloading updates](../downloads.md) make sure to download an update that supports the version of the ArPiRobot image you are using (the latest update should always support the latest image).

In addition to Robot updates there are also Development Environment updates. Robot updates are packaged differently and contain multiple components and python dependencies that will work only on the robot. Development environment updates use the same python library version, but are intended to be installed on your development computer and require that the development computer be connected to the internet while installing.

*For each update you should make sure to install the robot update to the robot and the development environment update on your development computer(s). If the same update version is not used on both there may be false errors detected on your computer, or errors on the robot that your computer did not know about.*

## Robot Updates
After [downloading](../downloads.md) the Update zip file to your computer, open the Deploy Tool. Connect your computer to the Robot's WiFi then click connect in the Deploy Tool. Once connected, select the updates tab, choose the downloaded update zip file, and apply the update to the robot using the deploy tool.

## Development Environment Updates
After [downloading](../downloads.md) the Update zip to your computer, open VSCode and open a python file. Select the python version to update (bottom bar). Press `Ctrl+Shift+P` search ArPiRobot and select `Install ArPiRobot Update...`. Confirm the python version (prompt in lower right corner) then select the update in the open file dialog. Wait for the update to finish installing the reload the window when prompted.