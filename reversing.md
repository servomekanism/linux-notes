[//]: # (tags: xxd hexdump reverse reversing binwalk magicbytes)

### To hexdump the vmlinuz byte by byte to grep it (`1f 8b 08 00` are the magic bytes for gz):

`xxd -g1 /boot/vmlinuz-3.6.2-4.fc17.x86_64 | grep "1f 8b 08 00"`

### known headers
```
gz: 1F 8B (08 00)

7z: 37 7A BC AF 27 1C

bz2: 42 5A 68

zip: 50 4B 03 04

rar > version 1.50: 52 61 72 21 1A 07 00

rar > version 5.0: 52 61 72 21 1A 07 01 00
```

### To dd from specific point into the file:

`dd bs=1 skip=17613 if=/boot/vmlinuz-3.6.2-4 of=/tmp/vmlinux.gz`

### Or you can do binwalk all at once:

`binwalk -e vmlinuz-3.6.2-4`

