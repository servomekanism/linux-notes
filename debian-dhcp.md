[//]: # (tags: dhcp isc-dhcp-server)
### setup debian basic dhcp server
```
apt install isc-dhcp-server
cp /etc/default/isc-dhcp-server{,.original}
vim /etc/default/isc-dhp-server
```
Modify `/etc/default/isc-dhcp-server` to read:
```
DHCPDv4_CONF=/etc/dhcp/dhcpd.conf
DHCPDv4_PID=/var/run/dhcpd.pid
INTERFACESv4="eth0"
INTERFACESv6=""
```
```
cp /etc/dhcp/dhcpd.conf{,.original}
vim /etc/dhcp/dhcpd.conf
```
Modify `/etc/dhcp/dhcpd.conf` to read:
```
ddns-update-style none;
default-lease-time 600;
max-lease-time 7200;
#no service will be provided at the VMware NAT network
subnet 172.16.248.0 netmask 255.255.255.0 {
}

subnet 172.16.2.0 netmask 255.255.255.0 {
    option subnet-mask 255.255.255.0;
    option broadcast-address 172.16.2.255;
    option routers 172.16.2.62;
    option domain-name-servers 8.8.8.8;
    range 172.16.2.65 172.16.2.67;
}
```
Start the dhcp server
`systemctl start isc-dhcp-server`
