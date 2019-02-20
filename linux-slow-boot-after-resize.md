### Linux fix slow boot due to resizing of disks

Use ```blkid``` to determine the UUID of your swap partition, and while at it,
make sure all other partitions have correct UUID's in ```/etc/fstab``` . Also
can use ```lsblk -f``` to find the UUIDs.  

Put the correct UUIDs into ```/etc/fstab```, especially swap, for this error.  

Put the correct UUID for swap into ```/etc/initramfs-tools/conf.d/resume```.  

```sudo update-initramfs -u```

