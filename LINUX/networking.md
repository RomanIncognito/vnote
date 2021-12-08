# networking

Folders/files : 
    - /etc/network/interface
    - /etc/sysconfif/network-scripts

To restart networking:
    - ubuntu - systemctl restart networking.server (restarting NetworkManager is NOT doing the trick) or `nmcli networking off/on`
    - rocky. Might have to add new file for new interface in folder network-scripts. Then bring it up with `ifup ens37` and then `nmcli networking off/on`
    

**interface file (UBUNTU):** 
dhcp 
```
auto eth1
iface eth1 inet dhcp
```

static
```
iface eth1 inet static
address 10.0.0.15/8
gateway 10.0.0.1
```

**ifcfg-name_of_interface file (rocky):** 
DEVICE=ens33
ONBOOT=yes
BOOTPROTO=dhcp