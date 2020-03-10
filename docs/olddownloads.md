# Downloads


#### **About**
There are a lot of things listed below, and there are even more components that go into making the robots work, so the following is just a basic description. More in-depth descriptions can be found on the [Components Page](reference/components.md).

Modified versions of [Raspbian](https://www.raspberrypi.org/documentation/raspbian/) are released that are pre-configured to work with ArPiRobot robots and have some components pre-installed. Each version of the images listed below will have several updates created for it. The update packages will update or install the components (Python Library, Raspbian tools, etc) that are used to make robot programs work.

Updates generally contain the ArPiRobot PythonLibrary and the ArPiRobot Raspbian Tools. Each of these have independant versions, but will be desiged to work with one specific image (and one specific python version). They will be bundled in an update package for that image.

To elimimate the need for internet access on the robot the update packages also contain python packages (dependencies of the ArPiRobot Python Library) that will be installed using pip on the robot.

The ArPiRobot Drive Station versions are often compatible with multiple python library versions (and thus often work across image versions). However, at times the communciation between the python library and the Drive Station will change. Make sure the Drive Station version you are using is compatible with the python library version you are using (using the latest of each should always work).

Similarly, the Deploy Tool uses some of the Raspbian Tools to perform tasks. Often these do not change between image versions, but if they do it is important to make sure that the deploy tool works with the same version of raspbian tools (using the latest of each should always work).


*Note: Some of the early betas did not follow the versioning scheme described below*

**Images are versioned as follows:**

- Beta1, Beta2, etc

**Updates are Versioned as follows:**

*IMAGE_VERSION*-UpdateNumber

For example the fourth update to the Beta2 image would be called

Beta2-Update4

The second update to v1.0.0 would be called

v1.0.0-Update2


#### **ArPiRobot Images**

| Release Date | Image Version | Python Version | Download |
| ------------ | ------------- | -------------- | -------- |
| 8-10-19 | Beta1 | 3.5 | [Download](https://1drv.ms/u/s!AhjgTI1qxX9xg7RJJkTmLCj5crQlBg?e=r78qN6) |
| 12-06-19 | Beta2 | 3.7 | [Download](https://1drv.ms/u/s!AhjgTI1qxX9xg9QQMEcYP5wnOJGr2g?e=c5RYlK)

When downloading updates, the Drive Station, the Deploy Tool, and the Arduino Firmware, make sure the versions you download are compatible with the image you are using. The release notes for each component (visible with the download) will list compatible image versions.


#### **Compatibility Tables**:

**Drive Station and Python Library Compatibiltiy**

| Drive Station Versions | Supported Python Libray Versions |
| ---------------------- | -------------------------------- |
| v0.3.0 to current      | v0.0.7 to current                |
| v0.2.0 to v0.2.2       | v0.0.4 to v0.0.6                 |
| Before v0.2.0          | Before v0.0.4                    |

**Deploy Tool and Raspbian Tool Compatilility**

| Deploy Tool Versions | Raspbian Tool Versions |
| -------------------- | ---------------------- |
| v0.1.0 to current    | v0.1.0 to current      |
| Before v0.1.0        | Before v0.1.0          |


#### **ArPiRobot Updates**
Updates can be downloaded from [GitHub](https://github.com/MB3hel/ArPiRobot-UpdatePackager/releases). 

There are two update zips for each update: A robot update zip (to be installed on the robot with the Deploy Tool) and a development update (to be installed on the development computer using the containd installer scripts).

#### **Drive Station**
The Drive Station can be downloaded form [GitHub](https://github.com/MB3hel/ArPiRobot-DriveStation/releases).

Each release has three files: A windows installer (.exe), a macOS package (.zip), and a universal Java archive (.jar). The Windows installers and macOS packages install Java with the Drive Station, however if you are using the JAR (usually on Linux or *BSD), it is necessary to also install Java (11 or newer). It is recommended to use distribution packages for Linux and BSDs. If using the JAR on macOS or Windows, it is recommended to use [AdoptOpenJDK](https://adoptopenjdk.net/) packages.

#### **Deploy Tool**
The Deploy Tool can be downloaded form [GitHub](https://github.com/MB3hel/ArPiRobot-DeployTool/releases).

Each release has three files: A windows installer (.exe), a macOS package (.zip), and a universal Java archive (.jar). The Windows installers and macOS packages install Java with the Deploy Tool, however if you are using the JAR (usually on Linux or *BSD), it is necessary to also install Java (11 or newer). It is recommended to use distribution packages for Linux and BSDs. If using the JAR on macOS or Windows, it is recommended to use [AdoptOpenJDK](https://adoptopenjdk.net/) packages.

#### **Arduino Firmware**
The ArPiRobot Arduino firmware can be downloaded from [GitHub](https://github.com/MB3hel/ArPiRobot-ArduinoFirmware/releases).

It is necessary to install the [Arduino IDE](https://www.arduino.cc/en/Main/Software) (Not the online version) to build and upload the project to an Arduino compatible board. If using a Teensy board it is also necessary to install [Teensyduino](https://www.pjrc.com/teensy/teensyduino.html).

#### **ArPiRobot VSCode Extension**
The ArPiRobot Arduino firmware can be downloaded from [GitHub](https://github.com/MB3hel/ArPiRobot-VSCodeExtension/releases).

