[//]: # (tags: debian deb package unpack reinstall default config repack repackage)
### reinstall package and its default config files at debian

`apt-get -o Dpkg::Options::="--force-confmiss" install --reinstall PKGNAME`

### unpack modify and repackage deb file

1. Extract deb package
    `dpkg-deb -x <package.deb> <dir>`
2. Extract control-information from a package
    `dpkg-deb -e <package.deb> <dir/DEBIAN>`
3. After completed to make changes to the package, repack the deb
    `dpkg-deb -b <dir> <new-package.deb>`
