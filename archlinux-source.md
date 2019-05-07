[//]: # (tags: archlinux source download pacman)

`pacman -S asp`

say you want the source for the package `NetworkManager`

`pacman -Qo NetworkManager` to find out the package who owns it

`asp export networkmanager`

```
cd networkmanager
makepkg -od
```
