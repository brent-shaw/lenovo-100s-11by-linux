# Getting Linux onto the Lenovo 100s

Guides for installation linux on Lenovo 100s 11by

## Prepare for installation (draft)

**WARNING!!!** Please check the parameters according to your configuration

### What are needed?

The following two ISOs will be needed.

- debian-live-9.1.0-i386-xfce.iso 
- linuxmint-18.2-xfce-32bit.iso

### Prepare USB stick

```sh
$ sudo dd if=linuxmint-18.2-xfce-32bit.iso of=/dev/sdX bs=4M
$ sync
```
Unplug and plug again USB stick

```
$ sudo fdisk /dev/sda
```
Make new partition with EFI
```
Command (m for help): p
Disk /dev/sda: 14.7 GiB, 15728640000 bytes, 30720000 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x49b13675

Device     Boot Start     End Sectors  Size Id Type
/dev/sda1  *        0 2566143 2566144  1.2G 17 Hidden HPFS/NTFS
```
You can use the "n" command in fdisk to create a partition.

```

Command (m for help): p
Disk /dev/sda: 14.7 GiB, 15728640000 bytes, 30720000 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x49b13675

Device     Boot   Start      End  Sectors  Size Id Type
/dev/sda1  *          0  2566143  2566144  1.2G 17 Hidden HPFS/NTFS
/dev/sda2       2566144 30719999 28153856 13.4G ef EFI (FAT-12/16/32)
```

```
$ sudo mkfs.fat -F32 /dev/sda2
```

The structure of /dev/sda2
- /efi/boot/bootia32.efi (from Debian 32 bit installation)
- /efi/boot/i386-efi/* (from Debian 32 bit installation)

- copy all directories and files from the Mint ISO (other than the boot directory) to your sdX2 partition


### Boot from USB stick

```
grub> ser prefix=(hd0,2)/efi/boot
grub> insmod linux
grub> insmod all_video
grub> linux /casper/vmlinuz file=/cdrom/preseed/linuxmint.seed boot=casper 
grub> initrd /casper/initrd.lz
grub> boot
```

## Installation Linux Mint

TODO need to install `apt install grub-efi-ia32`

# Compatibility and support

The kernel that ships with Mint 18.2 (4.8) has poor support for the Lenovo 100s hardware. It is best to upgrade to the latest state kernel or another working kernel on the list below.

| Kenrel  | Keyboard | Mouse | WiFi | Battery | Webcam | Audio |
| ------- | -------- | ----- | ---- | ------- | ------ | ----- |
| 4.8     | N        | N     | N    | N       | N      | N     |
| 4.10    | Y        | Y     | N    | N       | N      | N     |
| 4.13.9  | Y        | Y     | Y    | Y       | N      | N     |
| 4.13.10 | Y        | Y     | Y    | Y       | Y      | N     |
| 4.13.11 | Y        | Y     | Y    | Y       | Y      | N     |

A program like Ukuu (Ubuntu Kernel Update Utility) makes this very simple and easy to do.

```
sudo apt-add-repository -y ppa:teejee2008/ppa
sudo apt-get update
sudo apt-get install ukuu
```

