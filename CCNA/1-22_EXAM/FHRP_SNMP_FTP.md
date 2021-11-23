# FHRP_SNMP_FTP

First Hop Redundancy Protocols (FHRPs) provides redundancy for the function of the default router in any subnet.
FHRP names a family of protocols that fill the same role.

HSRP - HOT Standby Router Protocol most popular option of FHRP.

Single point of failure is a part or point in the network that, in case of not proper functio, can influence another part of the network.

![](vx_images/217425702816916.png)

HSRP operates with an active/standby model (also more generally called active/passive).


The active/standby model of HSRP means that in one subnet all hosts send their off-subnet packets through only one router.
HSRP does support load balancing by preferring different routers to be the active router in different subnets.

--------------------------------------------------------------------------------------------------

versions used today: SNMPv2c  and SNMPv3.
SNMP is an application layer protocol that provides a message format for communication between what are termed managers and agents.
The SNMP agent is software running inside each device.

Each agent keeps a database of variables that make up the parameters, 
status, and counters for the operations of the device. This database, called the Management Information Base (MIB)
NMS uses the SNMP Get, GetNext, and GetBulk messages

agent can initiate conenction to station with notifications, use two specific SNMP messages: Trap and Inform.
Inform messages are like Trap messages but with reliability added. Added to the protocol with SNMP Version 2 (SNMPv2)
MIB defines each variable as an object ID (OID).



