# Drive Station Manual

The ArPiRobot Drive Station is the program for your computer (Windows, macOS, Linux) that is used to control the robot. Its primary function is to enable/disable the robot and send data from a gamepad connected to your computer to the robot via WiFi.

![Drive Station Screenshot](../img/drive_station.png){: style="height:300px"}

## Uses / Features
- Connect to a running robot program over the Raspberry Pi's WiFi network
- Enable and Disable the Robot
- Show the status (enabled / disabled) of the robot
- Show and test connected gamepads
- Use one or more gamepads to control the robot
- Show logs sent from the robot after connecting the drive station
- Show the value of NetworkTable key / value pairs form the robot
- Create or edit NetworkTable key / value pairs on the robot
- Save and load configurations of shown NetworkTable values and their sizes / positions on the screen

## Installing and Running
The Drive Station is written in Java (at the time of writing this it should work with Java 1.8 or newer), so you will need to make sure you have a Java runtime installed before using the Drive Station. Sometimes, a Java runtime is included in the package and sometimes it is not.

The Drive Station is built and available for download in the following formats

- `.exe` file - This is a windows installer that will install the Drive Station along with a Java runtime. It will also setup shortcuts in the start menu and configure an uninstaller for the Drive Station accessible through the Windows Control Panel.
- `.app.zip` - This is a zip file containing a macOS app for the drive station. After extracting the zip you can drag the `.app` file to your Mac's `Applications` folder so the Drive Station will show up in Launchpad. This app includes a java runtime used by the Drive Station.
- `.deb` file - This is a package that can be used to install the Drive Station on Ubuntu Linux (it should also work on other debian based systems using `.deb` packages). It depends on the `default-jre` for whatever distribution you are installing it on so you should not need to do anything special to install a Java runtime.
- `.jar` file - This is an executable Java Archive file. This can be run on any supported<sup>&ast;</sup> platform, but you must install java separately first. Usually after installing java you can run the Drive Station just by double clicking on the `.jar` file, however if that does not work you may need to right click it and choose to open it with "Java" or "Java Runtime". If all else fails make sure the `java` command is in your path and run the following command in a terminal or command prompt: `java -jar /path/to/jar_file.jar`. Replace the path and name of file as appropriate.

<sup>&ast;</sup>Supported platforms are 32-bit and 64-bit Windows (7 or newer), 64-bit macOS, 64-bit (x86_64) Linux. Other platforms will require a custom build of the native library used by the Drive Station to manage gamepads.

## Connecting to the Robot
Before attempting to connect to the robot you will need to make sure your computer is connected to the WiFi network created by the robot. You will also generally need to make sure you are disconnected from other networks (including ethernet connections).

Once your computer is connected to the robot's WiFi click the "Connect" button at the bottom of the drive station window to connect to the running robot program. If there is no robot program running the drive station will not connect and you will need to use the [Deploy Tool](./deploy_tool.md) to put a program on the robot first. Once connected the "Connect" button will become a "Disconnect" button and can be clicked to disconnect the Drive Station from the running robot program.

Only one Drive Station can be connected to the robot at a time.

If you are not using the robot's WiFi network or have modified the network configuration on the robot such that the robot has a non-default IP address you can change the IP address used when connecting to the robot in the Drive Station's settings (`File > Settings`). Hostnames can also be used and will be resolved using DNS.

## Enabling and Disabling the Robot
Above the connect button there are two other buttons: one labeled "Disable" and another labeled "Enable". These disable and enable the robot respectively. If the robot is already in the matching state the robot will just ignore the command, however you will still be able to click the button in the Drive Station.

Above the Enable and Disable buttons there is a label that indicates the robot's state. It may read "Enabled", "Disabled", or "Not Connected" (if the Drive Station is not connected to the robot). It could also read "Unknown" if connected to the robot, but it is unknown whether the robot is enabled or disabled (this can happen when using older versions of the PythonLib or if network table issues occur).

## Selecting and Using Gamepads
When the "Controllers" in the bottom half of the Drive Station is selected you will see on the left side of the screen a section labeled "Controllers". Any gamepads connected to your PC will be listed in this section with their name beside a checkbox. 

You can drag the gamepads in the list to reorder them. The topmost gamepad will be gamepad 0. The next one will be gamepad 1, then gamepad 2, and so on. 

To "enable" a gamepad so that the drive station sends data from it to the robot it is connected to you will need to check the checkbox beside the gamepad's name. The Drive Station will send the data from each checked gamepad to the robot. Uncheck any gamepad to stop sending its data to the robot. Whether or not gamepads are checked does not change the number order of the gamepads. For example, if the top gamepad is unchecked, but the next one is checked the checked gamepad will still be gamepad number 1 and the robot will not receive data from gamepad 0 (even though it does receive data for gamepad 1).

If a gamepad is disconnected from the computer the Drive Station will uncheck it in the list and it will become "greyed out". It will keeps its position in the controller list so other gamepad's numbers do not change. You will not be able to check a "greyed out" gamepad. If a new gamepad with the same name is connected the gamepad will no longer be greyed out and you will be able to check its checkbox again to send data for it to the robot.

Beside the gamepad list there are a set of indicators that show the values of the axes, buttons, and dpad of the selected gamepad. To "select" a gamepad and see it's controls values in this section click on its name in the controller list. It will have a different background color to indicate that it is the selected controller.

## Reading Logs
There are two logs visible from the Drive Station: the robot log and the drive station log. The Drive Station log will show errors, warnings, and other messages from the Drive Station, while the robot log will show messages received from the robot (this only displays messages from the robot *after* the drive station connects).

There are four log levels. The lower the level the more detailed the information that will be shown. Both the robot and drive station logs have these same levels.

1. Debug
2. Info
3. Warning
4. Error

By default only info and higher level messages are shown, but this can be changed in the Drive Station's settings (`File > Settings`). The setting is called "Displayed Log Level". The selected level and any higher levels will be shown. By default Info is selected, which means that Info, Warning, and Error messages will be shown, but not Debug messages. When the displayed log level is changed the drive station will change the displayed portions of the logs accordingly. All messages received since connecting to the robot will be shown (even if they weren't before). This means that if you switch form info to debug you will see any debug messages since you last connected to the robot, even though they were hidden before. It does **not** only affect messages received after changing the setting.

## Main Battery Voltage
If a voltage monitor device is designated as the main voltage monitor on the robot (see [INA260PowerSensor]() or [VoltageMonitor]()) the Drive Station will display the voltage as determined by the sensor in the lower right hand corner of the Drive Station window. The battery voltage indicator will change color based on the percentage range of the voltage read compared to the nominal (as printed on label) battery voltage. The main battery voltage can be set in the Drive Station settings (`File > Settings`). This voltage should be the nominal voltage of the batteries in use. For example, with 5 AA batteries at 1.5V each the "Main Battery Voltage" should be 5 * 1.5 = 7.5. Most batteries exceed this value when fully charged or new (in the case of non-rechargeable batteries).

The colors of the indicator by percentage level are as follows

- 100% and above = Green
- 85%-99% = Yellow
- 70%-84% = Orange
- 70% and below = Red

Keep in mind that it is normal for the battery voltage to drop significantly when motor first start moving.

## NetworkTable Indicators
An indicator shows the value of a key / value pair from the Network Table. You can add indicators in two ways:

- By creating a new indicator (and if it does not exist a corresponding key / value pair in the Network Table)
- By adding an indicator for existing key / value pairs received from the robot or already known to the drive station

The top portion of the drive station is the indicators panel. In this boxed in area any indicators you add are shown and can be resized and moved around any way you want as long as the indicators panel is editable (`File > Editable`).

To create a new indicator click `Add > Network Table Indicator`. Then enter a key name in the dialog and click OK. You can only have one indicator for any key / value pair.

To add an indicator for an existing key / value pair select an existing key under `Add > Existing Keys`.

Once an indicator is on the panel it can be deleted (removed from the panel, but not the Network Table) by right clicking it and choosing delete. You can also edit the value of an indicator (and change the corresponding value on the Network Table) by right clicking and making the indicator editable. You will then be able to type values in the text field next to the indicator's label. Uncheck "Editable" in the right click menu to disable editing an indicator. Setting the value of an indicator is an easy way to send small amounts of information to the robot.

To resize an indicator click and drag any of the small squares at the edges and corners of the indicator. To move it click and drag the indicator's label (showing the key). Be aware that positions are "absolute" meaning if you resize the Drive Station window it is possible to move an indicator off the screen. For this reason it is recommended to keep the Drive Station the same size at all times. The two easy ways to do this are to use the default size or to always maximize the window (there is a setting that will start the Drive Station maximized in `File > Settings`).

Several menu options in the `File` menu are used with the indicator panel.

- `Clear` will remove all indicators form the panel
- `Open` will open a saved set of indicators and restore their positions/sizes on the indicators panel
- `Save` will save a set of indicators and their positions/sizes to a file to be loaded later (even after restarting the Drive Station). It will use the same file name as previously used, unless it has not saved or loaded before in which case it will perform a `Save As`.
- `Save As` will do the same thing as save, but will not use the same file name as last used.

## About Menu
Can be accessed by clicking `Help > About`. An about dialog will open up with information about the license of the Drive Station and the other software it makes use of. In the title bar of the dialog (as with the title bar of the main Drive Station window) the version number of the Drive Station is shown.

## Drive Station Themes
Java applications can have a variety of themes. Sometimes apps will try to emulate the theme of the operating system (windows, macOS, Linux, etc) so the app looks like other apps on the computer. Sometimes they will use a custom theme so they are the same across platforms. By default the Drive Station uses the Nimbus theme on all platforms. In the settings dialog (`File > Settings`) you can change this to a different universal theme (Metal) or to the system theme (which will change by platform).

## Closing the Drive Station
When closing the Drive Station you will see a dialog asking for confirmation before closing.