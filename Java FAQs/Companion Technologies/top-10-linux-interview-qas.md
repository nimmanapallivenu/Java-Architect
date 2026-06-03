# Top 10 Linux interview Q&As   Java Success.com

## Table of Contents

- [Q1: How do you check for open ports in Linux?](#q1)
- [Q2: How do you find the Linux kernel version?](#q2)
- [Q3: How do you check system’ s current ip address?](#q3)
- [Q4: How do you manage services on Linux?](#q4)
- [Q5: How do you check for free disk space in Linux?](#q5)
- [Q6: How do you check the size of a directory’ s contents in Linux ?](#q6)
- [Q7: How do you check the CPU usage for a process in Linux?](#q7)
- [Q8: What are the dif ferent file types in Unix?](#q8)
- [Q9: What is a mount point in Linux?](#q9)
- [Q10: How will you go about installing Docker in Linux?](#q10)

---

## Q1: How do you check for open ports in Linux?

**Answer:**

With the netstat command. In general you will be using the following:
“-t” for tcp, and “-u” for udp. T o see the name as in “3622/impalad” you need to be an admin. Y ou may have to use “sudo netstat -tulpn”.

---

## Q2: How do you find the Linux kernel version?

**Answer:**

Firstly , Linux is not UNIX as UNIX is a copyrighted name used mostly in a commercial nature by companies like IBM AIX, HP-UX, etc. Linux is a UNIX
clone written from scratch, and it is just a kernel , which manages the cpu, memory , processes, devices like disks, usb, bluetooth, mouse, etc and
distributions like Red Hat Enterprise Linux (i.e. RHEL), Ubuntu Linux, Debian Linux, Fedora Linux, Suse Enterprise Linux, Alpine Linux (e.g. in Docker),
etc make a system complete with GUI, GNU utilities (e.g. cp, mv , ls, date, bash, etc), editors like vi, GNU c/c++ compilers, installation & management tools,
and applications such as Microsoft of fice, Firefox, etc.
You can use uname or hostnamectl .root@quickstart ~]# netstat -tulpn | grep 21050
cp 0 0 0.0.0.0 :21050 0.0.0.0 :* LISTEN 3622 /impalad 
$ hostnamectl

lsb_r elease
lsn stands for Linux Standard Base.root@quickstart ~]# uname -a
Linux quickstart .cloudera 4.9.125 -linuxkit #1 SMP Fri Sep 7 08:20:28 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
root@quickstart ~]# uname -r
4.9.125 -linuxkit
$ lsb_release -r
Release : 7.3
$ lsb_release -cs
Maipo

---

## Q3: How do you check system’ s current ip address?

**Answer:**

Using the “ ifconfig ”

---

## Q4: How do you manage services on Linux?

**Answer:**

“service “, “systemctl “, and “ journalctl ” commands.
SysV init Vs systemd?$cat /etc/redhat -release
Red Hat Enterprise Linux Server release 7.3 (Maipo )
root@quickstart ~]# ifconfig
$ ip addr show
$ ip addr show eth0

How to start and stop services depend on our system uses SysV init or systemd (used by newer distributions). Y ou can tell it with
service allows the smooth transition from /etc/init.d scripts to upstart scripts. service may run scripts from either /etc/init or /etc/init.d (upstart or System V).
systemd
Systemd provides a standard process for controlling what programs run when a Linux system boots up. While systemd is compatible with SysV and Linux
Standard Base (LSB) init scripts, systemd is meant to be a replacement for service.$ ps -p 1
$ sudo service httpd status
$ sudo service httpd stop
$ sudo service httpd start
$ sudo service httpd restart
$ ps -p 1
PID TTY TIME CMD
 1 ? 00:01:03 systemd

---

## Q5: How do you check for free disk space in Linux?

**Answer:**

“df” command. It stands for “ d“isk “ f“ilesystem.
“-a” is for all file systems, and “ -h” is for human readable format. Use “-i” for inode (i.e. index node). An inode is a data structure that stores all information
about a file except its name and its actual data. Each inode stores file metadata like file creation date, owner , permission and disk block location of the object’ s
data.
Linux supports a range of file systems, including ones used on other operating systems such as W indows FAT and NTFS . Those may be supported by
embedded developers but normally a Linux file system like the 4 extended file system ( ext4), XFS , or BTRFS will be used for most storage partitions.$ sudo systemctl status httpd
$ sudo systemctl stop httpd
$ sudo systemctl start httpd
$ sudo systemctl restart httpd
$ sudo systemctl enable httpd
$ sudo systemctl disable httpd
$ sudo systemctl is-enabled httpd
$ sudo systemctl is-active httpd
$ sudo systemctl show httpd
$ sudo systemctl umask httpd
$ df -ah
$ df -ih

Use “ -T” to list the file system type, and “ -t” to list a particular type of
tmpfs is a common name for a temporary file storage facility on many Unix-like operating systems. It is intended to appear as a mounted file system, but stored
in volatile memory instead of a persistent storage device.
The /proc file system is a volatile file system. It does not exist on a disk. Instead, the kernel creates it in memory . It is used to provide information about the
system processes.
cgroup (aka control group), is a Linux kernel feature that allows processes to be or ganised into hierarchical groups whose usage of various types of resources
can then be limited and monitored. The kernel’ s cgroup interface is provided through a pseudo-file system called cgroup.
Docker Engine on Linux relies on cgroups , which allow Docker Engine to share available hardware resources to containers and optionally enforce limits and
constraints. For example, you can limit the memory available to a specific container .

---

## Q6: How do you check the size of a directory’ s contents in Linux ?

**Answer:**

Use the “ du” command. It stands for disk usage.
“-h” is for human readable format, “-s” is for summary . “-a” is for all files & directories.$ df -Th ~
$ df -aht tmpfs

---

## Q7: How do you check the CPU usage for a process in Linux?

**Answer:**

Using the “ ps” command.
How to get CPU and memory information?$ du -sh ~
$ du -ah ~
$ ps auxwww | grep haproxy
$ top
$ htop

“sar” stands for system activity report. “3 5” for every 3 seconds a total of 5 times.$ cat /proc/cpuinfo
$ sar -u 3 5
$ cat /proc/meminfo
$ free -m
$ vmstat

How to find the number of processors?

---

## Q8: What are the dif ferent file types in Unix?

**Answer:**

In a Unix system, everything is treated as a file. Even devices such as printers, USB drives and disk drives.
1. ordinary files
2. directory files
3. device files
Device files are usually found under the /dev directory . Represent a character device file like a mouse as in /dev/input/mice exposes the movement of the
mouse as a character stream$ nproc --all
$ lscpu
$ cat /proc/cpuinfo | awk '/^processor/{print $3}' | wc -l

and block device file represents a hard disk, such as /dev/sda , /dev/sda1 , /dev/sdb , etc.

---

## Q9: What is a mount point in Linux?

**Answer:**

A mount point is a directory to access your data (files and folders) which is stored in your disks.
Devices in Linux – /dev
Linux disks and partition names may be dif ferent from other operating systems. Y ou need to know the names that Linux uses when you create and mount
partitions. Here’ s the basic naming scheme:
The first floppy drive is named /dev/fd0 .
The second floppy drive is named /dev/fd1 .
The first hard disk detected is named /dev/sda .
The second hard disk detected is named /dev/sdb , and so on.
The first SCSI CD-ROM is named /dev/scd0 , also known as /dev/sr0 .
The partitions on each SCSI disk are represented by appending a decimal number to the disk name: sda1 and sda2 represent the first and second partitions of the
first SCSI disk drive in your system.
fdisk
To view all disk partitions in Linux$ ls -ltr /dev/input
# fdisk -l
Device Boot Start End Blocks Id System

/etc/fstab
The /etc/fstab file is a system configuration file on Linux and other Unix-like operating systems that contains information about major filesystems on the
system.
mounting
VFAT is an extension of the F AT file system and was introduced with W indows 95.dev/sda1 * 1 9879 62498816 83 Linux
root@xxx ~]# mkdir /dos
root@xxx ~]# mount /dev/sda3 /dos
# mount ‑t vfat /dev/sda3 /dos
# mount /dev/sdb1 /mnt/media
# mount /dev/sdd1 /media/usb
# umount DIRECT ORY
# umount DEVICE_NAME

---

## Q10: How will you go about installing Docker in Linux?

**Answer:**

In short
Installing docker -ce (i.e. Community Edition) on RHEL
Step 1: Remove older version of docker if any .
Step 2: Install required packages.
Step 3: Configure the docker -ce repo.
To define a new repository , you can either add a [repository] section to the /etc/yum.conf file, or to a .repo file in the /etc/yum.r epos.d/ directory .$ sudo yum install docker
$ sudo yum remove docker docker -common docker -selinux docker -engine -selinux docker -engine docker -ce
$ sudo yum install -y yum-utils device -mapper -persistent -data lvm2

Step 4: Install docker -ce.
Step 5: Enable the docker service.
Step 6: Start the docker service.
Installing docker on Ubuntu
Step 1: Install the gpg (i.e GNU Privacy Guard) key with “ apt-key add “. Download thekey file with “curl” and pass it to “apt-key add”. “-” means pass the
downloaded file as an input to “apt-key”.$ sudo yum-config -manager --add-repo https ://download.docker .com/linux/centos/docker -ce.repo
$ sudo yum install docker -ce
$ sudo systemctl enable docker
$ sudo systemctl start docker

Step 2: Add the Docker repository to APT sources:
Step 3: Update the package database with the Docker packages from the newly added repository .
Step 4: Install docker -ce
Note: Make sure you are about to install from the Docker repository instead of the default Ubuntu 16.04 repository with the “apt-cache policy docker -ce”
command which should say “500 https://download.docker .com/linux/ubuntu xenial/stable amd64 Packages”.
Step 5: Check the status of Docker service.$ curl -fsSL https ://download.docker .com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker .com/linux/ubuntu $(lsb_release -cs) stable"
$ sudo apt-get update
$ sudo apt-get install -y docker -ce

How do you practice your Linux?
1) Install a VM like virtualbox or vmware & Linux distribution on your W indows machine to practice. Getting started with BigData on Cloudera
2) Install the Docker engine on your W indows or Mac O/S. CI/CD – Docker T utorials | Cloudera Quickstart on Docker$ sudo systemctl status docker

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
