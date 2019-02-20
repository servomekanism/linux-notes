[//]: # (tags: macchanger macspoofing)
### after installing macchanger:
1. create systemd service at `/etc/system/system/macspoof@.service:
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
