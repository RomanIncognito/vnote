# commands

DEFAULT SETTING RESET
```
write erase
del flash:vlan.dat
reload
```

HOUSEKEEPING

```
en
config t
hostname myswitch1
enable secret cisco
ip default-gateway 10.1.1.1
ip domain-name roman.com
crypto key generate rsa general-keys modulus 2048
int vlan 1
ip address 10.1.1.1 255.255.255.0      # settup ip for switch to connect via telnet/ssh
username roman password cisco123

line con 0
logging synchronous
exec-timeout 10
login local  #user command "login" to activate password only, without user
exit

line vty 0 4
transport input ssh
login local
password cisco123  # specify password ssh (without ssh)
```

`banner login ^This is banner^`

CREATE DEDUNDANCY IN CHANNEL (2 protocols)
LACP
```
config t
interface range fa0/1-2
channel-group 1 mode active
```
PAgP
```
config t
interface range fa0/1-2
channel-group 1 mode desirable  # on other side - auto
```

RSTP
```spanning-tree mode rapid-pvst```

ASSIGN ROOT in STP
```
spanning-tree vlan 1 root primary  # root secondary
```

SETTING UP TRUNK
```
show vtp status
config t
vtp domain roman.com
vtp version 2
vtp password my_password
vtp mode <options>  # options - client, server, transparent
```

CREATE 2 VLANs AND TRUNK
```
vlan 100
name name_of_vlan
exit
interface range fa0/1-10
switchport mode access
switchport access vlan 100
#same for fa0/11-20 $ vlan 200
interface gi0/1
switchport mode trunk
switchport trunk encapsulation dot1q  # default behaivior
```

ALTER PRIORITY OF SWITCH
```
show spanning-tree vlan x
config t
spanning-tree vlan X priority 32...
```

ADDING STATIC ROUTE
```
show ip route static
config t
ip route 10.0.0. 255.255.255.0 10.0.0.1
```

SETTING UP IP FOR A PORT
```
interface Gio/1
ip address 192.168.0.1 255.255.255.0
no shutdown # short "no shut"
```

SETTING CUSTOM ADMIN DISTANCE
`ip route 172.16.2.0 255.255.255.0 172.16.2.1 130`

STATIC DEFAULT ROUTE (GATEWAY OF LAST RESORT)
`ip route 0.0.0.0 0.0.0.0 S0/0/1`

FLOATING STATIS ROUTE (ACHIEVED WITH VARIABLE ADMIN. DISTANCE)
```
ip route 172.17.201.0 255.255.255.0 123.0.0.1
ip route 172.17.201.0 255.255.255.0 23.0.0.3
```

OSPF show commands

```
show ip ospf <interface>
show ip ospf
show ip ospf neighbor
show ip ospf database
```

calculate cost for OSPF
cost = 100000 / bandwidth
so one wants cost 64, bandwidth must be 1562

unlike STP. higher # is higher priority that is used in election of DR, BDR

**OSPF**

```
en -> config t
router ospf 1
network 10.0.0.0 0.0.0.255 area 0  # for distribution port
network 172.12.0.0. 0.0.0.7 area 0 # for other routers
passive-interface gi0/0 # tells OSPF not send HELLO to port
interface s0/0/0
bandwidth 56 #change bandwidth manually
```

CHANGE OSPF PRIORITY
```
config t
int gi0/0  # one must enter specific interface
ip ospf priority 99  # default is 1
router-id # other option to user router-id that has the same format as ip
```

OSPF pont-to-point
int gi0/0/0
ip ospf netowkr point-to-point6

IP DEFAULT ROUTE ::/0 
IPV6 loopback ::1/128

SETUP IPV6 IN ROUTER
```
config t
ipv6 unicast-routing
interface gi0/0/0
ipv6 address fd01::1/64
no shut
do show ipv6 int br
```

ADD STATIC ROUTE
`ipv6 route bb::/64 2020:dead::2`

ACTIVATING SLAAC

```
ipv6 unicast-routing
interface gi0/1
ipv6 address autoconfig
do show ipv6 int br
```

IPV6 IN OSPFV3

```
# THERE IS NO 'NETWORK' COMMAND IN OSPFV3.
# TO ASSIGN/ENABLE IPV6 OSPF ON INTERFACE, GO IN AND USE "IPV6 OSPF" COMMAND
#establish ip for each interface with ipv6 address

ipv6 unicast-routing
router-id 1.1.1.1
ipv6 router ospf 1
do show ipv6 ospf neighbor
show ipv6 route
INT SERIAL0/0/0
ipv6 ospf 1 area 0
exit
```

ACL
#NOT MATCHED PACKETS ARE DISCARDED IF AT LEAST ONE ROUTE WAS ESTABLISHED.
#IT IS EMPLICIT AS LAST/DEFAULT RULE
#CAN BE ALTERED BY ADDING EXPLICIT LAST RULE
```
access-list 1 permit any # STANDARD
access-list 101 permit ip any any # EXTENDED
```

STANDARD
```
access-list 1 permit 10.1.1.1
access-list 1 deny 10.1.1.0 0.0.0.255
int s0/0/0  # access list is applied on specific interface
ip access-group 1 in
show access-list
show ip interface s0/0/0 | include access list
```

ACL EXTENDED TEMPLATE
```
access-list <num> {dny|permit} {tcp|udp} src src_wildcard operator port dst dst_wcard operator port 
#instead of network one can use "host <IP>"
if both tcp and udp , use "ip"
```
EXAMPLE
```
int serial0/0/0
ip access-group 101 in
exit
access-list 101 dny tcp host 172.16.3.10 172.16.1.0 0.0.0.255 eq ftp
access-list 101 deny tcp 172.16.3.0 0.0.0.255 172.16.1.0 0.0.0.255 eq www
access-list 101 permit ip any any
```

ACL EXTENDED example 2
```
access-list 150 permit tcp host 192.168.33.3 host 172.22.242.23 eq 80
access-list 150 deny ip any host 172.22.242.23
access-list permit ip any any
int gi0/1
ip access-group 150 out
```

ACL NAMED standard
```
ip access-list standard 24 # new style acceslist
permit 10.1.1.0 0.0.0.255
.
.
do show ip access-list 24
no 20 # deleting line marked as 20
5 deny 10.1.1.1 # inserting ln in between lines OR even in the front
```
 
ACL NAMED EXTENDED
```
ip access-list extended acl1
deny ip 192.0.2.0 0.0.255.255 host 192.0.2.10 log
permit tcp any any 
interface fastethernet 0/0/0
ip access-group acl1 out
```

SWITCHPORT PORT-SECURITY
```
switchport port-security  # enable port security
switchport port-security maximum <n> # change max. allowed. max is default=1
switchport port-security vialation {protect|restrict|shutdown}
switchport port-security mac-address <mac_address> # static port security
switchport port-security mac-address sticky
show port-security interface fa0/2
show mac address-table secure
show mac address-table static
```

SIMPLE DHCP
```
Floor1(config)#ip dhcp excluded-address 192.168.0.1 192.168.0.50
Floor1(config)#ip dhcp pool Floor1DHCP
Floor1(dhcp-config)#network 192.168.0.0 255.255.255.0
Floor1(dhcp-config)#default-router 192.168.0.1
Floor1(dhcp-config)#dns-server 192.168.0.1
```

DHCP relay
```
-------g0/0 R-----------DHCPs172.16.2.11
int g0/0
ip helper-address 172.16.2.11
show ip int g0/0
```

Switch obtaining ip through DHCP
```
int vlan 1
ip address dhcp
no shut
show interface vlan 1
show ip default-gateway
```

DHCP SNOOPING
```
ip dhcp snooping # enabling dhcp snooping
ip dhcp snooping vlan 1
int g0/22 # port connected to legit dhcp
ip dhcp snooping trust
show ip dhcp snooping database
show ip dhcp snooping binding
ip dhcp snooping information option # enabling dhcp option-82 data insertion
```

ARP inspection DAI
```
ip arp inspection vlan 1# ACTIVATING DAI
int gi0/22
ip arp inspection trust
show ip arp inspection
ip arp inspection validate src_mac
ip arp inspection validate dst_mac
ip arp inspection validate ip
ip arp inspection validate src_mac, dst_mac, ip
# CHECK IF VLAN 1 is UP
```

ENCAPSULATION dot1Q

router
```
int gi0/0/1
no ip address
exit
int gi0/0/1.10
encapsulation dot1Q <vlan-id>
ip address 192.168.110.1 255.255.255.0
```

NAT. STATIC
```
ip nat inside source static 10.1.1.2 170.168.2.3
ip nat inside source static 10.1.1.3 170.168.2.4
int gi0/0
ip nat inside
int gi0/1
ip nat outside
show ip nat transactions
show ip nat statistics
```

NAT DYNAMIC
```
access-list 42 permit 10.1.1.0 0.0.0.255
ip nat pool poo_name 170.168.2.3 170.168.2.5 netmask 255.255.255.0
ip nat inside source list 42 pool pool_name
int gi0/0
ip nat inside
int gi0/1
ip nat outside
show ip nat transactions
show ip nat statistics
```

NAT PAT 
```
access-list 100 permit ip 10.1.1.0 0.0.0.255 any
ip nat inside source list 100 interface s0/0/0 overload
int gi0/0
ip nat inside
int s0/0/0
ip nat outside
```

NTP
```
#SERVER
show clock
clock set 16:12:12 22 April 2021
ntp master <statum_num> # on server side ntp master 5
# setup OSPF (including loopback - 172.16.30.1)

#CLIENT
# on client side setup ospf with loopback as well
ntp server 172.16.30.1
```

HSRP
```
int fa0/0 # enter interface on which HSRP must be established. priority 100 is default
standby <group-num> ip <ip-num>  # the only 'must execute' command
standby <group-num> priority <pr>
standby <group-num> preempt
standby <group-num>  timers 4 12
standby version 2 # version 2. must be executed first
```

IOS (12,13)

```
enable
copy running-config tftp # backup
copy tftp flash # back is return back on device
boot system flash:c2660-advioservice9.bin
reload
dir flash
show server
```
IOS upgrade (15,  16)
works with add-ons
license boot module c3900 techlogy-package data















