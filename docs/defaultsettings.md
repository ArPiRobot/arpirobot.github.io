# Default Settings

A list of all default settings for the ArPiRobot OS Image

**Default WiFi Network Settings**

- Channel 6 (2.4G)
- Country: US<sup>&ast;</sup>
- SSID: `ArPiRobot-Robot`
- Password: `arpirobot123`
- Pi's IP Address: `192.168.10.1`
- Pi's Hostname: `ArPiRobot-Robot`

    <sup>&ast;</sup>If you are not in the US make sure to change this or channels outside of regional restrictions may be enabled on the Pi. The default channel used by the image (channel 6) should be allowed in most regions. The country can be changed using the deploy tool or by logging into the Pi and editing `/etc/hostapd/hostapd.conf`.

**Default Username and Password**

- Username: `pi`
- Password: `arpirobot`
