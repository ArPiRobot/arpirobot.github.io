# Default Settings

A list of all default settings for the image

## Images

### Default Network Settings

- Generated WiFi Network
    - Channel 6 (2.4G)
    - Country: US<sup>&ast;</sup>
    - SSID: `ArPiRobot-Robot`
    - Password: `arpirobot123`
    - Pi's IP Address: `192.168.10.1`
- Pi's Hostname (any network): `ArPiRobot-Robot`
    - If connected to pi's generated wifi network add `.local` to the end of the Pi's hostname

    <br />

    <sup>&ast;</sup>If you are not in the US make sure to change this or channels outside of regional restrictions may be enabled on the Pi. The default channel used by the image (channel 6) should be allowed in most regions. The country can be changed using the deploy tool or by logging into the Pi and editing `/etc/hostapd/hostapd.conf`.

### Default Username and password
- Username: `pi`
- Password: `arpirobot`
