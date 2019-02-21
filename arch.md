[//]: # (tags: archlinux pacman source download aplay microphone arecord record backup sendEmail restore bluetooth bluez cryptsetup luks encryption 7z zip)

# Arch Linux

## governor
it is recommended to install `cpupower`

### list available governors
`cpupower frequency-info`

### change to performance
`cpupower frequency-set -g performance`

or

`echo governor > /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor` (repeat for every core)

### default to performance
edit `/etc/default/cpupower`

`systemctl enable cpupower`

## pacman
### To download source packages (example with `coreutils`):
1. `pacman -S asp`

2. `asp export coreutils`

3. `makepkg -od`

<<<<<<< HEAD
### to disable and remove all multilib:
1. `pacman -R $(comm -12 <(pacman -Qq | sort) <(pacman -Slq multilib | sort))`

2. `pacman -S gcc-libs base-devel`

3. comment out the `[multilib]` entry in `/etc/pacman.conf` 

=======
>>>>>>> cd7c1ae25deb6389f6d87cea77869e8a63e63f47
### To check if you have microphone
`arecord -d 5 kati.wav`

`aplay kati.wav`

or

`arecord -vv -f dat /dev/null`

notice the activity meter when you talk

## backup
### to create 7z archive from notes
`7z a -mx9 -mmt9 notes-$(date +"%d-%m-%Y").7z *` (mx9 is level of encryption maximum and mmt9 is number of threads)

### send email from command line
`sendEmail -o tls=yes -f frommail@gmail.com -t tomail@gmail.com -s smtp.gmail.com:587 -xu tomail@gmail.com -u "hello from sendemail" -m "how are you?" -a file-`date +"%d-%m-%Y"`.zip` (care here, there are backticks that generate the date automatically)

### create an image file with dd (1GB)
`dd if=/dev/zero of=oneg.bin bs=1 count=0 seek=1G`

### create a 5 mb file
`dd if=/dev/zero of=wtf.dat bs=1M count=5`

### create a 500 mb file
`dd if=/dev/zero of=bigfile bs=500M count=1`


## bluetooth
### to connect to a device
`[root@tux ~]# systemctl start bluetooth.service`

`[root@tux ~]# bluetoothctl`

`[bluetooth]# power on`

`[bluetooth]# agent on`

`[bluetooth]# scan on`

`[bluetooth]# devices`

`[bluetooth]# trust 5C:F8:21:F8:47:D7`

`[bluetooth]# pair 5C:F8:21:F8:47:D7`

## networking
### ip commands (without net-tools package)
`ip link set eth0 up`

`ip addr add 192.168.1.100/24 broadcast 192.168.1.255 dev eth0`

`ip route add default via 192.168.1.254`

### reset all configuration for an interface
`ip addr flush dev eth0`

### Sniff HTTP traffic for a specific host
`tcpdump -i tun0 -A -s 0 'src www.yourhost.gr and tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2))!= 0)'`

### virtual interfaces for vlan hopping
### remove vlan interface created from frogger
`vconfig rem eth0.100`  (100 is the VLAN ID)

### create vlan interface
`vconfig add eth0 100`

# var
## TMUX
`Ctrl+b "` - split pane horizontally

`Ctrl+b %` - split pane vertically

`Ctrl+b arrow key` - switch pane

Hold `Ctrl+b`, don't release it and hold one of the arrow keys - resize pane

### External screen (projector) with xrandr
`xrandr --output DP1 --mode 832x624 --rate 75 --right-of LVDS1`

## systemctl
`systemctl list-unit-files`

### mask a unit so that it's impossible to start it
`systemctl mask unit`

### unmask
`systemctl unmask unit` 

### help
`systemctl help unit`

### system d services that failed to start
`systemct --state=failed`

### journald
`journalctl -b`

`journalctl -b -1`

`journalctl -b -2`

`journalctl --since="2012-10-1"`

`journalctl --since "20 min ago"`

`journalctl /usr/bin/something` 

`journalctl _PID=1`

`journalctl -b -u sshd`

`journalctl -u netcfg`

`journalctl -k` (kernel ring logs)

`journalctl -f -l SYSLOG_FACILITY=10` (auth logs)


### generate a random password of 30 chars
`tr -cd '[:graph:]' < /dev/urandom | head -c 30;echo`

### gcc stuff
`gcc file.c -o file -save-temps` (shows .i and .s and .o files)

`readelf -S file`

### maybe gtk boost:
`mkdir ~/.compose-cache`

### VIM disable all identation
`set paste`

`set nopaste` (to re-enable the configured identation)

## Media
### to record directly from mic to mp3
`ffmpeg -f alsa -i hw:0 -acodec libmp3lame -ab 192k -ac 1 -ar 44100 -vn meeting.mp3`

### other
`arecord -f dat | ssh -C user@host aplay -f dat`    #speak on mic and listen on ssh server!!

`mplayer tv:// -vf screenshot`    #take snapshot from camera and use 's' to store it

### to convert all m4as to mp3 192k
`for i in *.m4a;do ffmpeg -i "$i" -acodec mp3 -ac 2 -ab 192k "${i%.m4a}.mp3"; done`

### download m3u8 videos
`ffmpeg -i "http://ep.ert.gr:1935/vodedge/_definst_/mp4:dvrorigingr/2016/ksenes-seires/ERT3/karamazof/20170113-karamazof-5.mp4/playlist.m3u8" -c:v copy -c:a libvo_aacenc karamazov-episode5.mkv`

## Hashing stuff 
### find all files and calculate their hashes
`find . -type f -name '*' -exec sha512sum '{}' + > hashes.txt` 

### delete all hidden files
`find . -iname '.*' -type f -delete`

## regexps
if FILE is `some-file.jpg` then:

`FILE=some-file.jpg`

`${FILE%.*}=some-file`

`${FILE##*.}=jpg`

### convert DOS to Unix files
`tr -d '\015' <DOS-file >UNIX-file`

### cryptsetup setup LUKS etc
`dd if=/dev/zero of=/dev/sdX iflag=nocache oflag=direct bs=4096`

`fdisk /dev/sdX`

`cryptsetup -v -y -c aes-xts-plain64 -s 512 -h sha512 -i 5000 --use-random luksFormat /dev/sdX1`

`cryptsetup luksHeaderBackup --header-backup-file /path/to/file.img /dev/sdX1`

`cryptsetup luksOpen /dev/sdX1 external01`

`mkfs.ext4 /dev/mapper/external01`

`mount /dev/mapper/external01 /mnt/external`

`umount /mnt/external`

`cryptsetup luksClose /dev/mapper/external01`

### cryptestup quick mounting
`sudo cryptsetup luksOpen /dev/sdX1 external01`

`sudo mount /dev/mapper/external01 /mnt/external`

`DO YOUR WORK HERE`

`sudo umount /mnt/external`

`sudo cryptsetup luksClose /dev/mapper/external01`

### add/remove cryptestup keys luks password
`cryptsetup luksDump /dev/<device> |grep BLED`

`cryptsetup luksAddKey /dev/<device>`

`cryptsetup luksChangeKey /dev/<device> -S 6`

`cryptsetup luksRemoveKey /dev/<device>`

`cryptsetup luksKillSlot /dev/<device> 6`

### rsync backup
`rsync --exclude=*.vdi -av -h --progress SOURCE DESTINATION (without tr /)`

`rsync --exclude=*.vdi -av -h --progress /home/user/music /home/user/pentest /home/user/pictures /home/user/programming/mnt/external`

