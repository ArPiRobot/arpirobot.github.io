# Building a Minimal ArPiRobot Image
This guide details how the prebuilt minimal Raspbian image is created for ArPiRobot robots. This guide exists mostly to document the image creation process, but can be followed to create a custom Raspbian image or an image with another Linux OS. The minimal image configured should be used when development will not be done on the robot or when the robot may be powered off unexpectedly.

## Why a Custom Image?
There are few reasons to ever need a custom image. Most users do not need to spend the time creating their own image. The most common reasons for wanting to build a custom image are:

1. If you want to use or test a newer version of Raspbian that the prebuilt image uses.
2. If you want to use a different Linux OS (not Raspbian).
3. If you want to understand more about how all the components of this project fit together.

It is important to be aware that this guide is only up to date for the version of Raspbian that was used to make the prebuilt image. There will likely be slight differences if using another version of Raspbian or another Linux OS. Some important things to remember if using another Linux OS: there must be a `pi` user (or systemd services must be edited) and the `pi` user (or whatever user is used with the deploy tool) must be able to use `sudo` with no password.

## Raspbian Version
This guide is currently up to data for Raspbian Lite version 2019-04-09. The resulting image has been tested on a Raspberry Pi Zero W and a Raspberry Pi 3A+.



## Base Raspbian Install
- Download the Raspbian Image from [here](https://www.raspberrypi.org/downloads/raspbian/). If the correct version is no longer available it is likely OK to use a newer version, but older images can be found [here](http://downloads.raspberrypi.org/raspbian_lite/images/).
- Download and install [balenaEtcher](https://www.balena.io/etcher/)
- Install balenaEtcher
- Insert the SD Card into an SD Card slot on your PC or connect it using a USB adapter.
- Run balenaEtcher
- Select the downloaded raspbian image
- Select the destination SD Card
- Click flash and wait for it to finish
- Once it finishes remove and re-insert the card to mount the boot partition. Edit the `config.txt` file and add the following line at the end. This will enable the serial console which can help with setup of the image and with debugging robot code or network issues while in use on a robot.

```
enable_uart=1
```

- Finally, move the SD Card into the Pi, connect keyboard, mouse, monitor and power on the pi.

## Automatic process
Instead of following the instructions below, the work-in-progress [configuration scripts](https://github.com/MB3hel/ArPiRobot-IamageScripts) can be used, but make sure to double check them! Once done with the scripts be sure to delete the downloaded/cloned repo. Also make sure to run each and every script *and in order*. If something goes wrong you will likely have to start over.

After running the scripts it is recommeded to delete the github repo for the scripts (from `/home/pi`) and to delete the log files from `/root/`. After doing so clear bash history again.

To view the script log (live)

```
sudo tail -f /root/setup_log.txt
```

Everything that follows is the manual setup instructions.

## Readonly Filesystem
*Tested using commit 1b596c2ced2a6ad87347cbb8faa675f20ec7af52* of `rpi-readonly` project.

The readonly filesystem should be configured on a clean install of Raspbian Lite. Login with username `pi` and password `raspberry`.

To setup a temporary WiFi configuration (this is not necessary if using an ethernet cable to access the internet is an option) run `sudo raspi-config` and select "Network Options" then "Wi-fi". Select the region, enter an SSID and password for the network to connect to.

After connecting to a network (either ethernet or wifi) run the following commands.

```
sudo apt install git
git clone https://gitlab.com/larsfp/rpi-readonly.git
cd rpi-readonly
sudo ./setup.sh
```

Do an apt update when prompted. When the script completes reboot. After rebooting you will need to go back into read/write mode. Run the following command to do so the continue with the reset of the setup. Remember that after every reboot during the setup root will need to be remounted read/write. The script will add an alias for this

To remount read/write run

```
rw
```

To make readonly again run

```
ro
```


## Initial Raspbian Setup

Remove the SD Card from your PC and insert it in the Raspberry Pi. Connect the power cable and wait for the pi to boot. The first boot may take a minute or two.

To change the hostname and password for the pi user run the following command.

```
sudo raspi-config
```

To change the user password select "Change User Password" and enter "arpirobot" without the quotes when prompted.

To change the hostname select "Network Options" then choose "Hostname". Change the hostname to "ArPiRobot-Robot" when prompted. *Note: if using a different hostname make sure to change it in the dnsmasq config file later and make sure the case matches.*

Under "Localization Options" change disable en_GB.UTF8 and enable en_US.UTF8. Make en_US.UTF8 the default locale. Optinally also change the timezone under "Localization Options". The default used for ArPiRobot images is United States Eastern Time (New York time).

After changing the hostname, password, locale, and timezone select Finish and choose "Yes" when asked to reboot.

After rebooting change the keyboard layout (still under "Localization Options") to "Generic 105 Key" > "Other" > "English (US)" > "English (US)". Keep all other settings default. *Remember to make the pi writable first!*

Next goto "Advanced Configuration" and change the default resolution to 1280x720 (helps to prevent HDMI flickering issues).

## Enable Interfaces

Run
```
sudo raspi-config
```

Under "Interfacing Options" enable SPI and I2C.

## Upgrade software
Make sure you are still connected to a network and run
```
sudo apt update && sudo apt upgrade
```

Then reboot

```
sudo reboot
```


## Remote Login (SSH)
To enable the SSH server for remote login access (if not already done by the file on the boot partition) run `sudo raspi-config` again and select "Interfacing Options" then select "SSH" and choose "Yes" to enable. Then, select "Finish".

## Wireless Setup

*WARNING: If setting the image up over a network (SSH) make sure not to reboot until all the following configuration is completed and verified!!!*

### Install ArPiRobot Raspbian Tools
The Raspiban Tools contains several helper scripts that manage networking. Install the raspbian tools before proceding.

```
cd ~
git clone git@github.com:MB3hel/ArPiRobot-RaspbianTools.git
cd ArPiRobot-RaspbianTools
chmod +x install.sh
sudo ./install.sh
```

### Run the start script on boot

Edit `/etc/rc.local` and add the following line before `exit 0`
```
/usr/local/bin/wifistart.sh
```


### Install Other Required Software

```
sudo apt install hostapd dnsmasq
```

Hostapd is used to host the access point and dnsmasq is a DNS and DHCP server.



### Configuration

The virtual adapter and all networking services will be created/started in the correct order by the `arpirobot-networking` service installed with the Raspbian tools.


#### Setup Dnsmasq and Hostapd
Edit `/etc/dnsmasq.conf`. Replace the contents with the following

```
interface=lo,ap0
server=8.8.8.8
domain-needed
bogus-priv
dhcp-range=192.168.10.2,192.168.10.10,255.255.255.0,24h
```

Create `/etc/hostapd/hostapd.conf` with the following contents

```
channel=11
ssid=AP_SSID
wpa_passphrase=AP_PASSWORD
interface=ap0
hw_mode=g
macaddr_acl=0
auth_algs=1
wpa=2
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
driver=nl80211
```

Edit ` /etc/default/hostapd` and change the `DAEMON_CONF` line to

```
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```

Edit `/etc/dhcpcd.conf`. Add the following to the end of the file

```
interface ap0
static ip_address=192.168.10.1
nohook wpa_supplicant
```

Finally to fix dnsmasq on readonly file system add the following line to `/etc/fstab`

```
  tmpfs /var/lib/misc tmpfs nosuid,nodev 0 0
```


#### Setup Client network
For now, use a real network here. Once the setup is completed change this to a dummy network so real wifi credentials are not being distributed.

Edit `/etc/wpa_supplicant/wpa_supplicant.conf`. Replace its contents with the following

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
        ssid="YOUR_SSID_HERE"
        psk="YOUR_PASS_HERE"
}
```

#### Configure Services

Disable networking services at boot as the custom `arpirobot-networking` service will handle starting them in the correct order
```
sudo systemctl stop hostapd
sudo systemctl stop dnsmasq
sudo systemctl stop dhcpcd
sudo systemctl disable hostapd
sudo systemctl disable dnsmasq
sudo systemctl disable dhcpcd
sudo systemctl unmask hostapd
```

Reboot and make sure both interfaces come up.

If both interfaces come up successfully it should be safe to work remotely after this point. No more network configuration will be done.

## Installing Software

### Python and Python Libraries

Run the following command to install Python 3 (arpirobot does not support python2).

```
sudo apt install python3 python3-pip python3-setuptools python3-wheel
```

To install required python libraries run the following command. A list of all required libraries is kept up to date [here](../pythonlibraries.md)

```
sudo pip3 install [SPACE SEPARATED LIST OF LIBRARIES HERE]
```

### Install the ArPiRobot Python Library

```
git clone git@bitbucket.org:MB3hel/arpirobot-pythonlib.git
cd arpirobot-pythonlib
sudo python3 setup.py install
```

## Setting up directory structure for arpirobot programs

```
mkdir ~/arpirobot
```

## Test the Image
Put the simple test program from the pythonlib samples into the `arpirobot` folder and set the filename in `main.txt`. Reboot, then connect with DS and make sure it is working correctly.


## Cleanup
Remove any git keys that were added to the system or any passwords that were saved with a credential manager.

Remove the real wifi settings from `/etc/wpa_supplicant/wpa_supplicant.conf`
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
        ssid="DUMMY_NETWORK"
        psk="DUMMY_PASSWORD"
}
```

Clear bash history

```
history -c
sudo history -c
```
