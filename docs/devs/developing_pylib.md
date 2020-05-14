# PythonLib Development

Development of the ArPiRobot-PythonLib can be done using VSCode with the same basic setup as is used for writing robot code.

Type hints should be used whenever possible.

## Testing Setup
A directory called `testrobot` in the ArPiRobot-PythonLib directory is excluded form the git repo. This is so that a test robot program can be created in this folder while making changes to the pythlonlib. It is often useful to open this folder in its own VSCode window or error detection may not work correctly.

When ready to test you will need to be connected to a robot via its WiFi network (or have some network connection to the Robot's Pi).

On the development PC run the `dev-deploy.sh` script (you can use WSL or Git Bash on windows) in the root of the ArPiRobot-PythonLib folder. You will be prompted twice to enter the password for the pi user on the robot. This will copy the important files and folders from the ArPiRobot-PythonLib repo folder on your PC to `/home/pi/ArPiRobot-PythonLib` on the robot. The robot will remain read/write after running this script.

To run the `testrobot` program (which is copied to the Pi by the `dev-deploy.sh` script) login to the Pi via ssh Can use OpenSSH on Windows 10 or PuTTY on Windows 10 and older versions of windows. The commands below are OpenSSH commands which will work with OpenSSH on Windows 10, macOS, Linux, etc.

```
ssh -l pi 192.168.10.1
```

Then (in the SSH session after logging in as `pi`) run. Change `robot.py` as needed.

```
# Only have to run this once to stop the program started on boot
sudo systemctl stop arpirobot-program

cd ~/ArPiRobot-PythonLib
PYTHONPATH=. python3 testrobot/robot.py
```

This will start the testrobot program using the same PythonLib code as is on your development PC.