# Setup your Computer

There are several tools used to write ArPiRobot code on your computer, deploy code to and configure the robot, and control the robot.

## Installing Python

ArPiRobot code is written in python using the ArPiRobot python library, so we must first install python.

### Windows
The easiest way to install a specific python version on windows (along with pip and configuring your path) is by using a command line package manager called [Scoop](https://scoop.sh/). This will allow programs to be installed on windows as packages (much as is done on most Linux systems).

Follow the instructions on [Scoop's Website](https://scoop.sh/) to install scoop. Then run the commands below in  a powershell window.

```
scoop install git
scoop bucket add versions
```

To install python for only the current user
```
scoop install python35
```

To install python for all users
```
scoop install -g python35
```

** *Note: In the above examples python 3.5 is installed. Make sure to install the same version of python which is used on the robot (this is based on the Raspberry Pi Image used).* **

### macOS
The easiest way to install a specific python version on macos (and ensure that all required libraries are available and up to date) is by using a tool called pyenv, which can be installed with [Homebrew](https://brew.sh/) (a command line package manager for macOS). Follow the instructions on Homebrew's page to install homebrew. Then follow the process below to install pyenv and use it to build and install a specific python version.

First install required libraries
```
brew install openssl readline sqlite3 xz zlib
```

Then install pyenv
```
brew install pyenv
```

Then build and install a specific python version
```
pyenv install -v 3.5
```

** *Note: In the above example python 3.5 is installed. Make sure to install the same version of python which is used on the robot (this is based on the Raspberry Pi Image used).* **

### Linux / *BSD
The process of installing a specific python version will vary with Linux distributions (or BSD OSes). Pyenv should be able to be used. Just google instructions.

**Ubuntu**

Ubuntu users can use a ppa repo providing many python versions.

```
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.5
```

This will install python35 and pip35 executables.

** *Note: In the above example python 3.5 is installed. Make sure to install the same version of python which is used on the robot (this is based on the Raspberry Pi Image used).* **

## Installing Visual Studio Code
There are many development environments for writing python code, but for ArPiRobot we will focus on Visual Studio Code (VSCode). It provides intelligent code suggestions and error detection, and there is an ArPiRobot extension for VSCode that can help create projects.

Download and install VSCode from [https://code.visualstudio.com/](https://code.visualstudio.com/).

After installing it, open VSCode. A few extensions will be required/helpful. First, install Microsoft's Python extension. Then download the ArPiRobot VSCode Extension (see the [downloads page](../downloads.md)). Once you've downloaded the `vsix` file open VSCode's extensions pane (left side). Click the three dots menu button in the top right of the extensions pane. Choose install form VSIX and choose the downloaded VSIX file.

## Installing the Drive Station and Deploy Tool

### Windows
Download the exe installers for both (see [downloads page](../downloads.md)). Run the installer. A Java runtime will be installed with each program. The installer will create a start menu shortcut (and optionally a Desktop shortcut) as well.

### macOS
Download the `.zip` macOS packages (see [downloads page](../downloads.md)). Extract the zip files and move the resulting `.app` files to the `Applications` folder. Each app comes with its own Java runtime.

### Other
This method will work on any platform where Java is available (Linux, *BSD, Windows, macOS, etc). First install Java 11+. Then download the `.jar` files (see [downloads page](../downloads.md)). Most of the time, if Java is installed, double clicking the `.jar` file will run the program. If not, the command below can be used

```
java -jar /full/path/to/jar/file.jar
```
