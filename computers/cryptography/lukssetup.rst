Steps to make a Luks Container
==============================

About
-----

Here, I am going to make a 120 MB luks file container.

Prereq
------

Requires cryptsetup.

These commands will need to be run as root.


Create Block Device
-------------------

First, create a raw data file. I'm going to write 4K at a time, until I reach 120MB (4096*30720 = 125829120 = 120 * (1024*1024)).

    dd if=/dev/zero of=/path/to/container bs=4096 count=30720

Connect to Loop Device
----------------------

I now have a raw data file, which is currently full of zeros. We will use this as a device. To do that, we need to have linux treat this as a device. We will use the loopback device for this.

First, setup the file for use as a loopback device.

    losetup /dev/loop0 /path/to/container

If the device is busy, move to next loop file in /dev. You can check what is using a loop device by running:

    losetup /dev/loop0


Partition Device File
----------------------

We now have the file setup as a device. From this point on, we can treat this loop device like a disk drive device. It needs to be partitioned. Using fdisk is beyond scope here.


Setup Encryption
----------------

Now that a partition exists, we can encrypt it.

    cryptsetup --verify-passphrase luksFormat /dev/loop0


Format Encrypted Device
-----------------------

Now that the container is encrypted, we can format the contents for the file system.

First, open the device.

    cryptsetup luksOpen /dev/loop0 container

    mkfs.ext4 -j /dev/mapper/container


Mount Drive
-----------

At this point, we now have an ext4 formated drive that is stored in the encrypted file container. It is a device located at /dev/mapper/container. This is used to mount the device like a physical disk drive.

    mount /dev/mapper/container /mnt/point


Disconnecting Drive
-------------------

To remove the drive, first unmount. Then, close the container.

    umount /mnt/point

    cryptsetup luksClose container

    losetup -d /dev/loop0

