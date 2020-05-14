# Installing Updates

## About Updates
It is *always* necessary to install an ArPiRobot Robot update after flashing the image to the SD Card to ensure that the latest version of the ArPiRobot Python Library is installed on the robot. When [downloading updates](../downloads.md) make sure to download an update for the image you are using. Each update only supports one image. For example if you are using the "Beta6" image download the latest Beta6 update.

The python installation on the PC you are using to write the robot code also needs to be updated. This can be done through the VSCode extension.

*For each update you should make sure to install the robot update to the robot and the development environment update on your development computer(s). If the same update version is not used on both there may be false errors detected on your computer, or errors on the robot that your computer did not know about.*

*It is **not** necessary to go through installing each update for an image in order. It is fine to jump from update 1 to 4 for example. The updates are all standalone.*

## Robot Updates
Note: You do not need internet access on the robot to update the robot.

After [downloading](../downloads.md) the Update zip file to your computer, open the Deploy Tool. Connect your computer to the Robot's WiFi then click connect in the Deploy Tool. Once connected, select the updates tab, choose the downloaded update zip file, and apply the update to the robot using the deploy tool.

## Development Environment Updates
Note: You will need internet access on your computer to update the development environment.

After [downloading](../downloads.md) the Update zip to your computer, open VSCode and open a python file. Select the python version to update (bottom bar). Press `Ctrl+Shift+P` search ArPiRobot and select `Install ArPiRobot Update...`. Confirm the python version (prompt in lower right corner) then select the update in the open file dialog. Wait for the update to finish installing the reload the window when prompted.

**Manual Updates (Only if not using VSCode extension)**

- Extract the update zip
- Extract the pythonlib zip
- In the extracted pythonlib directory run the following where `python` is the correct python interpreter (version) you are using for robot program development (you will need internet access)

```
python -m pip install -r requirements.txt
python -m pip install .
```