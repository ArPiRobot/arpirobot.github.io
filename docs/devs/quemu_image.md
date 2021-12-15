# Running Raspberry Pi OS Image in QEMU

**Sources:**

[https://github.com/dhruvvyas90/qemu-rpi-kernel](https://github.com/dhruvvyas90/qemu-rpi-kernel)

**Instructions:**

```sh
sudo apt install qemu-system-arm qemu-kvm libvirt-clients libvirt-daemon-system bridge-utils virtinst libvirt-daemon virt-manager
```

Download raspios image from [here](http://downloads.raspberrypi.org/raspios_lite_armhf/images/) or older raspbian images from [here](http://downloads.raspberrypi.org/raspbian_lite/images/). Extract the zip.

Download kernel and dtb from [here](https://github.com/dhruvvyas90/qemu-rpi-kernel). You can also follow the instructions there to use qemu's rpi model, however on Ubuntu 20.04 QEMU 5.1 is not packaged, thus this method is more accessible.

Resize the raspios image so you have room to install stuff and work with the image.

```shell
# Grow the image by 1 GiB
dd if=/dev/zero bs=1MiB count=1024 >> ./2020-05-27-raspios-buster-lite-armhf.img

# Setup the image on a loop device
sudo losetup -f --show ./2020-05-27-raspios-buster-lite-armhf.img
sudo partprobe /dev/loop<number>

# Resize partition and filesystem. Could also use gparted
sudo parted -s -a opt /dev/loop<number> "resizepart 2 100%"
sudo e2fsck -f /dev/loop<number>p2
sudo resize2fs /dev/loop<number>p2

# Cleanup
sudo losetup -d /dev/loop<number>
```


Emulate using the following command

```sh
sudo qemu-system-arm \
  -M versatilepb \
  -cpu arm1176 \
  -m 256 \
  -hda ~/qemu-pi/2020-05-27-raspios-buster-lite-armhf.img \
  -net nic \
  -net user,hostfwd=tcp::5022-:22 \
  -dtb ~/qemu-pi/versatile-pb-buster.dtb \
  -kernel ~/qemu-pi/kernel-qemu-4.19.50-buster \
  -append 'root=/dev/sda2 panic=1'
```

You can then modify the image or use the image scripts to construct a new image. You can start ssh in the emulated pi and connect to ssh using localhost:5022 on the host machine.
