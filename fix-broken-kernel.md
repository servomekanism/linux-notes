[//]: # (tags: debian kernel grub boot error)
### debian fix broken kernel/grub

Example assumes `/dev/sda5` is the root partition of the linux installation.
Also, assumes that you have booted with a debian live cd.


`mount -t ext4 /dev/sda5 /mnt/something`

If you have a separate `boot` partition mount that too.

`mount -t ext2 /dev/sda1 /mnt/something/boot`

Now in order to have a functional `chroot`, we need the `proc`, `dev` and `sys`
subsystems to be mounted onto the `chroot`.
```
mount -t proc none /mnt/something/proc
mount -o bind /dev /mnt/something/dev
mount -o bind /sys /mnt/something/sys
```

In the case of the `sys` and `dev` dirs, we need to reference the exact same
mountpoints as the host so we use the `-o bind` option.

Last thing, we want to have functional network name resolution so we copy over
the host's `/etc/resolv.conf` to `/mnt/something/etc/resolv.conf`

Now the `chroot` is ready
```
chroot /mnt/something /bin/bash
source /etc/profile
```

Then do what you want to do like installing the kernel again with `apt` or fix
the `grub` or whatever the problem is
