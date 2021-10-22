---
title:  "Extend EC2 EBS Volume size without downtime"
date:   2021-06-30 12:33:22 +0530
categories:
  - AWS
  - EC2
  - EBS
toc: true
toc_label: "Contents"
toc_icon: "file-alt"
toc_sticky : true
tags:
  - AWS
  - EC2
  - EBS
author: Akhilesh Moghe
show_author_profile: true
---


## Introduction
It's a very common scenario when you run out of space on an running EC2 instance. And you need to increase the volume of that running instance without shutting it down. This post is all about the same scenario.

## Modify EBS Volume:
* Login to your AWS console
* Choose “EC2” from the services list
* Click on “Volumes” under ELASTIC BLOCK STORE menu (on the left)
* Choose the volume that you want to resize, right-click on “Modify Volume”
* You’ll see an option window like this one:
* Set the new size for your EBS volume (in this case i extended an 8GB volume to 20GB)
* Click on modify.

![Modify EBS Volume](/assets/images/Extend_EC2_EBS_Volume_size.png)

* SSH to the EC2 instance where the EBS we’ve just extended is attached to.
* Type the following command to list our block devices:
  ```
  ubuntu@ip-172-31-6-183:~$ lsblk
  NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
  loop0     7:0    0 32.3M  1 loop /snap/snapd/11588
  loop2     7:2    0 55.5M  1 loop /snap/core18/1997 
  loop3     7:3    0 33.3M  1 loop /snap/amazon-ssm-agent/3552 
  loop4     7:4    0 32.3M  1 loop /snap/snapd/12398 
  loop5     7:5    0 55.5M  1 loop /snap/core18/2074 
  xvda    202:0    0  150G  0 disk  
  └─xvda1 202:1    0  150G  0 part / 
  ubuntu@ip-172-31-6-183:~$ 

  ubuntu@ip-172-31-6-183:~$ lsblk 
  NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT 
  loop0     7:0    0 32.3M  1 loop /snap/snapd/11588 
  loop2     7:2    0 55.5M  1 loop /snap/core18/1997 
  loop3     7:3    0 33.3M  1 loop /snap/amazon-ssm-agent/3552 
  loop4     7:4    0 32.3M  1 loop /snap/snapd/12398 
  loop5     7:5    0 55.5M  1 loop /snap/core18/2074 
  xvda    202:0    0  200G  0 disk  
  └─xvda1 202:1    0  150G  0 part / 
  ubuntu@ip-172-31-6-183:~$
  ```
* As you can see size of the root volume reflects the new size, 200GB, the size of the partition reflects the original size, 150 GB, and must be extended before you can extend the file system.
* To do so, type the following command: 
  ```
  Be careful, there is a space between device name and partition number! 

  ubuntu@ip-172-31-6-183:~$ sudo growpart /dev/xvda 1 
  CHANGED: partition=1 start=2048 old: size=314570719 end=314572767 new: size=419428319,end=419430367 
  ubuntu@ip-172-31-6-183:~$ 

  ubuntu@ip-172-31-6-183:~$ lsblk 
  NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT 
  loop0     7:0    0 32.3M  1 loop /snap/snapd/11588 
  loop2     7:2    0 55.5M  1 loop /snap/core18/1997 
  loop3     7:3    0 33.3M  1 loop /snap/amazon-ssm-agent/3552 
  loop4     7:4    0 32.3M  1 loop /snap/snapd/12398 
  loop5     7:5    0 55.5M  1 loop /snap/core18/2074 
  xvda    202:0    0  200G  0 disk  
  └─xvda1 202:1    0  200G  0 part / 
  ubuntu@ip-172-31-6-183:~$
  ```
* Extend the filesystem itself.
* Confirm File System type with:
  ```
  ubuntu@ip-172-31-6-183:~$ df -Th 
  Filesystem     Type            Size    Used    Avail    Use%    Mounted on 
  udev               devtmpfs   16G     0          16G      0%        /dev 
  tmpfs              tmpfs         3.2G    824K   3.2G     1%        /run 
  /dev/xvda1     ext4      146G  128G   18G  88% / 
  tmpfs          tmpfs      16G   88K   16G   1% /dev/shm 
  tmpfs          tmpfs     5.0M     0  5.0M   0% /run/lock 
  tmpfs          tmpfs      16G     0   16G   0% /sys/fs/cgroup 
  /dev/loop0     squashfs   33M   33M     0 100% /snap/snapd/11588 
  /dev/loop2     squashfs   56M   56M     0 100% /snap/core18/1997 
  /dev/loop3     squashfs   34M   34M     0 100% /snap/amazon-ssm-agent/3552 
  /dev/loop5     squashfs   56M   56M     0 100% /snap/core18/2074 
  /dev/loop4     squashfs   33M   33M     0 100% /snap/snapd/12398 
  tmpfs          tmpfs     3.2G     0  3.2G   0% /run/user/1000 
  ubuntu@ip-172-31-6-183:~$
  ```
* If your filesystem is an ext2, ext3, or ext4, type:
  ```
  ubuntu@ip-172-31-6-183:~$ sudo resize2fs /dev/xvda1  
  resize2fs 1.44.1 (24-Mar-2018) 
  Filesystem at /dev/xvda1 is mounted on /; on-line resizing required 
  old_desc_blocks = 19, new_desc_blocks = 25 
  The filesystem on /dev/xvda1 is now 52428539 (4k) blocks long. 

  ubuntu@ip-172-31-6-183:~$
  ```
* If your filesystem is an XFS, then type:
  ```
  [ec2-user ~]$ sudo xfs_growfs /dev/xvda1
  ```
* Finally we can check our extended filesystem by typing:
  ```
  ubuntu@ip-172-31-6-183:~$ df -Th 
  Filesystem     Type      Size  Used Avail Use% Mounted on 
  udev           devtmpfs   16G     0   16G   0% /dev 
  tmpfs          tmpfs     3.2G  824K  3.2G   1% /run 
  /dev/xvda1     ext4      194G  128G   67G  66% / 
  tmpfs          tmpfs      16G   88K   16G   1% /dev/shm 
  tmpfs          tmpfs     5.0M     0  5.0M   0% /run/lock 
  tmpfs          tmpfs      16G     0   16G   0% /sys/fs/cgroup 
  /dev/loop0     squashfs   33M   33M     0 100% /snap/snapd/11588 
  /dev/loop2     squashfs   56M   56M     0 100% /snap/core18/1997 
  /dev/loop3     squashfs   34M   34M     0 100% /snap/amazon-ssm-agent/3552 
  /dev/loop5     squashfs   56M   56M     0 100% /snap/core18/2074 
  /dev/loop4     squashfs   33M   33M     0 100% /snap/snapd/12398 
  tmpfs          tmpfs     3.2G     0  3.2G   0% /run/user/1000 
  ubuntu@ip-172-31-6-183:~$
  ```
## References:
* [Tutorial: How to Extend AWS EBS Volumes with No Downtime][Tutorial: How to Extend AWS EBS Volumes with No Downtime]
* [Extend a Linux file system after resizing a volume][Extend a Linux file system after resizing a volume]


[Tutorial: How to Extend AWS EBS Volumes with No Downtime]: https://medium.com/geekculture/tutorial-how-to-extend-aws-ebs-volumes-with-no-downtime-ec7d9e82426e
[Extend a Linux file system after resizing a volume]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/recognize-expanded-volume-linux.html?icmpid=docs_ec2_console

