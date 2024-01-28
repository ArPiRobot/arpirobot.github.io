
Before you can start programming or using ArPiRobots you will need to setup a few tools on the computer you plan to use to write code / work with the robot. This will be referred to as the "development computer". This is not the computer on the robot (Raspberry Pi, or other single board computers) which is referred to as the robot's "main computer". 

Some of the tools your development computer will need depend on the programming language you will use, so it is good to choose which programming language(s) you plan to use first.


## Choosing a Programming Language

In general, it does not matter too much which language you use for robot code, as the same core library is used in all cases. Therefore, the biggest thing you should consider is familiarity with the language. If you're new to programming, it is recommended to use python as it is easy to learn and uses simpler syntax than many other languages.

In addition to familiarity with the language you should consider the complexity of setting up to use the language. Programming the robot using C++ requires more tools setup to build the programs. In contrast, python is easier to setup requiring only a python interpreter on your computer.

Finally, consider what your robot code will be doing. If computationally demanding code is needed in user code, C++ will often perform better than Python due to Python's [GIL](https://wiki.python.org/moin/GlobalInterpreterLock).


## VSCode (Code Editor)

You can write robot code with any text editor / code editor. However, using Visual Studio Code (VSCode) is recommended as an extension is provided to generate ArPiRobot projects.

- [VSCode Download for Windows, macOS, Linux](https://code.visualstudio.com/)
- ArPiRobot extension: Currently, this extension is not on the VSCode marketplace so you will need to download it from the [downloads](../../downloads.md) page. The downloaded file will be a `.vsix` file. It can be installed by opening VSCode, navigating to the extensions panel (fourth item down on the left menu bar) and choosing `Install from VSIX...` in the menu in the top right of the extensions panel.

- Finally, it is recommended to install the following extensions (depending on which programming language you plan to use). These can be installed from the VSCode marketplace by searching in the extensions panel.
    - **C++**: C/C++ Extension Pack (By Microsoft)
    - **Python**: Python Extension (By Microsoft)


## Language-Specific Tools

Depending on the programming language you plan to use you will need to install certain tools on your computer.

### C++

To build robot programs written in C++ the following tools are needed

- [LLVM Clang & LLD Linker](https://llvm.org/)
- [Ninja Build](https://ninja-build.org/)
- [CMake](https://cmake.org/)

??? info "Windows Install Instructions"
    It is recommended to install these tools using the [scoop package manager](https://scoop.sh/) (follow the quickstart instructions on the linked page to install scoop). A package manager (such as scoop) allows installing programs using a single command. Scoop also installs the required tools such that the commands are in the user's PATH. This makes several of these tools easier to install.

    After installing scoop, run the following command (open powershell, copy paste and press enter)

    ```sh
    scoop install llvm ninja cmake
    ```

    If you don't want to use scoop, these tools can be installed manually too from the links above. However, you will need to make sure that `cmake.exe`, `ninja.exe`, `clang.exe`, `clang++.exe`, `lld.exe` are in the user's or system's PATH (environment variable).

??? info "macOS Install Instructions"
    **Note: macOS includes Apple clang, however LLVM clang is needed to be able to use the lld linker. This should be installed using brew!**

    It is recommended to install these tools using the [brew package manager](https://brew.sh/) (follow the installation instructions on the linked page to install brew). A package manager (such as brew) allows installing programs using a single command. Brew also installs the required tools such that the commands are in the system PATH. This makes several of these tools easier to install.

    After installing brew, run the following command (open terminal, copy paste and press enter)

    ```sh
    brew install llvm ninja cmake
    ```

    If you don't want to use brew, these tools can be installed manually too from the links above. However, you will need to make sure that `cmake`, `ninja` are in the user's or system's PATH (environment variable). When building ArPiRobot programs, you will also need to make sure LLVM `clang`, `clang++`, and `lld` are in the PATH environment variable before Apple Clang's executables (this is not needed if using brew to install because the toolchain files check where brew installs these by default).

??? info "Linux Install Instructions"
    Most Linux distributions should include these tools via the system package manager.

    **Debian / Ubuntu:** `sudo apt install clang lld ninja-build cmake`

    **Fedora / RHEL:** `sudo dnf install clang lld ninja-build cmake`

    **Arch:** `sudo pacman -S clang lld ninja cmake`

### Python

You will need to install python on your development computer. ArPiRobot robots currently use Python `3.11`, thus it is recommended to install this version of python on your development computer (note: you can have multiple versions of python installed).


??? info "Windows Install Instructions"
    It is recommended to install this using the [scoop package manager](https://scoop.sh/) (follow the quickstart instructions on the linked page to install scoop). A package manager (such as scoop) allows installing programs using a single command. Scoop also installs the required tools such that the commands are in the user's PATH. This makes several of these tools easier to install.

    After installing scoop, run the following commands (open powershell, copy paste and press enter). 

    ```sh
    scoop bucket add versions
    scoop install python311
    ```

    If you don't want to use scoop, you can download python from [python](https://www.python.org/).

??? info "macOS Install Instructions"
    It is recommended to install this using the [brew package manager](https://brew.sh/) (follow the installation instructions on the linked page to install brew). A package manager (such as brew) allows installing programs using a single command. Brew also installs the required tools such that the commands are in the system PATH. This makes several of these tools easier to install.

    After installing brew, run the following command (open terminal, copy paste and press enter)

    ```sh
    brew install python@3.11
    ```

    If you don't want to use brew, you can download python from [python](https://www.python.org/).

??? info "Linux Install Instructions"
    Your Linux distribution may provide packages for the required version of python. If so, they are likely named `python3.11` or `python311`. You can check the version of python3 included with your distribution by running `python3 --version` (if it is `3.11.x`, where `x` is any number, you already have the required python version).

    If your distribution doesn't provide the required version, you may be able to find a third party build (eg [deadsnakes ppa for Ubuntu](https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa)).

    If you can't find the required version any other way, you may have to build from source (extract the python source package from python.org the run `./configure; make; sudo make install`)


## Drive Station and Deploy Tool

Finally, there are two ArPiRobot specific tools that need to be installed (regardless of which programming language you will use).

The Drive Station is used to connect to a program running on the robot and control the robot using a game controller. If you do not have a game controller that can be connected to your PC there is also a Mobile Drive station app for Android phones and tablets with a virtual gamepad.

The Deploy Tool connects to the robot's main computer. It is used to configure things such as WiFi networks and camera streaming, but most importantly it is used to deploy code from your development computer to the robot with the click of a button.

??? info "Windows Install Instructions"
    Download the exe installers for both the Drive Station and Deploy Tool (see [downloads page](../../downloads.md)). Run the installer. The installer will create a start menu shortcut (and optionally a Desktop shortcut) as well.

??? info "macOS Install Instructions"
    Download the `.zip` macOS packages for both the Drive Station and Deploy Tool (see [downloads page](../../downloads.md)). Extract the zip files and move the resulting `.app` files to the `Applications` folder. Each app is *unsigned*, so the first time your run it open "Applications" in Finder. Then right click the app and choose open. This is only necessary the first time you run the app.

??? info "Linux Install Instructions"
    For Debian and derivatives (including Ubuntu and Linux Mint) there is a `.deb` package that can be installed. For other distributions, you will need to install python3 with pip and venv using your distribution's package manager. Then, download the `.run` installer. Run it using the command `sudo sh filename.run` (change filename to the name of the file you downloaded). If using the `.run` installer, it can be uninstalled by using `uninstall.sh` in `/opt/ArPiRobot-DriveStation` or `/opt/ArPiRobot-DeployTool`.


## ArPiRobot CoreLib Update Package

Finally, it is necessary to download a CoreLib update package (see [downloads page](../../downloads.md)). The CoreLib update package contains a build of the ArPiRobot core library that can run on the robot along with other files needed when writing programs for the robot (regardless of programming language). The same CoreLib update package is used for all programming languages. A CoreLib update package must be installed on your PC before deploying a program to the robot.

To install the downloaded update package open the Deploy Tool (installed perviously) and select the "This PC" tab. Click the "Install Update Package" button and select the CoreLib update package that was downloaded.
