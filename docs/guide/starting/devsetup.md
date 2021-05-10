# Setup your Computer

There are two ArPiRobot tools that run on your PC that allow you to control/drive the robot (Drive Station) and configure the robot (Deploy Tool).

In addition this guide will cover setting up Visual Studio Code to write ArPiRobot code. There is also an ArPiRobot extension available for VSCode that allows creating ArPiRobot projects.


## Installing the Drive Station and Deploy Tool

**Windows**

Download the exe installers for both (see [downloads page](../../downloads/latest.md)). Run the installer. A Java runtime will be installed with each program. The installer will create a start menu shortcut (and optionally a Desktop shortcut) as well.

**macOS**

Download the `.zip` macOS packages (see [downloads page](../../downloads/latest.md)). Extract the zip files and move the resulting `.app` files to the `Applications` folder. Each app comes with its own Java runtime. Each app is also *unsigned*, so when you first run it you will have to approve the app in System Preferences > Security.

**Ubuntu Linux**

A `.deb` package has been provided for Ubuntu. It has generally been tested on the latest LTS release. While not guaranteed it should still work on other ubuntu releases or other distributions that use deb packages. If you have trouble with the deb package just install java and use the `.jar` file.

**Arch Linux**

A `.pkg.tar.zst` package for Arch Linux is provided and can be installed using pacman.

**Other**

This method will work on any platform where Java is available (Linux, *BSD, Windows, macOS, etc). First install Java 11 (or newer). Then download the `.jar` files (see [downloads page](../../downloads/latest.md)). Most of the time, if Java is installed, double clicking the `.jar` file will run the program. If not, the command below can be used

```
java -jar /full/path/to/jar/file.jar
```

Note: For &ast;BSD users the drive station does not include a build of the required native library. You will have to build it yourself. You can find instructions on doing so in the Drive Station's GitHub repository. The only platforms that have builtin support are: Windows (32-bit), Windows (64-bit), Linux (64-bit), macOS (64-bit).


## Installing Visual Studio Code

While there are many different development environments, using VSCode is recommended for ArPiRobot code as there is easy support for many different programming languages. In addition, there is an ArPiRobot extension for VSCode that can be used to create ArPiRobot projects from a template.

Download and install VSCode from [https://code.visualstudio.com/](https://code.visualstudio.com/).

Then download the ArPiRobot VSCode Extension (see the [downloads page](../../downloads/latest.md)). Once you've downloaded the `vsix` file open VSCode's extensions pane (left side). Click the three dots menu button in the top right of the extensions pane. Choose install form VSIX and choose the downloaded VSIX file.

Finally, there are a few extensions you may want to install for different programming languages. These extensions can be installed form VSCode's marketplace (by searching in the extensions panel in VSCode).

**C++ Programming:**

- C/C++ Extension (By Microsoft)
- CMake Tools Extension (By Microsoft)


**Python Programming:**

- Python Extension (By Microsoft)
- Pylance Extension (By Microsoft)


## CoreLib Updates

Before you can write programs for ArPiRobot robots, you will need to install an ArPiRobot-CoreLib update package on your computer. The ArPiRobot-CoreLib is a library that is used when writing robot programs to provide networking, support for different devices, sensors, etc. When you deploy your code using the deploy tool, whatever version of the CoreLib is on your computer will be deployed to the robot along with your code.

For this reason, it is important to regularly install the corelib update packages using the deploy tool. To install a corelib update package (see [downloads page](../../downloads/latest.md)) open the deploy tool, click the "This PC" tab, then click "Install update package" then choose the downloaded update package.


## Other Tools

The rest of the tools are language-specific. You will only need the tools for the programming language you plan to use.

### C++

**Cross Compiler Toolchain:**

The setup for programming with C++ can be relatively complex, but is much easier if you can find a pre-built cross compiler toolchain for compiling programs for the Raspberry Pi. The only time you will not need a cross compiler toolchain is if you are using an Arm (v6) linux system (such as another raspberry pi). In that case, you can use the native compiler for your system.

For Windows, you can download an installer for a prebuilt toolchain from [here](https://gnutoolchains.com/raspberry/)

For Linux, you can download a prebuilt toolchain from [here](https://github.com/abhiTronix/raspberry-pi-cross-compilers).

For macOS... I don't know of any pre-built toolchains. You can build one yourself, but you might prefer to look into using Python or Java.

When downloading the toolchain you need one for the same version of Raspberry Pi OS as is used on the robot (see Raspberry Pi OS version in image version table). Currently this is GCC 8.3.x for buster. When using the linux toolchains linked above, make sure to download one that supports a Pi zero target.

When installing the toolchain you need to install it to `.arpirobot/toolchain` in your home directory. The `.arpirobot` directory will be created the first time you run the deploy tool or drive station.

On windows, using the exe installer, set the install location to `C:\USERNAME\.arpirobot\toolchain`, where `USERNAME` is your windows username.

On Linux, extract the downloaded archive to `~/.arpirobot`. Then rename the `cross-pi-...` folder to `toolchain`.

If updating the toolchain, you will need to delete/uninstall the old one first.

**CMake**

You will also need CMake installed. As usual, if using Linux or BSD you should be able to install this from the system repos. If using Windows or macOS, you can download an installer from [cmake.org](https://cmake.org/).

**GNU Make**

On windows, you will also need a make program. One should be installed by default on macOS and Linux (if not use system packages). For windows, download from [gnuwin32 project](http://gnuwin32.sourceforge.net/packages/make.htm).

### Python

You will need to install Python on your PC. You can download python installers for windows and macOS from [python.org](https://www.python.org/downloads/). Generally, it is recommended to use a version of python that matches the first two numbers of the version on the Pi (see the images download table for which version is in use in the newest Pi image).

On Linux / BSD systems you should be able to install a recent version of python from the system repositories. You can use pyenv to build and install a specific version. On Ubuntu you can use the [deadsnakes PPA](https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa).

