---
title:  "Frequently used commands while creating a Linux Disk Image"
permalink: /_post/linux/disk_img_creation_process
date:   2022-06-03 1:33:22 +0530
categories:
  - Linux
  - Embedded Linux
  - Systems Engineering
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - Linux
  - Embedded Linux
  - Systems Engineering
author: Akhilesh Moghe
show_author_profile: true
---

In this article, we will learn the Linux Disk image creation, partioning the disk image, formatting the partitions, mounting it in order to view it's contents.
Also, we will try to understand the various commands used in this process like `dd`, `fdisk`, `mkfs`, etc...

## dd:
- `dd` is basically a command-line utility to copy a file, also convert and format as per the options specified.
- Now, Linux/Unix has everything as a file, weather it's a Hardware drive like `/dev/sda`, USB drives like `/dev/sdb` or special devices like `/dev/null` or `/dev/zero` or `/dev/random`, all are files only, so `dd` command can be used for all above devices, to read from, write to i.e. copy to/from and also format them.
- `dd` can be used for tasks such as *<u>backing up the </u>*__*<u>boot sector</u>*__ of a hard drive, and *<u>obtaining a fixed amount of random data</u>*.
- `dd` can also perform *<u>conversions on the data</u>* as it is copied, including *<u>byte order swapping</u>* and *<u>conversion to and from the ASCII and EBCDIC text encodings</u>* (option: conv=ascii/ebcdic).
- By default, `dd` reads from `stdin` and writes to `stdout`, but these can be changed by using the `if` (input file) and `of` (output file) options.
- On completion, `dd` prints to the `stderr` stream about statistics of the data transfer. Each of the *<u>Records in</u>* and *<u>Records out</u>* lines shows the number of *<u>complete blocks transferred + the number of partial blocks</u>*, e.g. because the physical medium ended before a complete block was read, or a physical error prevented reading the complete block.
- __*<u>Block Size (bs)</u>*__:
  - A *<u>block</u>* is a unit measuring the number of bytes that are read, written, or converted at one time.
  - Command-line options can specify a different block size for *<u>input/reading</u>* (__*<u>ibs</u>*__) compared to *<u>output/writing</u>* (__*<u>obs</u>*__), though the *<u>block size</u>* (__*<u>bs</u>*__) option will override both ibs and obs.
  - The __default__ value for both input and output block sizes is __512 bytes__ (the traditional block size of disks, and POSIX-mandated size of "a block").
  - Block size has an effect on the performance of copying `dd` commands.
    - Doing many small reads or writes is often slower than doing fewer large ones.
    - Using large blocks requires more RAM and can complicate error recovery, also larger block sizes usually don't really speed it up much more.
    - Basically you should use *<u>4M</u>* or a multiple of that as block size. Because, the organisation of SD card is such that , it erases 4M blocks in group, so partitions should be aligned on Erase Block Group, i.e. 4M boundary for efficiency.
- __*<u>Count</u>*__:
  - The count option for copying is measured in blocks, as are both the __*<u>skip count</u>*__ for reading and __*<u>seek count</u>*__ for writing.
- __*<u>Status</u>*__:
  - `dd` support the `status=progress` option, which enables periodic printing of transfer statistics to stderr.

- Examples:
{% highlight shell linenos %}
$ blocks=$(isosize -d 2048 /dev/sr0)
$ dd if=/dev/sr0 of=isoimage.iso bs=2048 count=$blocks status=progress
--> Creates an ISO disk image from a CD-ROM, DVD or Blu-ray disc.

$ dd if=system.img of=/dev/sdc bs=64M conv=noerror
--> Restores a hard disk drive (or an SD card, for example) from a previously created image.

$ dd if=/dev/sdb2 of=partition.image bs=64M conv=noerror
--> Create an image of the partition sdb2, using a 64 MiB block size.

$ dd if=/dev/sda2 of=/dev/sdb2 bs=64M conv=noerror
--> Clones one partition to another.

$ dd if=/dev/sd0 of=/dev/sd1 bs=64M conv=noerror
--> Clones a hard disk drive "sd0" to "sd1".

----------------------------X----------------------------

### In-place modification
$ dd if=/dev/zero of=path/to/file bs=512 count=1 conv=notrunc
--> dd can modify data in place. For example, this overwrites the first 512 bytes of a file with null bytes:
--> The notrunc conversion option means do not truncate the output file,
--> i.e. if the output file already exists, just replace the specified bytes and leave the rest of the output file alone.
--> Without this option, dd would create an output file 512 bytes long.

----------------------------X----------------------------

### Master boot record backup and restore
$ dd if=/dev/fd0 of=MBRboot.img bs=512 count=2
--> The example above can also be used to back up and restore any region of a device to a file, such as a master boot record.
--> To duplicate the first two sectors of a floppy disk.

----------------------------X----------------------------

### Disk wipe-out
To write zeros to a disk, use:
$ dd if=/dev/zero of=/dev/sda bs=16M.

To write random data to a disk, use: (slower than above, obviously due to random numbers generation)
$ dd if=/dev/urandom of=/dev/sda bs=16M.

----------------------------X----------------------------

### Generating a file with random data
To make a file of 100 random bytes using the kernel random driver:
$ dd if=/dev/urandom of=myrandom bs=100 count=1

----------------------------X----------------------------

### Converting a file to upper case
To convert a file to uppercase:
$ dd if=filename of=filename1 conv=ucase,notrunc

{% endhighlight %}

---

## fdisk:
- `fdisk` also known as *<u>format disk</u>* is used for creating and manipulating *<u>disk partition table</u>*. It is used for the view, create, delete, change, resize, copy and move partitions on a hard drive.
- It understands __GPT__, __MBR__, __Sun__, __SGI__ and __BSD__ partition tables. Block devices can be divided into one or more logical disks called partitions. This division is recorded in the *<u>partition table</u>*, usually found in *<u>sector</u>* 0 of the disk.
- `fdisk` allows you to create a *<u>maximum of four </u>*__*<u>primary</u>*__*<u> partitions</u>* and the *<u>number of logical partition depends on the size of the hard disk</u>*.
- It allows the user to:
  - To Create space for new partitions.
  - Organizing space for new drives.
  - Re-organizing old drives.
  - Copying or Moving data to new disks(partitions).
- Usage:
  - __*View All Disk Partitions*__:
    ```
    $ sudo fdisk -l
    ```
  - __*View Partition on a Specific Disk*__:
    ```
    $ sudo fdisk -l /dev/sda
    ```
  - __*View all fdisk Commands*__:
    - `fdisk` is a dialogue driven command as:
    ```
    $ sudo fdisk /dev/sda
    ```
      - Create a Hard Disk Partition: __*n*__
      - Make Partition __Primary__: __*p*__
      - Delete a Hard Disk Partition: __*d*__
      ![fdisk options](/assets/images/embedded/fdisk-options.png)

  - __*View the size of Partition*__:
    ```
    $ sudo fdisk -s /dev/sda
    ```
  - Further Details: [fdisk(8):man-page](https://man7.org/linux/man-pages/man8/fdisk.8.html)

---

## losetup:
- `losetup` is used to associate loop devices with regular files or block devices, to detach loop devices, and to query the status of a loop device.
- __*<u>Loop Devices</u>*__:
  - A loop device is a pseudo-device that *<u>makes a computer file accessible as a block device</u>*.
  - If the CD ISO images and floppy disk images contains an entire file system, the file may then be *<u>mounted</u>* as if it were a disk device.
  - Mounting a file containing a file system via such a loop mount makes the files within that file system accessible, just like any other directories accessed with `ls`, `cd` commands. They appear in the mount point directory (/mnt).
  - It makes it a convenient method for managing and editing file system images offline, that are later used for normal system operation.
  - It also provides a permanent segregation of data, by means of a data partition.
  - In Linux, these devices are called "loop" devices and device nodes are usually named `/dev/loop0`, `/dev/loop1`, etc.
  - They can be created with `makedev` for the static device directory, dynamically by the facilities of the device file system (`udev`), or directly with `mknod`.
  - The management user interface for the loop device is `losetup`, which is part of the package `util-linux`.
  - :x: A `loopback` device, term is reserved for a networking device in operating systems. The concept of the loop device is distinct.

  - Mounting a file containing a disk image on a directory requires two steps:
    - Association of the file with a loop device node,
    ```
    $ losetup /dev/loop0 example.img
    ```
    - Mounting of the loop device at a mount point directory
    ```
    $ mount /dev/loop0 /home/you/dir
    ```
    - The `mount` utility is usually capable of handling the entire procedure (both above steps):
    ```
    $ mount -o loop example.img /home/you/dir
    ```
    - The device can then be unmounted with the following command:
    ```
    $ umount /home/you/dir
    $ umount /dev/loop<N>
    ```
  - __*`/dev/null` vs `/dev/zero`*__:
    - `/dev/null` produces no output.
    - `/dev/zero` produces a continuous stream of NULL (zero value) bytes.
    - `/dev/zero` is used to create dummy files or swap.
    
{% highlight shell linenos %}
$ dd if=/dev/null of=file count=10
0+0 records in
0+0 records out
0 bytes (0 B) copied, 0.000276193 s, 0.0 kB/s

----------------------------X----------------------------

$ dd if=/dev/zero of=file count=10
10+0 records in
10+0 records out
5120 bytes (5.1 kB) copied, 0.00090775 s, 5.6 MB/s
{% endhighlight %}

- __*Mount multiple partitions from disk image simultaneously*__:
{% highlight shell linenos %}
$ losetup -P -f --show my.img
--> Creates one `/dev/loopXpY` per partition.

$ sudo mount /dev/loop5p1 /mnt/CROS_Boot/
$ sudo mount /dev/loop5p2 /mnt/CROS_imgpart2/
$ sudo mount /dev/loop5p3 /mnt/CROS_imgpart3/
$ sudo mount /dev/loop5p4 /mnt/CROS_imgpart4/
--> After `losetup`, every partition needs to be mounted, before accessing it.
{% endhighlight %}

---

## mkfs
- `mkfs` command stands for __*make file system*__ is utilized to make a file system on a formatted storage device usually, a partition on a hard disk drive (HDD) or USB drive or it can also be a loop device, etc.
- `mkfs` is a front-end for the various specific file system creation programs that are available in Linux, such as `mke2fs`, `mkfs.ext4` and `mkfs.vfat`, etc.
![mkfs-supported-file-types](/assets/images/embedded/mkfs-supported-file-types.jpeg)
- Use `fdisk` command to create the volume partitions and `losetup` command to mount the partition of an disk image on a loop device, if required.
- Formatting the new volume created as a FAT32 FS type:
```
$ sudo mkfs.vfat /dev/sdb1
```
![mkfs-vfat-output](/assets/images/embedded/mkfs-vfat-output.jpeg)
- Verifying the newly created filesystem:
```
$ sudo file -sL /dev/sdb1
```
![mkfs-vfat-verification](/assets/images/embedded/mkfs-vfat-verification.jpeg)

  ---

## mount & umount commands
- All files accessible in a Unix system are arranged in one big tree, the file hierarchy, rooted at `/`. These files can be spread out over several devices.
- The `mount` command serves to attach the filesystem found on some device to the big file tree.
- Conversely, the `umount(8)` command will detach the filesystem again.
- The filesystem is used to control how data is stored on the device or provided in a virtual way by network or other services.
- Some Important Options:
  - l : Lists all the file systems mounted yet.
  - h : Displays options for command.
  - V : Displays the version information.
  - a : Mounts all devices described at /etc/fstab.
  - t : Type of filesystem device uses.
  - T : Describes an alternative fstab file.
  - r : Read-only mode mounted.
- List of `ext4` file systems mounted on a machine:
```
$ mount -l -t ext4
/dev/sda1 on / type ext4 (rw,relatime,errors=remount-ro)
$
```
- Mount file system on devices to a directory on a machine
```
$ sudo mount <source-device-path> <destination-directory-path>
```
- Unmount the file system
```
$ sudo umount <mounted-destination-path>
```

###### References:
- [dd:wikipedia](https://en.wikipedia.org/wiki/Dd_(Unix))
- [dd(1):man-page](https://man7.org/linux/man-pages/man1/dd.1.html)
- [fdisk(8):man-page](https://man7.org/linux/man-pages/man8/fdisk.8.html)
- [losetup(8):man-page](https://man7.org/linux/man-pages/man8/losetup.8.html)
- [Loop Devices: wikipedia](https://en.wikipedia.org/wiki/Loop_device)
- [Difference between `/dev/null` and `/dev/zero`](https://unix.stackexchange.com/q/254384/98539)
- [How to mount multiple partitions from disk image simultaneously?](https://unix.stackexchange.com/q/342463/98539)
- [mkfs](https://www.geeksforgeeks.org/mkfs-command-in-linux-with-examples/)
- [mount](https://man7.org/linux/man-pages/man8/mount.8.html)
