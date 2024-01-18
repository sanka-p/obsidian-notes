End to End Communication

Routing - Gathering information about the network
Forwarding - forward packets using forwarding table

Features of an IP address
- Logical
- Hierarchical
	- Makes routing tables small = routing becomes easy
	- Bulk delivery

For a pc to act as a router it should have multiple network interfaces

![[Pasted image 20240104024242.png]]

Class A
	Network - 8 bits (0xxxxxxx) - 2^7 = 128 possibilities
	Hosts - 2^24 ~ 16 million hosts
Class B
	Network - 16 bits (10xxxxxx xxxxxxxx) - 2^14 = 16k possibilities
	Hosts - 2^16 ~ 64k hosts
Class C
	Network - 24 bits (110xxxxx xxxxxxxx xxxxxxxx) - 2^22 = 2 million possibilities
	Hosts - 2^8 = 256 hosts

Private IP ranges
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16

Classfull addressing = waste of IPs
Hence CIDR (Classless Inter-Domain Routing) was introduced

### Route Aggregation
Splitting = move netmask to the right
Aggregating  = move netmask to the left

### Default Gateway
When a NIC receives a packet to be sent to another host, it checks the subnet mask, if the mask is same to its local network then it is locally delivered.

If the mask is for some host outside its local network then the packet is forwarded to the default gateway,

### NAT (Network Address Translation)
External DNS Zone
Internal DNS Zone

Benefits
- Provides a level of security
- Static NAT for servers - for servers inside the private ip addresses to be discovered by outside
- Dynamic NAT
Shortcomings
- VOIP
- Firewall

### PAT (Port Address Translation)
Static PAT for servers - for servers inside the private ip addresses to be discovered by outside
	- But now the client must know the IP + Port