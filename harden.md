[//]: # (tags: hidepid hide process proc ps aux)

### For `ps` to only show processes that belong to the user running the command:

`mount -o remount,rw,hidepid=2 /proc`

### To make it permanent, add this line to `/etc/fstab`:

`proc /proc proc defaults,hidepid=2 0 0`
