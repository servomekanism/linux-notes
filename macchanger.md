[//]: # (tags: macchanger macspoofing)
### setup fixed interface names by editing `/etc/udev/rules.d/10-network.rules`:
```
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="XX:XX:XX:XX:XX:XX", NAME="wlan0"
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="XX:XX:XX:XX:XX:XX", NAME="eth0"
```

### install `macchanger`

### after installing macchanger:
1. create systemd service at `/etc/system/system/macspoof@.service`:
    ```
    [No write since last change]
    [Unit]
    Description=macchanger on %I
    Wants=network-pre.target
    Before=network-pre.target
    BindsTo=sys-subsystem-net-devices-%i.device
    After=sys-subsystem-net-devices-%i.device

    [Service]
    ExecStart=/usr/bin/macchanger -r %I
    Type=oneshot

    [Install]
    WantedBy=multi-user.target
    ```
2. enable it for each interface you want it:
    
    `systemctl enable macspoof@eth0`
    
    `systemctl enable macspoof@wlan0`

*source*: https://wiki.archlinux.org/index.php/MAC_address_spoofing#macchanger_2
