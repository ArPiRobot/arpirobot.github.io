
The ArPiRobot Framework is designed to make building and programming a robot easy for beginners, while still being sufficiently flexible to allow support complex designs and software. This guide is intended to introduce important parts of the ArPiRobot Framework, as well as introduce key robotics concepts and walk you through building a basic robot.

This guide is written with beginners in mind. For those with more robotics experience, some sections can be skipped, however this guide will still provide important information specific to the ArPirobot Framework.


## Guide Outline

This Guide will cover the following topics

- Building a Robot: Covers what is necessary to build a custom robot to use with the ArPiRobot Framework. Also includes some example build with instructions to create them.
- Software Setup: Covers setting up software on your computer to program the robot and setting up the computer on the robot for use.
- Programming the Robot: Covers using various tools to program the robot and writing code to program the robot. This section is not a tutorial on any programming language. It covers how to use ArPiRobot CoreLib features to program the robot.
- Other Tasks: This section is a collection of various standalone guides to perform specific tasks (unrelated to each other or other sections of this guide).

It is intended that the first three sections of this guide be followed in order.


## Required Items

Before getting started with the ArPiRobot Framework you will need the following items. These are in addition to any parts and tools required to actually build the robot.:

- A computer running Windows, macOS, or Linux. This computer will be used to write robot code and run the Drive Station.
- Your computer will need to be capable of connecting to a WiFi network. Laptops will almost always have WiFi builtin, however many desktops do not. If you computer does not support WiFi, a USB WiFi adapter can be used instead. Cheap ones such as the [TP-Link TL-WN725N](https://www.amazon.com/TP-Link-wireless-network-Adapter-SoftAP/dp/B008IFXQFU) should work, however you may get better range with one with an external antenna such as the [TP-Link AC600](https://www.amazon.com/dp/B07P5PRK7J/).
- You computer will need a micro SD card slot. Many laptops have these builtin (some desktops do too). If your computer has a full size SD card slot that should also work as most micro SD cards come with adapters to connect them to a full size SD slot. If you do not have an SD card slot, you can use a USB micro SD card adapter such as [this one](https://www.amazon.com/Sabrent-SuperSpeed-Windows-Certain-Android/dp/B00OJ5WBUE/).
- To control the robot, you will need a gamepad that can connect to your computer. You may be able to use gamepads from various consoles if you have them, but the steps to use these vary with controller type and operating system your computer runs. You could also use a controller made for PC's such as the [Logitech F310](https://www.amazon.com/Logitech-940-000110-Gamepad-F310/dp/B003VAHYQY). Some console controller info is listed below. Generally, PlayStation controllers are easy to get working on any OS and XBox controllers are easy on Windows. Newer Bluetooth XBox One controllers are easy to get working on any OS.

??? "Console Controller Compatibility Info"

    *The table below may be incorrect, or may change over time. It is highly recommended to test your controller with the ArPiRobot Drive Station to confirm it works before planning on using it. Controllers can be tested by installing the drive station, connecting the controller. If the controller does not show up in the list, it is not functioning. If it shows up, select it in the list (click the name) and move axes / press buttons to make sure the drive station shows the state of the controller. If axis / button indicators do not change, the controller is not functioning.*

    | Controller           | Windows             | macOS             | Linux             |
    | -------------------- | ------------------- | ----------------- | ----------------- |
    | PS3 Controller       | USB and Bluetooth work with [third party software](https://vigem.org/projects/DsHidMini/) | USB and Bluetooth work | USB and Bluetooth work |
    | PS4 Controller       | USB and bluetooth work | USB and Bluetooth work | USB and Bluetooth work |
    | PS5 Controller       | USB and bluetooth work | USB and Bluetooth work | USB and Bluetooth work |
    | Original Xbox Controller | USB works with adapter | USB works with adapter and [third party software](https://github.com/360Controller/360Controller) | USB works with adapter | 
    | Xbox 360 Wired Controller | USB works | USB works with [third party software](https://github.com/360Controller/360Controller) | USB works |
    | Xbox 360 Wireless Controller | Works with Xbox 360 gaming receiver | Not easily supported even with [third party software](https://github.com/360Controller/360Controller) | Works with gaming adapter with [third party software](https://github.com/paroj/xpad) |
    | Xbox One Controller | USB works. Wireless adapter works | USB works with [third party software](https://github.com/360Controller/360Controller) | USB works. Wireless using adapter requires [third party software](https://github.com/medusalix/xone) |
    | XBox One Controller with Bluetooth Support | Bluetooth works. USB requires [third party software](https://github.com/360Controller/360Controller) | USB and Bluetooth Work | USB and Bluetooth work |

- Alternatively, instead of a gamepad you can use the Mobile Drive Station on an Android phone or tablet instead of running the PC Drive Station on your computer. However, the Mobile Drive Station currently lacks some features of the PC Drive Station and does **not** support iOS. As such, a controller that works with your computer is generally recommended.
