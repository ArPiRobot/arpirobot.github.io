# Building an ArPiRobot Image
This guide details how the prebuilt Raspbian image is created for ArPiRobot robots. This guide exists mostly to document the image creation process, but can be followed to create a custom Raspbian image or an image with another Linux OS. 

## Why Would I need to build an Image?
There are few reasons to ever need a custom image. Most users do not need to spend the time creating their own image. Some reasons for wanting to build a custom image are:

1. If you want to use or test a newer version of Raspbian that the prebuilt image uses.
2. If you want to use a different Linux OS (not Raspbian).
3. If you want to understand more about how all the components of this project fit together.

It is important to be aware that this guide is not always kept up to date. It is mostly intended as a reference. 

There will likely be slight differences if using another version of Raspbian or another Linux OS. Some important things to remember if using another Linux OS: there must be a `pi` user (or systemd services and `dt-` script must be edited) and the `pi` user (or whatever user is used with the deploy tool) must be able to use `sudo` with no password.

## Raspbian Version
This guide is currently up to data for the Beta6 image (2020-02-13-raspbian-buster-lite). The resulting image has been tested on a Raspberry Pi Zero W and a Raspberry Pi 3A+.

## Base Raspbian Install
- Download the Raspbian Image from [here](https://www.raspberrypi.org/downloads/raspbian/). If the correct version is no longer available it is likely OK to use a newer version, but older images can be found [here](http://downloads.raspberrypi.org/raspbian_lite/images/) or [here](http://downloads.raspberrypi.org/raspios_lite_armhf/images/).
- Download and install [balenaEtcher](https://www.balena.io/etcher/)
- Install balenaEtcher
- Insert the SD Card into an SD Card slot on your PC or connect it using a USB adapter.
- Run balenaEtcher
- Select the downloaded raspbian image
- Select the destination SD Card
- Click flash and wait for it to finish
- Finally, move the SD Card into the Pi, connect keyboard, mouse, monitor and power on the pi.

## Automatic process
Instead of following the instructions below, the [configuration scripts](https://github.com/ArPiRobot/ArPiRobot-ImageScripts) can be used. These scripts are used to generate the images and are usually more up to date than this guide, but there is no gaurentee they will work correctly for different Linux OSes or different versions of raspbian that the latest image released.

To view the script log (live)

```
sudo tail -f /root/setup_log.txt
```

Everything that follows is the manual setup instructions.

## Manual process

Note: Unless otherwise stated all commands are run as root in the pi user's home directory

#### Stage 1
- Create the text file indicating the image version name

            echo "VERSION_NAME" > /usr/local/arpirobot-image-version.txt

- Set a fixed resolution for HDMI (optional)
    - Edit `/boot/config.txt` and find the following lines


            #hdmi_group=1
            #hdmi_mode=1
            
    
    - Change them to 

            hdmi_group=1
            hdmi_mode=4

- Update apt repos and upgrade packages to their latest versions

        apt update
        apt upgrade

- Once finished reboot

        reboot

#### Stage 2
- Make sure dpkg had nothing left to do after upgrading packages
        
        dpkg --configure -a

- Install git

        apt install git

- Clone the rpi-readonly repo

        cd /home/pi
        git clone https://gitlab.com/larsfp/rpi-readonly.git

- Run the readonly script (note: there are different scripts for different raspbian versions. Make sure to use the right one!)

        cd /home/pi/rpi-readonly
        ./setup.sh

- Reboot

        reboot

#### Stage 3
- Remount read/write

        mount -o rw,remount /
        mount -o rw,remount /boot

- Change the Pi user's password
    - Run
        ```
        passwd pi
        ```
    - Enter the new password (default is `arpirobot`) when prompted

- Change the system hostname to the default (`ArPiRobot-Robot`)
    - Edit `/etc/hostname`
    - Replace the contents with the new hostname
    - Edit /etc/hosts
    - Replace instances of `raspberrypi` with the new hostname

- Change to US locale
    - Edit `/etc/locale.gen`
    - Comment out the default `en_GB.UTF-8` locale
    - Uncomment the `en_US.UTF-8` locale
    - Run

            locale-gen en_US.UTF-8
            localectl set-locale LANG=en_US.UTF-8
            localectl set-locale LC_CTYPE=en_US.UTF-8
            localectl set-locale LC_NUMERIC=en_US.UTF-8
            localectl set-locale LC_TIME=en_US.UTF-8
            localectl set-locale LC_COLLATE=en_US.UTF-8 
            localectl set-locale LC_MONETARY=en_US.UTF-8
            localectl set-locale LC_MESSAGES=en_US.UTF-8 
            localectl set-locale LC_PAPER=en_US.UTF-8
            localectl set-locale LC_NAME=en_US.UTF-8
            localectl set-locale LC_ADDRESS=en_US.UTF-8
            localectl set-locale LC_TELEPHONE=en_US.UTF-8
            localectl set-locale LC_MEASUREMENT=en_US.UTF-8
            localectl set-locale LC_IDENTIFICATION=en_US.UTF-8

- Change the keyboard layout
    - Use `raspi-config` and select the correct layout in the UI
    - **OR** run the following command

            raspi-config nonint do_configure_keyboard us

- Enable interfaces (SSH, SPI, I2C, Camera)
    - Use `raspi-config`'s interactive menues
    - **OR** run the following commands

            raspi-config nonint do_spi 0 
            raspi-config nonint do_i2c 0
            raspi-config nonint do_ssh 0
            raspi-config nonint do_camera 0

- Enable console UART
    - Edit `/boot/config.txt` add the following at the end

            enable_uart=1

- Disable the systemd-rfkill service (the service fails to start with a readonly filesystem so all this does is speed up the boot process)
    
        systemctl disable systemd-rfkill.service
        systemctl mask systemd-rfkill.service
        systemctl disable systemd-rfkill.socket
        systemctl mask systemd-rfkill.socket

- Disable daily apt update and upgrade services

        systemctl disable apt-daily.service
        systemctl disable apt-daily-upgrade.service

- Install sysstat (used by ArPiRobot-Tools scripts to get CPU usage)

        apt install sysstat

- Install required software before disconnecting from internet (WiFi AP setup will do this)

```sh
# Python for running robot programs
apt-get -y install python3 python3-pip python3-setuptools python3-setuptools-scm python3-wheel

# Java for running robot programs
apt-get -y install openjdk-8-jdk-headless

# GPIO libraries
apt-get -y install pigpio pigpiod pigpio-tools wiringpi

# Required for simpleaudio python module
apt-get -y install libasound2-dev

# For wifi bandwidth testing
apt-get -y install iperf3

# For camera streaming
apt-get -y install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-doc gstreamer1.0-tools gstreamer1.0-alsa gstreamer1.0-pulseaudio
```

- Install ArPiRobot-Tools

```sh
cd /home/pi
git clone https://github.com/ArPiRobot/ArPiRobot-Tools.git
cd ArPiRobot-Tools
chmod +x ./install.sh
./install.sh
```

- Install hostapd (Access point) and dnsmasq (DHCP server) for robot's WiFi AP

        apt install hostapd dnsmasq

- Replace the contents of `/etc/dnsmasq.conf` with the following (create the file if it does not exits).


        interface=wlan0
        dhcp-range=192.168.10.2,192.168.10.20,255.255.255.0,24h
        domain=local
        address=/ArPiRobot-Robot.local/192.168.10.1


- Replace the contents of `/etc/hostapd/hostapd.conf` with the following (create the file if it does not exits)

        country_code=US
        ieee80211d=1
        interface=wlan0
        ssid=ArPiRobot-RobotAP
        hw_mode=g
        channel=6
        macaddr_acl=0
        auth_algs=1
        ignore_broadcast_ssid=0
        wpa=2
        wpa_passphrase=arpirobot123
        wpa_key_mgmt=WPA-PSK
        wpa_pairwise=TKIP
        rsn_pairwise=CCMP
        wmm_enabled=1


- Add the following to `/etc/default/hostapd`

        DAEMON_CONF="/etc/hostapd/hostapd.conf"

- Unblock wifi

        sudo rfkill unblock wlan



- Make /var/lib/misc use a ramdisk (fix dnsmasq on readonly filesystem)

    - Edit `/etc/fstab` and add the following

            tmpfs /var/lib/misc tmpfs nosuid,nodev 0 0
    
- Configure dhcpcd
    - Edit `/etc/dhcpcd.conf` and add the following

            interface wlan0
                static ip_address=192.168.10.1/24
                nohook wpa_supplicant

- Enable hostapd on boot

        systemctl unmask hostapd
        systemctl enable hostapd

- Reboot

#### Stage 4
- Remount RW

        mount -o rw,remount /
        mount -o rw,remount /boot

- Run console-setup once with read/write (only necessary after keyboard layout is changed)

        systemctl restart console-setup

- Create the robot program directory

        mkdir /home/pi/arpirobot
        chown pi:pi /home/pi/arpirobot

- Cleanup
    - If you connected the pi to wifi for setup clear the wifi settings from `/etc/wpa_supplicant/wpa_supplicant.conf`
    - Delete any ssh keys used from `/home/pi/.ssh` and/or `/root/.ssh`

#### Final Stages
- Test everything to make sure it works now
- Delete SSH host keys (these should regenerate on each pi so they cannot be in the image)
- Patch the ssh-regenerate-host-keys service to work with the readonly filesystem
    - Edit `/lib/systemd/system/regenerate_ssh_host_keys.service`
    - Add the following two lines to the [Service] Section

            ExecStartPre=/bin/mount -o rw,remount /
            ExecStartPost=/bin/mount -o ro,remount /
    
    - Enable the patched service

            systemctl daemon-reload
            systemctl enable regenerate_ssh_host_keys
    
- Setup to expand the filesystem to fill the SD card on next boot (so the image can be dumped as small as possible)
    - Append the following to the end of the first line in `/boot/cmdline.txt`

            quiet init=/usr/lib/raspi-config/init_resize.sh
    
    - Patch the script to work with readonly filesystem
        - Edit `/usr/lib/raspi-config/init_resize.sh` and replace

            mount /boot/

        with

            mount -o rw,remount /boot/
    
    - Write the resize filesystem service
        - Edit (create new file) `/etc/init.d/resize2fs_once` with the following contents

                #!/bin/sh
                ### BEGIN INIT INFO
                # Provides:          resize2fs_once
                # Required-Start:
                # Required-Stop:
                # Default-Start: 3
                # Default-Stop:
                # Short-Description: Resize the root filesystem to fill partition
                # Description:
                ### END INIT INFO
                . /lib/lsb/init-functions
                case "\$1" in
                start)
                    log_daemon_msg "Starting resize2fs_once"
                    mount -o rw,remount / &&
                    ROOT_DEV=\$(findmnt / -o source -n) &&
                    resize2fs \$ROOT_DEV &&
                    update-rc.d resize2fs_once remove &&
                    bash -c "sleep 5;mount -o ro,remount /" &
                    rm /etc/init.d/resize2fs_once &&
                    log_end_msg $?
                    ;;
                *)
                    echo "Usage: $0 start" >&2
                    exit 3
                    ;;
                esac
    
    - Enable the new service at next boot

            chmod +x /etc/init.d/resize2fs_once
            update-rc.d resize2fs_once defaults

- Cleanup

        rm -rf /home/pi/ArPiRobot-ImageScripts
        rm -rf /home/pi/ArPiRobot-Tools
        rm -rf /home/pi/rpi-readonly

        rm /root/.bash_history
        history -c
        rm /home/pi/.bash_history

- Poweroff (do not reboot becasue the one-time filesystem expand will run on next boot)

        poweroff
