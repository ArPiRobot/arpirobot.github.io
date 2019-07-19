# Using the Minimal Image

## Configure the Pi
To configure the Pi first connect to its WiFi network. It will be called `ArPiRobot-RobotAP` and the password will be `arpirobot123`. Once connected open the Deploy Tool and connect to the robot at the address `192.168.10.1` (default username and password `pi` and `arpirobot` should work).

Once connected there are a few things to configure. First, have it connect to a WiFi network with internet access. This is done *in addition to* generating its own network. It will even provide internet access over its own network once it connects to another one with internet access. To connect to a different network goto the WiFi tab and under the "Other Network" section enter the name (SSID) and password for a different network.

*Note: the deploy tool cannot currently connect the pi to a network with no password. Only WPA2 networks are supported*

You can also at this time change the default name and password for the robot's access point if you want to. When done click Apply. The deploy tool will now be disconnected and WiFi changes will be made. If the robot does not seem to connect to the other network that was specified, just reboot it.

You may also want to use the deploy tool at this time to change the robot's hostname (under the "Other" tab).

## Creating a Program
When using a robot with the minimal image code will (usually) be developed on another PC. It is therefore necessary to setup said other PC for python development. There are many IDEs that can be used for writing python code, but the simplest and most common is IDLE (installed with python). This guide will use IDEL, however other IDEs can be used too.

### Installing Python3 and IDLE
To install python 3 and IDLE on Windows or macOS use the installers provided here. On Linux or BSD packages are most likely provided. Google it.

It is often useful to install the same version of python as is on the Raspberry Pi (3.5.x at the time of writing).

When installing on windows make sure to select Customize install and ensure that Pip is installed. Also (on the second customization page) make sure to check "Add python to environment variables" and "Create shortcuts for installed applications". Once the install completes move on to the next step.

## Setting Up For ArPiRobot Development
A few python libraries are *required* for ArPiRobot and some others are *recommended*. It is a good idea to install all of these or IDLE may give you error messages that would not occurr on the robot.

The following command will install all required and recommended packages. If you are familiar with virtual environments and how to use them

Windows (run in command prompt)
```
python -m pip install apscheduler ansicolors pyserial adafruit-circuitpython-motorkit
```

macOS/Linux (run in terminal)
```
pip3 install apscheduler ansicolors pyserial adafruit-circuitpython-motorkit
```

Next install the ArPiRobot library itself. Goto the [github repository](https://github.com/MB3hel/ArPiRobot-PythonLib) and clone the repository or download and extract it as a zip file. Open a command prompt or terminal in that folder and run

Windows:
```
python setup.py install
```

macOS/Linux:
```
sudo python3 setup.py install
```

### Creating a Program

- First, create a folder to keep ArPiRobot programs in. For example you could create a folder called `ArPiRobotProjects` in your documents folder.
- Next, open IDLE (there should be a shortcut in the start menu on windows and on other OSes running the command `idle3` should work).
- Select File > New File and paste the following contents.

```
from arpirobot.core.logger import *
from arpirobot.core.robot import BaseRobot, start_robot

class MyRobot(BaseRobot):
    def robot_started(self):
        pass

    def process(self):
        pass

    def enabled(self):
        pass

    def process_enabled(self):
        pass

    def disabled(self):
        pass

    def process_disabled(self):
        pass

my_robot = MyRobot()
start_robot(my_robot)
```

- Since this program does not do anything with any robot devices (motors, sensors, etc) it can actually be run on the computer by clicking Run > Run Module. If there are any errors when doing so make sure all the packages listed above were installed correctly.
- The robot program can run on any PC (not just a Raspberry Pi) as long as it is not using Raspberry Pi specific hardware (such as motor hats, I2C, SPI, GPIO, etc). You could even connect the drive station to the robot program running on the local PC by using "localhost" as the robot address.
- For more infomation on the structre of this program see the [Programming Guide](guide/intro.md)

## Deploying The Program To The Robot

Now that we have a working program it is time to deploy it to the robot. To do so make sure the robot is powered on. Make sure you are connected to the same network as the robot (either the "Other network" it is setup to connect to or the robot's own network). Open the deploy tool and connect to the robot. Under the "Robot Code" tab select the project folder (this is the folder that has the `example.py` program in it). Then select a main script. Choose `example.py`. Finally, click "Deploy to Robot". This will copy the entire contents of the project folder to the robot and configure it to run the main script on boot. It will then start the main script.

The code should now be running on the robot. Launch the drive station and try to connect. Make sure the robot address is either set to the hostname or IP address of the robot on your network. Try the enable and disable buttons and look at the robot log panel. You will see the messages from `log_info`.

## Other Tasks

### Changing The Password

To change the password for the pi user (this is the same password used to login with the deploy tool) login over SSH. Change the hostname in the following command if necessary.

```
ssh pi@arpirobot-robot
```

Enter the current password then run

```
passwd
```

And enter the required information.

### Updating Software on the Raspberry Pi
Login via SSH (see previous section) and use `apt` to install or upgrade linux software.

### Updating ArPiRobot Components
Git repos for the python library and Raspbian tools are located in the pi user's home folder.

When connected to the internet the git repos for different components can be updated by running the following command in one of the `arpirobo-[component]` directories. 

```
git pull origin master
```

For the python library re-run `sudo python setup.py install`

For the raspbian tools re-run `sudo ./install.sh`

### Powering Off or Rebooting
As always, the deploy tool can be used (other tab), but as long as the Pi is in Readonly mode (which it will be unless the deploy tool was used to change it since rebooting last) it is safe to unplug at any time. *This is true only for the minimal image. The complete image is never readonly!*