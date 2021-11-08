# ipv6
ipv6 is next generation layer 3 protocol/address of  128  that is represented in Hex (base 16) form. Full length is 8 sets of 4 hexadecimal digits (or 16 bits block).

ipv6:
- use hexidecimal form which saves room representing data.
- ipv6 uses more modern CIDR notation instead of mask to define the network
- ipv6 is using a bit simplified, henced more efficient header.

Communication Methods:
• Dual Stack Method - running both IPv4 and IPv6.
• Tunneling method
• NAT protocol Translation

in 2019, IANA considered all RIRs except AFRINIC to have exhausted their supply of IPv4 addresses.

Network Address Translation (NAT) and classless interdomain routing (CIDR) helped extend IPv4’s life another couple of decades.

OSPF version 3, was created to support IPv6.

Two basic rules let you, or any computer, shorten or abbreviate an IPv6 address:
    1. Inside each quartet of four hex digits, remove the leading 0s (0s on the left side of the   
        quartet) in the three positions on the left. (Note: at this step, a quartet of 0000 will
        leave a single 0.)
    2. Find any string of two or more consecutive quartets of all hex 0s, and replace that
        set of quartets with a double colon (::). The :: means “two or more quartets of all 0s.”
        However, you can use :: only once in a single address because otherwise the exact
        IPv6 might not be clear.
        
The prefix length defines how many bits of the IPv6 address define the IPv6 prefix
IPv6 prefix is subnet ID.
If the prefix length is /P, use these rules:
1. Copy the first P bits.
2. Change the rest of the bits to 0. 

(A prefix length is a multiple of 4)

IPv6 does not use any concept like the classful network concept used by IPv4.

Addressing: global unicast(similar to public in v4) OR unique local IPv6 (similar to private v4)

Global routing prefix - reserved block of ipv6 address.

Global unicast 2 or 3 (originally); all not otherwise reserved (today)
Unique local FD
Multicast FF
Link local FE80

**enabling IPV6 in router!!!!!!!!!!**
>config t
>ipv6 unicast-routing
>do sh ipv6 int br
>interface Gi0/0/0
>ipv6 address AA::1/64
>no shutdown
>do show ipv6 int br

IPV6 host 64 padded by zeros from the right + 48(MAC modified) + 16 bits (FFFE) injected right in the middle of MAC 
