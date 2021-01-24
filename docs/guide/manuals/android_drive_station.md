# Mobile Drive Station Manual
There is currently no iOS version of the Mobile Drive Station available. An Android phone or tablet is required.

The mobile version of the [Drive Station](./drive_station.md) performs the same basic function: allowing you to control the robot, however it was created to allow you to control the robot without the use of a gamepad. The Mobile Drive Station includes an on-screen virtual gamepad which is used to control the robot.

Currently the Mobile Drive Station lacks some of the features present in the PC [Drive Station](./drive_station.md) such as editing key/value pairs on the network table and using multiple gamepads, however it does work for basic control of the robot.

## Uses / Features
- Connect to a running robot program over the Raspberry Pi's WiFi network
- Enable and Disable the robot
- Show the status (enabled / disabled) of the robot
- Use the on-screen virtual gamepad to control the robot
- Show logs sent from the robot after connecting the Mobile Drive Station
- Show the value of NetworkTable key / value pairs form the robot

## Installing
To install the Mobile Drive Station you will need an Android Device running Android KitKat (version 4.4) or newer. You can download the APK file (see the [downloads page](../../downloads/latest.md)) to install on your device. You will need to download this on the device you want to install it on. Then open the file. You will need to enable installing from "Unknown Sources" (meaning not the Play Store) in your settings first. Once installed you can find the app in your device's app drawer.

## Connecting to the Robot
Before attempting to connect to the robot you will need to make sure your device is connected to the WiFi network created by the robot. You will also generally need to make sure you are disconnected from other networks (including mobile data connections). Your device may notify you that there is no internet access and ask if you want to stay connected to the robot's WiFi network. If this happens choose to stay connected.

The connect button is the left-most button located along the ActionBar at the top of the app. Tap it to connect to the robot. Once connected this button will become a disconnect button which can be tapped again to disconnect from the robot. If the connect button is not visible check under the three-dots menu in the top right.

Only one Drive Station can be connected to the robot at a time (this includes PC Drive Stations and Mobile Drive Stations).

If you are not using the robot's WiFi network or have modified the network configuration on the robot such that the robot has a non-default IP address you can change the IP address used when connecting to the robot in the Drive Station's setting. The settings options is usually under the three-dots menu. Hostnames can also be used and will be resolved using DNS.

## Enabling and Disabling the Robot
At the bottom of the screen in the center there are two buttons: one labeled "Disable" and other labeled "Enable". These disable and enable the robot respectively. If the robot is already in the matching state the robot will just ignore the command, however you will still be able to click the button in the Mobile Drive Station.

Above the Enable and Disable buttons there is a label that indicates the robot's state. It may read "Enabled", "Disabled", or "Not Connected" (if the Drive Station is not connected to the robot). It could also read "Unknown" if connected to the robot, but it is unknown whether the robot is enabled or disabled (this can happen when using older versions of the PythonLib or if network table issues occur).

## Using the Virtual Gamepad
The on-screen virtual gamepad has two joysticks and fourteen buttons (four of which make up the D-Pad). The numbers for these joystick axes and buttons match those shown in the PC [Drive Station](./drive_station.md)'s Controllers tab, but are also listed below.

**Axes**

| Axis                 | Axis Number |
| -------------------- | ----------- |
| Left X Axis          | 0           | 
| Left Y Axis          | 1           |
| Right X Axis         | 2           | 
| Right Y Axis         | 3           | 
| L2 Button            | 4           |
| R2 Button            | 5           |

Just like with a controller where the triggers are digital buttons, the L2 and R2 axes will either be 0 or 1.0 when read by the robot.

**Buttons**

| Button               | Button Number |
| -------------------- | ------------- |
| A                    | 0             |
| B                    | 1             |
| X                    | 2             |
| Y                    | 3             |
| Back                 | 4             |
| Start                | 6             |
| L1                   | 9             |
| R1                   | 10            |

There is no button 5 as there is no "Guide" button on the virtual gamepad. There are no buttons 7 and 8 as the virtual gamepad's joysticks cannot be pressed as a button.

**Dpad**

Based on the direction buttons pressed on the virtual dpad the gamepad's D-Pad number 0 will have the following values

| Direction                 | Value |
| ------------------------- | ----- |
| None (Center)             | 0     |
| North                     | 1     |
| Northeast                 | 2     |
| East                      | 3     |
| Southeast                 | 4     |
| South                     | 5     |
| Southwest                 | 6     |
| West                      | 7     |
| Northwest                 | 8     |

Unlike the PC [Drive Station](./drive_station.md) the virtual gamepad is always "enabled", meaning the Mobile Drive Station will send data for the virtual gamepad to the robot whenever the robot is connected. Data will be sent as gamepad number 0.

## Reading Logs
There are two logs visible from the Mobile Drive Station: the robot log and the drive station log. The Mobile Drive Station log will show errors, warnings, and other messages from the Mobile Drive Station, while the robot log will show messages received from the robot (this only displays messages from the robot *after* the drive station connects).

To view logs tap the log button in the ActionBar (if it is not visible look under the three-dots menu). A screen with two tabs will open. One will display the robot log. The other will display the Mobile Drive Station log.

## Main battery voltage
If a voltage monitor device is designated as the main voltage monitor on the robot (see [INA260PowerSensor](./sensors.md) or [VoltageMonitor](./arduinosensors.md)) the Drive Station will display the voltage as determined by the sensor in a colored label above the status label. The battery voltage indicator will change color based on the percentage range of the voltage read compared to the nominal (as printed on label) battery voltage. The main battery voltage can be set in the Mobile Drive Station settings (look under the three-dot menu). This voltage should be the nominal voltage of the batteries in use. For example, with 5 AA batteries at 1.5V each the "Main Battery Voltage" should be 5 * 1.5 = 7.5. Most batteries exceed this value when fully charged or new (in the case of non-rechargeable batteries).

The colors of the indicator by percentage level are as follows

- 100% and above = Green
- 85%-99% = Yellow
- 70%-84% = Orange
- 70% and below = Red

Keep in mind that it is normal for the battery voltage to drop significantly when motor first start moving.

## Network Table Indicators
An indicator shows the value of a key / value pair from the Network Table. To add an indicator you will first need to open the Network Table screen (second icon from the left in the ActionBar or check under the three-dot menu).

Once open, the Network Table screen will have its own three-dot menu. Under this menu tap "Add Existing Indicator". This will show a list of Network Table keys known form the robot. Select one to add it to the network table view.

Indicators will appear in a list on the Network Table screen. If you leave this screen then return you will still have the same indicators in the list. To clear the indicators tap the "Clear Indicators" option in the three-dots menu.

Unlike with the PC [Drive Station](./drive_station.md) there is currently no way to view indicators and drive the robot at the same time. There is also no way to create new network table key/value pairs or edit existing ones.

## About Menu
The about menu can be accessed from the three-dot menu of the main screen. This will open the about screen which shows the Mobile Drive Station's license as well as the licenses of other software it makes use of.