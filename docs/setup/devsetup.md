# Setup your Computer

As mentioned in the previous section there are two ArPiRobot tools that run on your PC that allow you to control/drive the robot (Drive Station) and configure the robot (Deploy Tool).

In addition this guide will cover setting up Visual Studio Code to write python code. There is also an ArPiRobot extension available for VSCode that allows creating projects and installing the PythonLib on your PC (more about updates in the next section).

## Installing Python (if you plan to use Python)

ArPiRobot code can be written in python using the ArPiRobot python library, so we must first install python. You will want to make sure you are using the same version of python on your PC as is in use on the robot (you can see which version is in use on the robot on the [Downloads Page](../downloads/latest.md) in the table with information about the robot images.) Only the first two numbers need to match (for example if the robot uses python 3.7 it is OK to use 3.7.x where x is any number on your computer.)

### Windows or macOS
On windows or macOS installers for specific versions of python can be found at [Python.org](https://www.python.org/downloads/). Download the latest version where the first two numbers match the version of python in use on the raspberry pi. In some cases an installer may not be provided for the latest version (only the Sources can be downloaded). In this case look at the next newest version and download the installer for it (for example if 3.7.7 did not have an installer available for download I would look at 3.7.6 then 3.7.5 and so on).

Download and run the installer. Follow the on-screen instructions. On macOS make sure to read the section about Certificate Verification and SSL and follow the instructions. You will also need to make sure your mac allows installation of apps that are not form the app store.

### Linux / *BSD
The process of installing a specific python version will vary with Linux distributions (or BSD OSes). Pyenv should be able to be used. Just google instructions.

**Ubuntu**

Ubuntu users can use a ppa repo providing many python versions.

```
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.7 libpython3.7-dev
```

This will install a `python3.7` executable.

** *Note: In the above example python 3.7 is installed. Make sure to install the same version of python which is used on the robot (this is based on the Raspberry Pi Image used).* **

## Installing a Java Development Kit (if you plan to use Java)

You can also use Java to program your robot. Before doing so, you will need a Java Development Kit (JDK) installed on your computer. This will include the tools needed write and compile Java programs.

You should make sure you are using the same Java version as is in use on the robot (with Java a newer version is usually OK, but it is still recommended to use the same version). See the images section on the [downloads](../downloads/latest.md) page for details. 

Once you know which version of Java you want to download go to [adoptopenjdk.net](https://adoptopenjdk.net/) to download Java for your computer&ast;.

Download the installer for the correct version of Java (use the default "HotSpot" JVM). On Windows, this will be a `.msi` file. Run it and follow the directions to install Java. On macOS this will be a `.pkg` file. Run it and follow the directions to install Java.

&ast;If using a Linux Distribution you can probably install a JDK using the system's package manager. For example on Ubuntu, the following commands install Java 8 and 11 respectively.

```
sudo apt install openjdk-8-jdk
sudo apt install openjdk-11-jdk
```

## Installing Visual Studio Code
There are many development environments for writing python code, but for ArPiRobot we will focus on Visual Studio Code (VSCode). It provides intelligent code suggestions and error detection, and there is an ArPiRobot extension for VSCode that can help create projects and install ArPiRobot updates on the development PC.

Download and install VSCode from [https://code.visualstudio.com/](https://code.visualstudio.com/).

After installing it, open VSCode. A few extensions will be required/helpful. First, if using Python, install Microsoft's Python extension. If using Java install the "Java Extension Pack".

Then (for any language) download the ArPiRobot VSCode Extension (see the [downloads page](../downloads/latest.md)). Once you've downloaded the `vsix` file open VSCode's extensions pane (left side). Click the three dots menu button in the top right of the extensions pane. Choose install form VSIX and choose the downloaded VSIX file.

*Note: The first time you open a Python file you may be asked to install PyLint, my-py or another linter. If asked, allow the installation. These are tools used for error detection.*

## Installing the Drive Station and Deploy Tool

### Windows
Download the exe installers for both (see [downloads page](../downloads/latest.md)). Run the installer. A Java runtime will be installed with each program. The installer will create a start menu shortcut (and optionally a Desktop shortcut) as well.

### macOS
Download the `.zip` macOS packages (see [downloads page](../downloads/latest.md)). Extract the zip files and move the resulting `.app` files to the `Applications` folder. Each app comes with its own Java runtime.

### Ubuntu Linux
A `.deb` package has been provided for Ubuntu. It has generally been tested on the latest LTS release. While not guaranteed it should still work on other ubuntu releases or other distributions that use deb packages. If you have trouble with the deb package just install java and use the `.jar` file.

### Other
This method will work on any platform where Java is available (Linux, *BSD, Windows, macOS, etc). First install Java 11 (or newer). Then download the `.jar` files (see [downloads page](../downloads/latest.md)). Most of the time, if Java is installed, double clicking the `.jar` file will run the program. If not, the command below can be used

```
java -jar /full/path/to/jar/file.jar
```

Note: For &ast;BSD users the drive station does not include a build of the required native library. You will have to build it yourself. You can find instructions on doing so in the Drive Station's GitHub repository. The only platforms that have builtin support are: Windows (32-bit), Windows (64-bit), Linux (64-bit), macOS (64-bit).
