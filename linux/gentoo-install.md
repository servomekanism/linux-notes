gentoo installation guide -with full disk encryption- on macbook pro 9,2 (Mid 2012)
===================================================================================
1. Download the ```current-install-amd64-minimal``` from https://www.gentoo.org/downloads/mirrors/
(Make sure the mirror that you select to download it from also has the same bz2 stage 3 installer)
2. ```dd if=install-amd64-minimal-20160707.iso of=/dev/sdd bs=8192k && sync```
3. connect to the internet after booting your usb. For WPA2 network you can use
`iwconfig` or `wpa_supplicant`
4. Partitioning
a. setup `/dev/sda1` that will host the `/boot` partition as GPT
`parted /dev/sda`
`mklabel gpt`
`mkpart ESP fat32 1049kB 538MB`
`set 1 boot on`
`quit`
`mkfs.vfat -F32 /dev/sda1`

b. setup encryption `lvm` at `/dev/sda2`
```
fdisk /dev/sda

sda2[LVM] => Type Linux LVM

modprobe dm-crypt

cryptsetup --cipher aes-xts-plain64 --key-size 512 --hash sha512 --iter-time 5000 --use-random --verify-passphrase luksFormat /dev/sda2

cryptsetup luksOpen /dev/sda2 lvm

pvcreate /dev/mapper/lvm

vgcreate gentoovg /dev/mapper/lvm

lvcreate -L 12GB -n swap gentoovg

lvcreate -l 100%FREE -n root gentoovg

mkswap /dev/mapper/gentoovg-swap

swapon /dev/mapper/gentoovg-swap

mkfs.ext4 /dev/mapper/gentoovg-root
```

5. Installation
```
mount /dev/mapper/gentoovg-root /mnt/gentoo

mkdir /mnt/gentoo/boot

mount /dev/sda1 /mnt/gentoo/boot

date MMDDhhmmYYYY

cd /mnt/gentoo

links https://www.gentoo.org/downloads/mirrors/ and go to releases/amd64/autobuilds

tar xvjpf stage3-*.tar.bz2 --xattrs

nano -w /mnt/gentoo/etc/portage/make.conf
```
(REMEMBER TO ATTACH IT)

be sure to add: `cryptsetup branding -bindst systemd -consolekit truetype`

```

mirrorselect -i -o >> /mnt/gentoo/etc/portage/make.conf

mkdir /mnt/gentoo/etc/portage/repos.conf

cp -L /etc/resolv.conf /mnt/gentoo/etc/

mount -t proc proc /mnt/gentoo/proc

mount --rbind /sys /mnt/gentoo/sys

mount --make-rslave /mnt/gentoo/sys

mount --rbind /dev /mnt/gentoo/dev

mount --make-rslave /mnt/gentoo/dev

chroot /mnt/gentoo /bin/bash

source /etc/profile

export PS1="(chroot) $PS1"

emerge-webrsync

eselect news read

eselect profile list

eselect profile set 1
```
(use any make.conf backup here)

```
emerge --ask --update --deep --newuse @world --quiet

emerge --deselect sys-fs/udev

echo "Continent/City" > /etc/timezone

emerge --config timezone-data

nano -w /etc/locale.gen

locale-gen

eselect locale list

eselect locale set 8 
```
(use the english utf-8 one)
```
env-update && source /etc/profile && export PS1="(chroot) $PS1"

emerge -avq sys-kernel/gentoo-sources sys-apps/pciutils networkmanager sys-apps/usbutils

cd /usr/src/linux

make menuconfig 
```
(if you want default settings, for now)

replace `.config` with the backup-config we have
```

make -j5 && make -j5 modules_install && make install

emerge -avq genkernel-next 
```
(`cryptsetup` too, if not in `make.conf`)

```

genkernel --luks --lvm initramfs

emerge -avq wpa_supplicant linux-firmware iw

echo GRUB_PLATFORMS="efi-64" >> /etc/portage/make.conf

emerge --ask sys-boot/grub:2

emerge -avq dosfstools efibootmgr

emerge -avq cronie e2fsprogs dhcpcd

emerge app-portage/cpuinfo2cpuflags -1

grub2-install --target=x86_64-efi --efi-directory=/boot

blkid

nano -w /etc/default/grub
```
add the following:
`GRUB_CMDLINE_LINUX="init=/usr/lib/systemd/systemd
crypt_root=UUID=a0328490-a037-43c9-b253-57f5ead9853c dolvm quiet
systemd.show_status=1 acpi_backlight=none net.ifnames=0"`

(note: we use the `UUID` of `/dev/sda2` - the encrypted root partition)
```

grub2-mkconfig -o /boot/grub/grub.cfg

useradd -m -G users,wheel,audio,cdrom,portage,usb -s /bin/bash user

passwd

passwd user

exit

cd

umount -l /mnt/gentoo/dev{/shm,/pts,}

umount /mnt/gentoo{/boot,/sys,/proc,}

reboot
```
6. Systemd setup

```

ln -sf /proc/self/mounts /etc/mtab

hostnamectl set-hostname myhost

hostnamectl status

(optional) 127.0.0.1 tux.local myhost localhost at /etc/hosts

localectl list-locales

localectl set-locale LANG=en_US.UTF-8

localectl status

localectl list-keymaps

localectl set-keymap us

localectl set-x11-keymap en_US

timedatectl set-timezone Continent/City

timedatectl status

emerge media-fonts/terminus-font

echo "sys-boot/grub:2 truetype" >> /etc/portage/package.use/grub

emerge sys-boot/grub:2

setfont ter-v16b

nano /etc/vconsole.conf

FONT=ter-v16b
```
