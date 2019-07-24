# Making an Image From an SD Card

## Linux
The image will be made using `dd`, but first the partitions on the SD card need to be shrunk to the smallest possible size. Use gparted to shrink the root partition on the SD Card.

Check the sector size and the end of the last partition
```
sudo fdisk -l /dev/mmcblk0
```

Output will look similar to
```
Disk /dev/mmcblk0: 29.8 GiB, 32010928128 bytes, 62521344 sectors
Units: sectors of 1 * 512 = 512 bytes                              
Sector size (logical/physical): 512 bytes / 512 bytes                     <----------------- THIS IS SECTOR SIZE
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x000bd3a3

Device         Boot   Start      End  Sectors  Size Id Type
/dev/mmcblk0p1         8192  3656250  3648059  1.8G  e W95 FAT16 (LBA)
/dev/mmcblk0p2      3656251 32546815 28890565 13.8G  5 Extended
/dev/mmcblk0p5      3661824  3727357    65534   32M 83 Linux
/dev/mmcblk0p6      3727360  3868671   141312   69M  c W95 FAT32 (LBA)
/dev/mmcblk0p7      3874816 32546815 28672000 13.7G 83 Linux                     <----------------- END SECTOR OF LAST PARITION
```

Sector size = 512 bytes

End of last sector = 32546815



For this dd operation will use block size (bs=) of 4M (this is 4 * 1024 * 1024 bytes).

So count will be (END_SECTOR * SECTOR_SIZE_IN_BYTES) / (BLOCK_SIZE_IN_BYTES) = blocks count



so (512 * 32546815) / (1024 * 1024 * 4)

so count is 3972.99987793 or 3973 (always round up)


To backup image from card
```
sudo dd bs=4M if=/dev/mmcblk0 of=/mnt/Files/rpi-image.img count=3973 status=progress
```

To restore to card

```
sudo dd bs=4M if=/mnt/Files/rpi-image.img of=/dev/mmcblk0 status=progress
```

After restoring to the SD Card expand the root partition to fill the SD Card using gparted.