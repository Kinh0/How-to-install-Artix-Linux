# How to install Artix Linux

#### To check your internet type:
```
ping -c 3 artixlinux.org
```
### and check all partitions using:
```
lsblk
```

### Preferably erase the partitions you have on:
```
cfdisk /dev/sda
```

#### In the label type window, select GPT.

### Use the arrow keys and Enter to create 3 partitions with cfdisk:

    /dev/sda1 # choose 512Mb of space (UEFI)
    /dev/sda2 # choose at least 10 GB of space (root)
    /dev/sda3 # choose all the left space (home)

Write the table to your hard drive and quit.

The first partition is the UEFI partition. It needs to be formatted with a FAT file system:

```
mkfs.fat -F32 /dev/sda1
```
### The other two partitions can be formatted in any Linux file system. I recommend using EXT4:
```
mkfs.ext4 /dev/sda2
mkfs.ext4 /dev/sda3
```
### and, mount the root partition:
```
mount /dev/sda2 /mnt
```

### Create a folder to mount the home partition and mount it:
```
mkdir /mnt/home
mount /dev/sda3 /mnt/home
```

## Install the system

```
basestrap /mnt base base-devel runit elogind-runit
```

## Install the kernel and your text editor

```
basestrap /mnt linux linux-firmware vim
```
### Use fstabgen to generate /etc/fstab, use -U for UUIDs as source identifiers and -L for partition labels:
```
fstabgen -U /mnt >> /mnt/etc/fstab
```
it's time for chroot
```
artix-chroot /mnt /bin/bash
```
Set hostname 
```
echo yourname > /etc/hostname

vim /etc/hosts          #add your name and save.
```
### Install Network
```
pacman -S NetworkManager
```
### And Enable type:
```
NetworkManager
```
### BootLoader

```
 pacman -S grub os-prober efibootmgr
 grub-install --recheck /dev/sda                                             
 grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub
 grub-mkconfig -o /boot/grub/grub.cfg
```
### Set root password
```
passwd
```
## Reboot 
```
exit
umount -R /mnt
reboot
```
# Now be happy.
