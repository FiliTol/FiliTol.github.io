---
layout: post
title: "Arch Linux installation step-by-step"
categories: Tech Linux
lang: en
---

One of my New Year's resolution is to update the personal website and my
online content as much as possible, thus I decided to write a brief tutorial about
how to create an encrypted Arch Linux installation from scratch.

I wrote it down for myself, to be much quicker every time that I have to restart
my system from scratch. For this reason, I omitted to give profuse explanations
about every step, but I may update the guide as I find errors, different best practices
or even more automated ways to create the full Arch installation.

As a reminder, I warn you that no guide can possibly substitute the role of the
<a href="https://wiki.archlinux.org/" target="_blank">Arch wiki</a>, thus I highly
suggest to pivot to it every time you have doubts or you want to deepen your knowledge
of a specific topic.

## System specifics

This guide has been tested on HP Pavilion Laptop 15, with 11th Gen Intel(R)
Core(TM) i7-119z and 16GB of RAM. Please note that the installation process could
significantly differ for systems with different specifics, particularly for
non-UEFI systems.

The resulting fresh Arch Linux installation will have an encrypted main volume
used for the filesystem. The system boots from a separated boot partition.

## Keyboard layout

```{bash}
loadkeys it
```

## Detecting boot mode

```{bash}
ls /sys/firmware/efi/efivars  # If exists, then boot mode is UEFI
```

## Internet connection

```{bash}
iwctl
device list
station <device> scan
station <device> get-networks
station <device> connect <SSID>
```
  
## Timezone

```{bash}
timedatectl set-timezone Europe/Rome
```

## Disk partitioning

The process considers a LVM partition with the LUKS encryption

### Detect disks

```{bash}
lsblk 
fdisk -l
```

### Create partition table and partitions

```{bash}
gdisk /dev/<DISK_name>
```

### Boot partition

```{bash}
n       # create new partition
Enter   # accept suggestion
Enter   # accept first sector
+512M   # provide last sector
Enter   # save last sector
ef00    # EFI partition type
Enter   # finish boot partition setup
```

### Main encrypted partition

```{bash}
n       # create new partition
Enter   # accept suggestion
Enter   # accept first sector
Enter   # accept last sector (all the remaining space encrypted)
8e00    # LVM partition type
Enter   # finish main partition setup
```

### Save current partition layout

```{bash}
w   # write on disk
```

### Format boot partition

```{bash}
mkfs.fat -F32 /dev/<BOOT-PARTITION_name>
```

## Encryption

```{bash}
modprobe dm-crypt
cryptsetup luksFormat /dev/<MAIN-PARTITION_name>  # Partition to be encrypted
> Here provide encryption password
```

Test encryption by reopening it

```{bash}
cryptsetup open --type luks /dev/<MAIN-PARTITION_name> lvm
```

## Create volumes

This process create partitions of the LVM

```{bash}
pvcreate /dev/mapper/lvm
vgcreate main /dev/mapper/lvm
```

### Create swap volume

The swap volume size is recommended to be RAM+2GB

```{bash}
lvcreate -L18G main -n swap
```

### Create main volume

```{bash}
lvcreate -l 100%FREE main -n root
```

### Format volumes

```{bash}
mkswap /dev/mapper/main-swap
mkfs.ext4 /dev/mapper/main-root
```

## Mount partitions

```{bash}
mount /dev/mapper/main-root /mnt
mkdir /mnt/boot
mount /dev/<BOOT-PARTITION_name> /mnt/boot
swapon /dev/mapper/main-swap
```

## OS installation

```{bash}
pacstrap /mnt base base-devel linux linux-firmware lvm2 man-db man-pages texinfo vim neovim iwd
```

## Setup

```{bash}
genfstab -U /mnt >> /mnt/etc/fstab 
```

```{bash}
arch-chroot /mnt
```

From now on the commands are executed in the actual new system

### Timezone setup

```{bash}
ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime
```

### Localization

- Edit file with `/etc/locale.gen` end uncomment desired locale (`en_GB.UTF-8`
or `en_US.UTF-8` recommended)
- Generate locale file with `locale-gen`
- Create `/etc/locale.conf` and insert the desired `LANG` value (es. `LANG=en_GB.UTF-8`)
- Create `/etc/vconsole.conf` and enter default keyboard layout (es. `KEYMAP=it`)

### Network

In `/etc/hostname` write the desired hostname. Then edit `/etc/hosts` as follows
(based on the hostname):

```{bash}
# Static table lookup for hostnames.
# See hosts(5) for details.
127.0.0.1  localhost HOSTNAME
::1        localhost HOSTNAME
127.0.1.1  HOSTNAME.localdomain HOSTNAME
```

### Boot order based on encryption

Edit `/etc/mkinitcpio.conf` and modify the order of parameters for `HOOKS` variable
in order to make the keyboard connect before the filesystem and assure that the
keyboard is unlocked before the loading of the decryption form.

For example:

```{bash}
HOOKS=(base udev autodetect modconf block keyboard encrypt lvm2 keymap consolefont
block filesystems fsck)
```

### Bootloader

Create the **initramfs**, which is an archive of the initial file system that gets
loaded into memory during the Linux startup process.

```{bash}
mkinitcpio -P
```

Install then `systemd-boot` bootloader with:

```{bash}
bootctl --path=/boot/ install
```

and then select the default arch profile in `/boot/loader/loader.conf`

```{bash}
default arch
editor 0
```

Create the profile by editing `/boot/loader/entries/arch.conf` as follows:

```{bash}
title Arch Linux
linux /vmlinuz-linux
initrd  /initramfs-linux.img
options cryptdevice=/dev/<MAIN-PARTITION_name>:main root=/dev/mapper/main-root
resume=/dev/mapper/main-swap lang=it locale=en_GB.UTF-8
```

### Define root

Create root password

```{bash}
passwd
```

## Finally reboot into Arch installation

Remember to install `iwd` before booting up again the system, because it is needed
for the network setup interface
Exit the chroot environment with `exit`, then **unmount** with `umount -R /mnt`.

Reboot the system with `reboot` and remove the usb drive used for the installation.

<div class="post-categories">
  {% if post %}
    {% assign categories = post.categories %}
  {% else %}
    {% assign categories = page.categories %}
  {% endif %}
  {% for category in categories %}
  <a href="{{site.baseurl}}/categories/#{{category|slugize}}">{{category}}</a>
  {% unless forloop.last %}&nbsp;{% endunless %}
  {% endfor %}
</div>
