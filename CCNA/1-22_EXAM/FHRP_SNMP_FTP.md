# FHRP_SNMP_FTP

First Hop Redundancy Protocols (FHRPs) provides redundancy for the function of the default router in any subnet.
FHRP names a family of protocols that fill the same role.

HSRP - HOT Standby Router Protocol most popular option of FHRP.

```
standby 1 {priority 110} # default 100
standby 1 preempt
standby 1 ip <virtual ip from the same network as real ip>
```

Single point of failure is a part or point in the network that, in case of not proper functio, can influence another part of the network.

![](vx_images/217425702816916.png)

HSRP operates with an active/standby model (also more generally called active/passive).


The active/standby model of HSRP means that in one subnet all hosts send their off-subnet packets through only one router.
HSRP does support load balancing by preferring different routers to be the active router in different subnets.

--------------------------------------------------------------------------------------------------

versions used today: SNMPv2c  and SNMPv3.
SNMP is an application layer protocol that provides a message format for communication between what are termed managers and agents.
network management system (NMS) - part of the SNMP system that controls all other agents installed on every network node.
The SNMP agent is software running inside each device.

Each agent keeps a database of variables that make up the parameters, 
status, and counters for the operations of the device. This database, called the Management Information Base (MIB)

NMS uses the SNMP Get, GetNext, and GetBulk messages

agent can initiate conenction to station with notifications, use two specific SNMP messages: Trap and Inform.
Inform messages are like Trap messages but with reliability added. Added to the protocol with SNMP Version 2 (SNMPv2)
MIB defines each variable as an object ID (OID).

ACL that permits packets sourced from the IP addresses of all NMS hosts, but no others.

SNMPv3 arrived with : message integrity, authentication, encryption.

**+++++++++++++++++++++++++FTP++++++++++++++++++++++++++++++++++++++++**

File Transfer protocol.

File System is a set of fules how files should be stored in the system.

`show file systems`
`show flash`
`dir`
`copy tftl flash`
`verify /md5 flash0:the_name_of_the_file.bin the_md5_hash`
`copy ftp://wendell:odom@192.168.1.170/the_file.bin flash`
to comfigure username is password : 
```
ip ftp username wendell
ip ftp password odom
```

FTP uses TCP

The commands that flow over this initial TCP connection—called the FTP control connection (port 21).
FTP reserves two well-known ports: port 21 for control connections and port 20 for data connections.
Active mode (FTP PORT command) it's when both connection is initiated by client, but data session - by server.
Meaning data session might be blocked by firewall in the client side. 
Passive mode (FTP PASV command) helps solve the firewall restrictions by requesting from the server 2-d port 
for data exchange by client and initiating data connection by client as well.
In Passive mode client will initiate data connection , that's why data session will not be blocked by wirewall

FTP AUTH command - ftp client command to initiated ftps

TFTP uses UDP 69. 
TFTP CANNOT change dir. , create dir., list files. 

+++++++++++++++QoS+++++++++++++++++++
Bandwidth - speed of a link
Delay - time a packet takes to go from source tp destination and back
Jitter - defference in time of a series of packets being received at destination.
Loss - percentage of packets lost in the transit.
QoS allows you to choose with traffic should be prioritized. 

Fields that used for filtring:
ip header: protocol, source ip, destination ip
tcp header: source port, destination port.

224.0.0.2 - all routers multycast address.


++++++++++++IOS upgrade+++++++++
10.0.0.1 # running ios 12
enable
copy running-config tftp # 

copy tftp flash: # copy from tftp

confgire t
boot system flash:C2600-advipservice9-mz.124.15.bin
reload 

dir flash:
show version # to check which file is reference during loading

10.0.0.3 ios 15
enable 
show version
license boot ............................. # to activate add-ons on the ios 15-16






















