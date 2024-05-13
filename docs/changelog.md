# Changelog

## Framework Version 1.1 (All Components)

1.1x Is a new minor release. There are no major changes to the fundamental architecture of the framework, however there are some significant changes to how things are done.

1. CoreLib Build Changes

    - Now builds using LLVM Clang and LLD instead of GNU toolchain
    - Since LLVM is natively a cross toolchain, there is no longer a need to build custom toolchains (hence, the ArPiRobot-Toolchains component / repo is archived - it is no longer necessary)
    - Instead of installing toolchains, you must install a sysroot for the architecture you are building for (Deploy tool can do this - see following sections)
    - CMake presets are now used instead of having to choose the correct toolchain file
    - Ninja is used to build instead of Make
    - CoreLib now finds dependencies (libraries) from system packages (or in sysroots) instead of building them from source

3. C++ Robot programs are built using LLVM, CMake presets, and Ninja just as is described for the CoreLib.

    - You will need to update existing C++ robot projects to use these new build system components. The easiest way to do this is likely to generate a new project and copy your source code to that new project.

4. Python Robot programs development changes
    
    - Projects include a `pyrightconfig.json` specifying python version to be used in error checking. This makes it easier to use a newer version of python on the development computer than is used on the robot.
    - Projects include a `requirements.txt` file used to create a virtual environment with the arpirobot core lib package installed
    - Projects are developed using a virtual environment (the vscode extension will automatically prompt you to create it)
    - The VSCode extension now has a command to (Re)Create the python environment

4. Supported Boards Changed

    - There are no changes to supported Raspberry Pi Boards
    - OrangePi Lite and OrangePi 3 LTS are no longer supported
    - OrangePi 3B and OrangePi Zero 2W are now supported

5. OS Image and ImageScript Changes

    - All OS images are now based on Debian Bookworm
    - ImageScripts generation method completely overhauled and almost fully automated
    - ImageScripts also generate sysroots necessary for cross compiling the CoreLib and robot programs
    - ImageScripts write board-specific files telling the CoreLib which I2C and SPI port numbers are the "defaults" which typically means they should be used with RPi hats.
    - The ArPiRobot-Tools component is now consolidated into the ImageScripts component.
    - Added scripts to all images allowing configuration of WiFi band (2.4GHz / 5.0GHz)
    - Split out and improved regulatory domain configuration scripts used on all images
    - Ethernet on images for boards with ethernet ports will run a DHCP server now.
    - Images now use newer mediamtx build (formerly rtsp-simple-server) 

6. Different IO Providers in CoreLib
    - *Note: Users don't likely need to worry about this change. The correct IO provider will still be selected based on which board you are using.*
    - pigpio is still fully supported
    - lgpio is supported
    - libsoc support has been removed (lgpio replaces)
    - serial support has been removed (libserialport replaces)

6. Camera streaming completely redone 

    - The old CameraStreaming component, service, and scripts are no longer used. Instead camera streams are started by the robot program (using the CoreLib)
    - The CoreLib now has objects for cameras using the V4L2, libcamera, and rpicam backends. Note: The rpicam backend uses the rpicam-vid tool (libcamera stack) not the old rpicam stack.
    - Because streams are now managed through the CoreLib, it is now possible for robot programs to receive images from the camera while it is simultaneously being streamed. This potentially allows use of OpenCV (now available on all OS images and a dependency of the CoreLib) to use camera frames in robot code
    - Camera streams only support RTSP (no more TCP or UDP support)

7. Deploy Tool Changes

    - "This PC" tab now shows LLVM, Ninja, and pkg-config versions instead of Make versoin
    - "This PC" tab now shows installed sysroots instead of toolchains
    - "This PC" tab now allows installation of sysroot packages instead of toolchain packages
    - Supports newer WiFi configuration scripts (added by ImageScripts) allowing control over WiFi band
    - WiFi configuration UI improved and populated valid options given the current configuration (eg regulatory domain)
    - Fixed a bug that prevented remote connection loss from being detected on Linux and macOS
    - Download links for various tools removed from UI
    - Camera Stream tab redone to only handle stream playback (as stream configuration now occurs in robot code)

7. Misc changes

    - Added a robotStopped function to the CoreLib's BaseRobot class. This method is called when the robot program is about to stop allowing the user to cleanly close any resources they have manually opened.

    - CoreLib's INA260 no longer uses a thread pool thread, thus use of this device will no longer impact availability of threads for tasks on the thread pool's scheduler.

    - Building the CoreLib (and thus robot programs) to run on Windows or macOS is no longer supported. You can still build to run on any Linux system for development testing. Most users will still have no reason to do this.

    - CoreLib uses board-specific information (from ImageScripts) to determine the default I2C and SPI busses. Functions to get these busses are now exposed on the `Io` class in both C++ and Python.

    - The VSCode extension has been updated to generate projects using the new C++ build system setup

    - Robot projects generated using the VSCode extension use an updated `main.sh` to preserve environment when running the robot program. If you need this, update your `main.sh` with the new one (this applies to both C++ and Python, but most users will not care about this).

    - (PLANNED) Bug fixed where drive station failed to detect remote disconnects on Linux

    - Removed OPi Lite variant of Mini Clipboard Example build from docs and CAD models

    - (PLANNED) Updated all examples in ArPiRobot-Examples to use new build systems (C++)

    - Added Pinouts for all supported boards on docs site

    - Updated existing pages on docs sites with correct install instructions and guides for the changes in v1.1

    - Drive Station: Fixed a bug that prevented remote connection loss from being detected on Linux and macOS
