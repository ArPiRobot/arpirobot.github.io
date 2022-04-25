# Setup your Computer

There are two ArPiRobot tools that run on your PC that allow you to control/drive the robot (Drive Station) and configure the robot (Deploy Tool).

In addition this guide will cover setting up Visual Studio Code to write ArPiRobot code. There is also an ArPiRobot extension available for VSCode that allows creating ArPiRobot projects.


## Installing the Drive Station and Deploy Tool

**Windows**

Download the exe installers for both (see [downloads page](../../downloads/latest.md)). Run the installer. A Java runtime will be installed with each program. The installer will create a start menu shortcut (and optionally a Desktop shortcut) as well.

**macOS**

Download the `.zip` macOS packages (see [downloads page](../../downloads/latest.md)). Extract the zip files and move the resulting `.app` files to the `Applications` folder. Each app comes with its own Java runtime. Each app is also *unsigned*, so when you first run it you will have to approve the app in System Preferences > Security.

**Ubuntu Linux**

A `.deb` package has been provided for Ubuntu. It has generally been tested on the latest LTS release. While not guaranteed it should still work on other ubuntu releases or other distributions that use deb packages. If you have trouble with the `.deb` package or use a different distribtion, a `.tar.gz` package is provided that should work on any linux distro (however, you must install python3 before using). If using the `.tar.gz` package run the `install.sh` script after extracting.


**Other**

The drive station and deploy tool are written in python and require python 3.6 or newer. In addition, several python packages are required (most notably `PySide6` and `PySDL2`). Download the zip of the repository (source code) and extract it on your system. Then install the required python packages (optionally in a virtual environment) by running `pip install -r requirements.txt` in the directory where you extracted the program. Then run the program with `python src/main.py`. Note that often one must replace `python` in the above commands with `python3`, but this is system dependent.


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

C++ development requires you have a compiler, linker, and necessary C libraries to build code for a different system (a Raspberry Pi). This collection of software is called a toolchain. Since software is being built for a different system than you are building it on, this is called cross compilation. 

Prebuilt cross compiler toolchains for the Raspberry Pi are available to be downloaded from the [ArPiRobot-Toolchain Releases](https://github.com/ArPiRobot/ArPiRobot-Toolchain/releases) page. Prebuilt toolchains are currently provided for Windows (64-bit), macOS (x86_64 = 64-bit Intel CPUs), and Linux (x86_64). For other systems, you can build a cross compiler toolchain yourself, but you might prefer to use Python to program robots instead. Building a cross compiler can be a complex task.

A toolchain is intended to be used with a specific version of the Raspberry Pi OS (ArPiRobot images are modified versions of the Raspberry Pi OS). Each release of the Raspberry Pi OS is based on a version of debian, identified by the version's codename. For example, on the downloads page, if an image's RasPiOS version is listed as `2020-05-27-raspios-buster-lite`, the image is based on Raspberry Pi OS Buster. "Buster" is the codename. As such, you need a toolchain designed to target a Raspberry Pi OS Buster system. All prebuilt ArPiRobot toolchains are listed under a specific codename. While it may be possible to use an older toolchain (for example, using a RasPiOS 10 "buster" toolchain and running the program on RasPiOS 11 "bullseye"), it is generally not recommended.

Alternatively, prebuilt toolchains can be downloaded from other sources. These toolchains have not been tested with the ArPiRobot project, but there is no reason they should not work. However, be aware that not all toolchains support the Pi Zero (`armv6` architecture). The ArPiRobot images are also 32-bit images, so a compiler targeting a 64-bit system will not work.

- SysProgs Toolchains (Windows Machine):  [link](https://gnutoolchains.com/raspberry/)
    - These are compatible with the Pi Zero
- raspberry-pi-cross-compilers project (Linux Machine): [link](https://github.com/abhiTronix/raspberry-pi-cross-compilers)
    - There are multiple builds of each toolchain. Generally, use the same version of gcc as is installed on the Pi.
    - When you download the toolchain, you will be taken to a stackoverflow page with multipe folders. If you want the version with Pi zero support, choose that folder.

The ArPiRobot toolchain packages can be installed using the Deploy Tool. Other toolchains must be installed manually. The toolchain should be placed in `$HOME/.arpirobot/toolchain`. The `toolchain` directory should contain `bin`, `include`, `lib`, etc (they should not be in another subdirectory). With third party toolchains it may be necessary to run an installer or extract it elsewhere and copy the correct directories.


**CMake**

You will also need CMake installed. As usual, if using Linux or BSD you should be able to install this from the system repos. If using Windows or macOS, you can download an installer from [cmake.org](https://cmake.org/). Alternatively, the [Homebrew](https://brew.sh/) package manager for macOS includes cmake and the [scoop](https://scoop.sh/) package manager for windows include cmake.

**GNU Make**

Make should be installed by default on macOS and Linux (if not use system packages). 

For windows, the easiest method is to use download from the [ezwin32 project](https://downloads.sourceforge.net/project/ezwinports/make-4.3-without-guile-w32-bin.zip). This will download a zip file, not an installer. You need to extract the zip somewhere on your system and add the "bin" folder to your `PATH` environment variable. Alternatively, the [scoop](https://scoop.sh/) package manager for windows includes make and will automatically add it to your path.


### Python

You will need to install Python on your PC. You can download python installers for windows and macOS from [python.org](https://www.python.org/downloads/). Generally, it is recommended to use a version of python that matches the first two numbers of the version on the Pi (see the images download table for which version is in use in the newest Pi image). Alternatively, you can use [Homebrew](https://brew.sh/) on macOS or [scoop](https://scoop.sh/) on windows.

On Linux / BSD systems you should be able to install a recent version of python from the system repositories. You can use pyenv to build and install a specific version. On Ubuntu you can use the [deadsnakes PPA](https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa) to install specific versions of python.
