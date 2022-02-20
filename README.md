# How to install Artix Linux

#### To check your internet type:
```sh
artix:[root]:~# ping -c 1 artixlinux.org
```
If the ping runs correctly, your internet is working, case not, use the ``wifi-menu`` to make it work.
<br>
<hr>

### and check all partitions using:
```sh
artix:[root]:~# lsblk
```

### Preferably erase the partitions you have on:
```sh
artix:[root]:~# cfdisk /dev/sda    #save/create/delete partitions
```

<font color=grey><h5>In the label type window, select GPT.</h5></font>
<br>

### Use the arrow keys and Enter to create 3 partitions with cfdisk:

    /dev/sda1 # choose 512Mb of space (UEFI)
    /dev/sda2 # choose at least 10 GB of space (root)
    /dev/sda3 # choose all the left space (home)

Write the table to your hard drive and quit.

The first partition is the UEFI partition. It needs to be formatted with a FAT file system:

```sh
artix:[root]:~# mkfs.fat -F32 /dev/sda1
```
### The other two partitions can be formatted in any Linux file system. I recommend using EXT4:
```sh
artix:[root]:~# mkfs.ext4 /dev/sda2
artix:[root]:~# mkfs.ext4 /dev/sda3
```
### and, mount the root partition:
```sh
artix:[root]:~# mount /dev/sda2 /mnt
```

### Create a folder to mount the home partition and mount it:
```sh
artix:[root]:~# mkdir /mnt/home
artix:[root]:~# mount /dev/sda3 /mnt/home
```

<hr>

## Install the system

```sh
artix:[root]:~# basestrap /mnt base base-devel runit elogind-runit
```

## Install the kernel and your text editor

```sh
artix:[root]:~# basestrap /mnt linux linux-firmware vim
```
### Use fstabgen to generate /etc/fstab, use -U for UUIDs as source identifiers and -L for partition labels:
```sh
artix:[root]:~# fstabgen -U /mnt >> /mnt/etc/fstab
```
<hr>

### it's time for chroot
```sh
artix:[root]:~# artix-chroot /mnt /bin/bash
```
<hr>

Set hostname 
```sh
artix:[root]:~# echo yourname > /etc/hostname

artix:[root]:~# vim /etc/hosts          #add your name and save.
```
### Install Network
```sh
artix:[root]:~# pacman -S NetworkManager
```
### And Enable type:
```sh
artix:[root]:~# NetworkManager
```
<hr>

### BootLoader

```sh
 artix:[root]:~# pacman -S grub os-prober efibootmgr
 artix:[root]:~# grub-install --recheck /dev/sda                                             
 artix:[root]:~# grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub
 artix:[root]:~# grub-mkconfig -o /boot/grub/grub.cfg
```
### Set root password
```sh
artix:[root]:~# passwd
```
## Reboot 
```sh
artix:[root]:~# exit
artix:[root]:~# umount -R /mnt
artix:[root]:~# reboot
```
<hr>

# Install a WM.
<center><font color=red><h1>In progress...</h1><font></center>