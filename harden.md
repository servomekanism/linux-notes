[//]: # (tags: hidepid hide process proc ps aux)

#### For `ps` to only show processes that belong to the user running the command:

`mount -o remount,rw,hidepid=2 /proc`

#### To make it permanent, add this line to `/etc/fstab`:

`proc /proc proc defaults,hidepid=2 0 0`

#### In case an app breaks cause of this, create a group say `appgroup` and:

`usermod -aG appgroup app`

`mount -o remount,rw,hidepid=2,gid=appgroup /proc`

and to make permanent add this to `/etc/fstab`:

`proc /proc proc defaults,hidepid=2,gid=appgroup 0 0`
