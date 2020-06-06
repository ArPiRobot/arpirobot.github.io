# Installing Updates

## About Updates

Robot updates generally contain two things:

- A version of the ArPiRobot-PythonLib
- A version of the ArPiRobot-RaspbianTools

The PythonLib is necessary to run python programs. The RaspbianTools are necessary to configure the robot and make sure everything works as intended.

It is *always* necessary to install an ArPiRobot Robot update after flashing the image to the SD Card to ensure that the latest version of the ArPiRobot-RaspbianTools are installed on the robot. This is necessary even if you are not using Python.

When [downloading updates](../downloads.md) make sure to download an update for the image you are using. Each update only supports one image. For example if you are using the "Beta6" image download the latest Beta6 update.

If you are writing your robot code in Python you will also need to install the *same* update on your PC to make sure the same version of the PythonLib is available on the robot and your PC. This can be done through the VSCode extension.

*For each update you should make sure to install the robot update to the robot and the development environment update on your development computer(s). If the same update version is not used on both there may be false errors detected on your computer, or errors on the robot that your computer did not know about.*

If you are using Java you do **not** need to install the updates on the development PC, as the JavaLib is handled differently. The JavaLib is not installed on the computer nor on the robot. Instead, it is included with the Java program when it is deployed to the robot.

*It is **not** necessary to go through installing each update for an image in order. It is fine to jump from update 1 to 4 for example. The updates are all standalone.*


You should always make sure you are using the latest available update as updates include bug fixes as well as new features. Also make sure the tools on your PC (Drive Station and Deploy Tool) are compatible with the update you are using (this information is on the [Downloads](../downloads.md) page).

## Robot Updates
Note: You do not need internet access on the robot to update the robot.

After [downloading](../downloads.md) the Update zip file to your computer, open the Deploy Tool. Connect your computer to the Robot's WiFi<sup>&ast;</sup> then click connect in the Deploy Tool. Once connected, select the updates tab, choose the downloaded update zip file, and apply the update to the robot using the deploy tool.

<sup>&ast;</sup>Make sure to disconnect from other types of networks when you do this (including ethernet connections).

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
