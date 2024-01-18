Hop to Hop Communication

Why the need for addressing
- Shared delivery mechanism
- Multiplexing and demultiplexing

Broadcast -  To everyone in the network
Unicast
Multicast
Anycast - Doesn't care about who does the task required
## VLAN
Subnet:VLAN
1:1 - This is what should be done ✅
N:1 - Large broadcast domain 
1:N - Not in the same broadcast domain
N:M

Two types of ports in a switch
- Access
- Trunk - Carries traffic from different VLANS

When two access ports of two different VLAN are connected they become one single broadcast domain

Trunking
Without trunking a pair of access ports from each vlan of each switch should be connected which is a waste of ports. Therefore use a trunk port to send frames of multiple VLANS

Broadcast Storm
Broadcast radiation and storming occur when a broadcast frame (DHCP or ARP request) or a multicast frame (OSPF or EIGRP message) enters a switch, is propagated out to all ports, then loops back to all the ports on the devices it just went to. The frame then ends up stuck in a cycle where it goes back to the original device and just keeps regenerating itself until the wire is so bogged down

Solutions:
- Switches will ensure there are no loops by creating a minimum spanning tree


VLAN Tagging
No tag (for vlan with most anticipated traffic) . This is known as the Native VLAN (by default VLAN 1 but can be changed)

A native VLAN mismatch  occurs when two switches on the same trunk link have different native VLANs configured

Default VLAN - all switch ports belong to this vlan at bootup (it is VLAN 1 and cannot be changed). When connecting to old switches that have no VLANS frames gets forwarded to default VLAN

Management Port
- Remote Access to Configure the Switch
- Some old switches require that management port should be on VLAN 1

### MAC Learning
MAC tables

### ARP
Say 2-PCs connected via a direct straight through cable (PC_1 and PC_2)

PC_1 pings PC_2 (PC_1 only knows PC_2’s IP)

PC_1 checks his ARP cache

If not found, broadcast ARP Request (”Who is? <ip_2>”)

PC_2 recieves that ARP Request and Replies as a Unicast ( <mac_2> “is” <ip_2>)


ARP Probe and ARP Annoucement
The idea is if a host acquires and puts to use an IP address that happens to already be in use on the network, it will cause connectivity issues for both hosts. As such, it is beneficial for a host to first test an IP address before putting it to use to ensure it is indeed unique.
send a few ARP Probes (typically 3), and if no one responds, officially claim the IP address with an ARP Announcement.
![[Pasted image 20240104033900.png]]
