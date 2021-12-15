# Running Raspberry Pi OS Image in QEMU

**Sources:**

[https://github.com/dhruvvyas90/qemu-rpi-kernel](https://github.com/dhruvvyas90/qemu-rpi-kernel)

**Instructions:**

```sh
sudo apt install qemu-system-arm qemu-kvm libvirt-clients libvirt-daemon-system bridge-utils virtinst libvirt-daemon virt-manager
```

Download raspios image from [here](http://downloads.raspberrypi.org/raspios_lite_armhf/images/) or older raspbian images from [here](http://downloads.raspberrypi.org/raspbian_lite/images/). Extract the zip.

Download kernel and dtb from [here](https://github.com/dhruvvyas90/qemu-rpi-kernel). You can also follow the instructions there to use qemu's rpi model, however on Ubuntu 20.04 QEMU 5.1 is not packaged, thus this method is more accessible.

Emulate

```sh
sudo qemu-system-arm \
  -M versatilepb \
  -cpu arm1176 \
  -m 256 \
  -hda ~/qemu-pi/2020-05-27-raspios-buster-lite-armhf.img \
  -net user,hostfwd=tcp::5022-:22 \
  -dtb ~/qemu-pi/versatile-pb-buster.dtb \
  -kernel ~/qemu-pi/kernel-qemu-4.19.50-buster \
  -append 'root=/dev/sda2 panic=1' \
  -no-reboot
```


TODO: How to resize image and use to construct arpirobot image
