
Before you can start programming or using ArPiRobots you will need to setup a few tools on the computer you plan to use to write code / work with the robot. This will be referred to as the "development computer". This is not the computer on the robot (Raspberry Pi, or other single board computers) which is referred to as the "main computer". 

Some of these tools are programming language specific, so it is good to choose which programming language(s) you plan to use first.


## Choosing a Programming Language

In general, it does not matter too much which language you use for robot code, as the same core library is used in all cases. Therefore, the biggest thing you should consider is familiarity with the language. If you're new to programming, it is recommended to use python as it is easy to learn and uses simpler syntax than many other languages.

In addition to familiarity with the language you should consider the complexity of setting up to use the language. Programming the robot using C++ requires a cross compiler toolchain setup as well as other tools installed on your computer. In contrast, python is easier to setup requiring only a python interpreter on your computer.

Finally, consider what your robot code will be doing. Most robot code is not performing computationally demanding tasks in user code. Generally, computationally demanding tasks are performed by the core library. In this case, there is minimal difference in performance between Python and C++. However, if computationally demanding code is needed in user code C++ will often perform better than Python due to Python's [GIL](https://wiki.python.org/moin/GlobalInterpreterLock).


## Editor / Development Environment

In order to write code you will need a text editor or code editor installed on your system. While you can use any text editor or code editor it is generally recommended to use Visual Studio Code (VSCode). VSCode can run on Windows, macOS, or Linux systems and has extensions to support both Python and C++ development. Additionally, an ArPiRobot extension for VSCode exists that enables creation of robot projects.

Visual Studio Code can be downloaded from [https://code.visualstudio.com/](https://code.visualstudio.com/). Once installed it is recommended to install the ArPiRobot extension. Currently, this extension is not on the VSCode marketplace so you will need to download it from the [downloads](../../downloads/latest.md) page. The downloaded file will be a `.vsix` file. It can be installed by opening VSCode, navigating to the extensions panel (fourth item down on the left menu bar) and choosing `Install from VSIX...` in the menu in the top right of the extensions panel.

Finally, it is recommended to install the following extensions (depending on which programming language you plan to use). These can be installed from the VSCode marketplace by searching in the extensions panel.

**C++ Programming:** C/C++ Extension Pack (By Microsoft)

**Python Programming:** Python Extension (By Microsoft)


## Language-Specific Tools

Depending on the programming language you plan to use you will need to install certain tools on your computer.

### C++

**Cross Compiler Toolchain:**

C++ development requires you have a compiler, linker, and necessary C libraries to build code for a different system (a Raspberry Pi). This collection of software is called a toolchain. Since software is being built for a different system than you are building it on, this is called cross compilation. 

Prebuilt cross compiler toolchains are available to be downloaded from the [downloads page](../../downloads/latest.md) page. Prebuilt toolchains are currently provided for Windows (x86_64), macOS (x86_64), and Linux (x86_64). *Use the revision of a toolchain linked on the downloads page.* New releases of the framework may use new revisions of a toolchain too.

Note that there toolchains provided for multiple target architectures (eg `armv6`, `aarch64`). Download the one matching the architecture of your main computer. Find this information where you download the OS image for your computer (again see downloads page for OS images).

The ArPiRobot toolchain packages can be installed using the Deploy Tool on the "This PC" tab.


**CMake**

You will also need CMake installed. As usual, if using Linux or BSD you should be able to install this from the system repos. If using Windows or macOS, you can download an installer from [cmake.org](https://cmake.org/). Alternatively, the [Homebrew](https://brew.sh/) package manager for macOS includes cmake and the [scoop](https://scoop.sh/) package manager for windows include cmake.

**GNU Make**

Make should be installed by default on macOS and Linux (if not use system packages). 

For windows, the easiest method is to use download from [here](https://gnuwin32.sourceforge.net/packages/make.htm) and run the installer. You must add `C:\Program Files (x86)\GnuWin32\bin` to the `PATH` environment variable. Alternatively, the [scoop](https://scoop.sh/) package manager for windows can install make and will automatically add it to your path when installed.

### Python

You will need to install Python on your PC. You can download python installers for windows and macOS from [python.org](https://www.python.org/downloads/). Generally, it is recommended to use a version of python that matches the first two numbers of the minimum version on the Pi (listed on the [downloads](../../downloads/latest.md) page under the OS image section). For example, if the "minimum python version" is listed as `3.7` it is recommended to install a python `3.7.x` version (where `x` can be any number). Note that some images will have newer python versions (the specific python version on the robot depends on which main computer your robot uses).

Alternatively, you can use the [Homebrew](https://brew.sh/) package manger on macOS or the [scoop](https://scoop.sh/) package manager on windows to install specific versions of python.

On Linux / BSD systems you should be able to install a recent version of python from the system repositories. You can use pyenv to build and install a specific version. On Ubuntu you can use the [deadsnakes PPA](https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa) to install specific versions of python.


## Drive Station and Deploy Tool

Finally, there are two ArPiRobot specific tools that need to be installed. The Drive Station is used to connect to a program running on the robot and control the robot using a game controller. If you do not have a game controller that can be connected to your PC there is also a Mobile Drive station app for Android phones and tablets with a virtual gamepad.

The Deploy Tool connects to the Raspberry Pi on the robot. The Deploy Tool is used to configure things such as WiFi networks and camera streaming, but most importantly it is used to deploy code from your computer to the robot with the click of a button.

**Windows**

Download the exe installers for both (see [downloads page](../../downloads/latest.md)). Run the installer. The installer will create a start menu shortcut (and optionally a Desktop shortcut) as well.

**macOS**

Download the `.zip` macOS packages (see [downloads page](../../downloads/latest.md)). Extract the zip files and move the resulting `.app` files to the `Applications` folder. Each app is *unsigned*, so the first time your run it open "Applications" in Finder. Then right click the app and choose open. This is only necessary the first time you run the app.

**Linux**

For Debian and derivatives (including Ubuntu and Linux Mint) there is a `.deb` package that can be installed. For other distributions, you will need to install python3 with pip and venv using your distribution's package manager. Then, download the `.run` installer. Run it using the command `sudo sh filename.run` (change filename to the name of the file you downloaded).


**Other**

The drive station and deploy tool are written in python and require python 3.6 or newer. In addition, several python packages are required (most notably `PySide6` and `PySDL2`). Download the zip of the repository (source code) and extract it on your system. Then install the required python packages (optionally in a virtual environment) by running `pip install -r requirements.txt` in the directory where you extracted the program. Then run the program with `python src/main.py`. Note that often one must replace `python` in the above commands with `python3`, but this is system dependent.


## ArPiRobot CoreLib Update Package

Finally, it is necessary to download a CoreLib update package (see [downloads page](../../downloads/latest.md)). The CoreLib update package contains a build of the ArPiRobot core library that can run on the robot along with other files needed when writing programs for the robot (in both C++ and Python). The same CoreLib update package is used for Python and C++. A CoreLib update package must be installed on your PC before deploying a program to the robot.

To install the downloaded update package open the Deploy Tool (installed perviously) and select the "This PC" tab. Click the "Install Update Package" button and select the CoreLib update package that was downloaded.
