### setup a desired ruby version manually with rvm in debian

```
sudo apt install curl g++ gcc autoconf automake bison libc6-dev libffi-dev libgdbm-dev libncurses5-dev libsqlite3-dev libtool libyaml-dev make pkg-config sqlite3 zlib1g-dev libgmp-dev libreadline-dev libssl-dev

sudo apt install dirmngr

command curl -sSL https://rvm.io/mpapis.asc | gpg --import -

command curl -sSL https://rvm.io/pkuczynski.asc | gpg --import -

curl -sSL https://get.rvm.io | bash -s stable

source ~/.rvm/scripts/rvm

rvm install 2.4.1 ``` 

(may get the exact version from .ruby-version file @ git)
```
rvm use 2.4.1 --default

ruby -v
```
