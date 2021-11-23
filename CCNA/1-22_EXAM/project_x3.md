# project_x3
VLAN 1200 - /21  27.27.248.0/27
VLAN 500 - /23   27.28.0.0/23
VLAN 200 - /24   27.28.2.0/24
Mgmt  - /30        27.28.3.0/24
M-n -   /30          27.28.3.4/30



**NFL SWITCH**
```
write erase
del flash:vlan.dat
reload
```


enable
confg t
hostname NFL
username roman password cisco123
ip domain-name roman.com
no ip domain-lookup
crypto key generate rsa general-keys modulus 2048
line console 0
loggin synchronous
login local
exec-timeout 10
exit
line vty 0 15
login local
transport input ssh
exec-timeout 8

vtp mode transparent # need to create vlan 1200 

vlan 1200
vlan 200
vlan 500
spanning-tree mode rapid-pvst
interface fa0/5
switchport mode access
swithchport access vlan 500
switchport port-security
switchport port-security mac-address sticky
interface fa0/12
switchport mode access 
switchport access vlan 1200
switchport port-security
switchport port-security mac-address sticky
interface gi0/2
switchport mode trunk
switchport trunk allowed 1,500
switchport trunk allowed vlan add 1200 # need to add 1200 separately
do show trunk

**+++++*MLS*+++++++**

enable
configure terminal
hostname soemname
username roman password cisco123
enable secret cisco
ip domain-name roman.com
crypto key generate rsa general-key modulus 2048
line console 0
logging synchronous
login local
exec-timeout 10
exit
line vty 0 15
login local
transport input ssh
exec-timeout 8
exit
vtp mode transparent


vlan 1200
vlan 500
vlan 200
ip routing # to TURN ON L3 FOR L3SWTICH
interface fa0/2
switchport mode access 
switchport acess vlan 200
switchport port-security
switchport port-security mac-address sticky
exit
interface fa0/1
no switchport
ip address 27.28.3.5 255.255.255.252
interface gi0/1
no switchport
ip address 27.28.3.6 255.255.255.252
interface gi0/2
switchport mode trunk
switchport mode dynamic desirable
switchport trunk
switchport trunk allowed 1,500
switchport trunk allowed vlan add 1200 # need to add 1200 separately

interface gi0/1
ip address 27.28.3.2 255.255.255.252
exit
interface vlan 1200
ip address 27.27.248.1 255.255.248.0
interface vlan 500
ip address 27.28.0.1 255.255.254.0
interface vlan 200
ip address 27.28.2.1 255.255.255.0


ip route 0.0.0.0 0.0.0.0 gi0/1

ip dhcp excluded-address 27.27.248.1 27.27.248.10
ip dhcp pool VLAN200
network 227.27.248.0 255.255.255.248.0
default-router 27.27.248.1
dns-server 8.8.8.8

ip dhcp excluded-address 27.27.248.1 27.27.248.10
ip dhcp pool VLAN200
network 227.27.248.0 255.255.255.248.0
default-router 27.27.248.1
dns-server 8.8.8.8

ip dhcp excluded-address 27.27.248.1 27.27.248.10
ip dhcp pool VLAN200
network 227.27.248.0 255.255.255.248.0
default-router 27.27.248.1
dns-server 8.8.8.8

ip dhcp excluded-address 27.27.248.1 27.27.248.10
ip dhcp pool VLAN200
network 227.27.248.0 255.255.255.248.0
default-router 27.27.248.1
dns-server 8.8.8.8

**+++++++++++++NBA+++++++++++++++**

int gi0/1
ip address 27.28.3.1 255.255.255.252
not shutdown
do ping 27.28.3.2
int gi0/0
ip address 5.5.5.2 255.255.255.252
no shutdown

+++routing+++
ip route 27.27.248.0 255.255.248.0 27.8.3.2
ip route 27.28.0.0 255.255.254.0 27.8.3.2
ip route 27.28.2.0 255.255.255.0 27.8.3.2
ip route 27.28.3.4 255.255.255.252 27.8.3.2

**+++++++++++isp router+++++++++**

int gi0/1
ip address 5.5.5.1 255.255.255.252
no shutdown

+++++++++NBA+++++++++

ip access-list standard MLB
deny 27.28.0.0 0.0.1.255
permit any 

ip nat inside source list MLB interface gi0/0 overload
interface gi0/0
ip nat outside
interface g0/1
ip nat inside


