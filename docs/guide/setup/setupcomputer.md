
Before you can start programming or using ArPiRobots you will need to setup a few tools on the computer you plan to use to write code / work with the robot. This will be referred to as the "development computer". This is not the computer on the robot (Raspberry Pi, or other single board computers) which is referred to as the robot's "main computer". 

Some of the tools your development computer will need depend on the programming language you will use, so it is good to choose which programming language(s) you plan to use first.


## Choosing a Programming Language

In general, it does not matter too much which language you use for robot code, as the same core library is used in all cases. Therefore, the biggest thing you should consider is familiarity with the language. If you're new to programming, it is recommended to use python as it is easy to learn and uses simpler syntax than many other languages.

In addition to familiarity with the language you should consider the complexity of setting up to use the language. Programming the robot using C++ requires more tools setup to build the programs. In contrast, python is easier to setup requiring only a python interpreter on your computer.

Finally, consider what your robot code will be doing. If computationally demanding code is needed in user code, C++ will often perform better than Python due to Python's [GIL](https://wiki.python.org/moin/GlobalInterpreterLock).


## VSCode (Code Editor)

You can write robot code with any text editor / code editor. However, using Visual Studio Code (VSCode) is recommended as an extension is provided to generate ArPiRobot projects.

- [VSCode Download for Windows, macOS, Linux](https://code.visualstudio.com/)
- ArPiRobot extension: Download from the [downloads](../../downloads.md) page. The downloaded file will be a `.vsix` file. It can be installed by opening VSCode, navigating to the extensions panel (fourth item down on the left menu bar) and choosing `Install from VSIX...` in the menu in the top right of the extensions panel.

- Finally, it is recommended to install the following extensions (depending on which programming language you plan to use). These can be installed from the VSCode marketplace by searching in the extensions panel.
    - **C++**: C/C++ Extension Pack (By Microsoft)
    - **Python**: Python Extension (By Microsoft)


## Language-Specific Tools

Depending on the programming language you plan to use you will need to install certain tools on your computer.

### Setting up a Package Manger

This section requires installing development tools on your computer. While this can be done manually, it is often a complex process to locate, install everything, and ensure it is in the system `PATH` so it can be located properly. As such, package managers will be used to simplify the installation process.

A package manager lets you install various programs by running a single command and it will take care of setting everything up so that it can be found easily. Linux distributions will include a system package manager (eg `apt` on Debian/Ubuntu and `dnf` on Fedora). However, we will need to install one on Windows or macOS.

??? info "Install scoop package manager on Windows"
    1. Open the start menu and search for "Powershell". Open "Windows Powershell"

    2. Copy the following command (comes from [scoop.sh](https://scoop.sh/))

    ```sh
    Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
    ```

    3. Right click in the powershell window to paste the copied text

    4. Press enter to run command

    5. Copy the following command (comes from [scoop.sh](https://scoop.sh/))

    ```sh
    Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
    ```

    6. Right click in the powershell window again to paste it

    7. Press enter to run it

    8. Wait for scoop to finish installing

??? info "Install brew package manager on macOS"
    1. Open Terminal (search for it in launchpad or open finder and go to `Applications > Utilities > Terminal`)

    2. Copy the following command (comes from [brew.sh](https://brew.sh/))

    ```sh
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```
    
    3. Paste it in the terminal window (CMD+V or right click > paste)

    4. Press enter to run the command

    5. Wait for brew to finish installing

Installing and using the above package managers is highly recommended. You can install the required tools in a different way, but this guide assumes you are using the package managers listed above.

### C++

To build robot programs written in C++ you need to install LLVM, CMake, Ninja, and pkg-config.

??? info "Windows Install Instructions"
    1. Search for "Powershell" in the start menu and open "Windows Powershell"

    2. Copy the following command

    ```sh
    scoop install llvm ninja cmake pkg-config
    ```

    3. Right click in the powershell windows to paste it

    4. Press enter to run the command

    5. Wait for scoop to finish installing the requested packages

??? info "macOS Install Instructions"
    1. Open Terminal (search for it in launchpad or open finder and go to `Applications > Utilities > Terminal`)

    2. Copy the following command

    ```sh
    brew install llvm ninja cmake
    ```
    
    3. Paste it in the terminal window (CMD+V or right click > paste)

    4. Press enter to run the command

    5. Wait for brew to finish installing the requested packages

??? info "Linux Install Instructions"
    Most Linux distributions should include these tools via the system package manager.

    Open a terminal and paste the following command. Then press enter.

    **Debian / Ubuntu:** `sudo apt install clang lld ninja-build cmake`

    **Fedora / RHEL:** `sudo dnf install clang lld ninja-build cmake`

    **Arch:** `sudo pacman -S clang lld ninja cmake`

### Python

You will need to install python on your development computer. ArPiRobot robots currently use Python `3.11`, thus it is recommended to install this version of python on your development computer (note: you can have multiple versions of python installed).


??? info "Windows Install Instructions"
    1. Search for "Powershell" in the start menu and open "Windows Powershell"

    2. Copy the following command

    ```sh
    scoop bucket add versions
    ```

    3. Right click in the powershell windows to paste it

    4. Press enter to run the command

    5. Wait for the command to finish

    6. Copy the following command

    ```sh
    scoop install python311
    ```

    7. Right click in the powershell windows to paste it

    8. Press enter to run the command

    5. Wait for scoop to finish installing the requested packages

??? info "macOS Install Instructions"
    1. Open Terminal (search for it in launchpad or open finder and go to `Applications > Utilities > Terminal`)

    2. Copy the following command

    ```sh
    brew install python@3.11
    ```
    
    3. Paste it in the terminal window (CMD+V or right click > paste)

    4. Press enter to run the command

    5. Wait for brew to finish installing the requested packages

??? info "Linux Install Instructions"
    Linux distributions usually include python, but it may not be version `3.11`. You can check the python version by running `python3 --version` in a terminal. If the version is `3.11.x` (x can be any number), you have python `3.11`.

    If you do not have python `3.11`, ideally you should install it (you can use a different version of python on your development computer, but error detection may not work properly if you do).

    How you install specific versions of python depends on your Linux distribution

    **Ubuntu:** The [deadsnakes ppa](https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa) may have the version of python you need. Python 3.11 packages are currently provided for Ubuntu 20.04 and 22.04. Add the ppa and run `apt install python3.11`

    **Fedora:** Fedora often includes many python versions. Try installing 3.11 using `dnf install python3.11`

    **Arch:** The aur will likely include any python version you'd ever need. If the system python is not 3.11, you should be able to install the `python311` aur package.

    If you can't find the required version any other way, you may have to build from source
    ```
    wget https://www.python.org/ftp/python/3.11.9/Python-3.11.9.tar.xz
    tar -xf Python-3.11.9.tar.xz
    cd Python-3.11.9
    ./configure --prefix=/usr/local
    make -j$(nproc)
    sudo make install
    ```


## Drive Station and Deploy Tool

Next, there are two ArPiRobot specific tools that need to be installed (regardless of which programming language you will use).

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
