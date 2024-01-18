![[Pasted image 20240104101719.png]]
Access ports are VLAN unaware hence, VLAN10 and VLAN20 become a single broadcast domain when they are connected together. Therefore, C & D will:

1.be able to communicate if they belong to the same subnet. Say C wants to communicate with D. It will first check to see if D is in the same subnet (using the IP of D and the Network address and Subnet mask of C) which is the case. So C will send a broadcast ARP request to resolve the IP address of D into a MAC address to which D will respond and they can start communicating.  

2. not be able to if they belong to different subnets. Say C wants to communicate with D. It will first check to see if D is in the same subnet (using the IP of D and the Network address and Subnet mask of C) which is not the case. So C will try to send an ARP request to the default gateway which is not configured using a router to resolve the IP address of D into a MAC address which fails.

