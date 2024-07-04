Purpose: Transfer **frame** between **physically** adjacent **nodes** (hosts, routers) along the connecting **link** (wired/wireless)

Lower level implemented in each and every host on the **NIC (Network Interface Card)** and upper level in software

Services:
- Framing
	- Header and a Trailer
	 - MAC addressing
- Reliable hop to hop delivery (especially on wireless links with high error rates)
	- no point in sending a corrupted packet half way around the world if we can drop it early
- Flow Control
- Error Detection
	- caused by attenuation, noise
	- on error, drops frame or signals retransmission
- Error Correction
- Half duplex and Full duplex

### Error Detection and Correction (EDC)
- EDC bits are in the trailer
- Parity
	- Single bit parity - detect single bit errors
	- 2D bit parity - detect and correct single bit errors
		![[Pasted image 20240118205100.png]]
- Internet Checksum
	- Treat UPD segment including headers as a sequence of 16 bit integers
	- Get one's complement sum
	- Put in UDP checksum field
- Cyclic Redundancy Check (CRC)
	- More powerfull
	- Widely used (Ethernet, 802.11 WiFi)

### Media Access Control (MAC) Protocols
- A distributed algorithm that determines how multiple devices can use a shared medium to communicate
- Communication about the channel sharing happens in the channel itself
- Multiple ways to do this:
	- Channel Partitioning
		- **Time Division Multiple Access (TDMA)**
			- Each station gets a fixed length slot in each round that is equal to the packet transmission time
		- **Frequency Division Multiple Access (FDMA)**
			- Each station gets assigned a fixed frequency band in the spectrum
	- **Random Access Protocols**
		- A node transmits at full channel data rate when needed
		- When another node tries: collision
		- The protocol specifies how to detect and avoid the collision
		- Examples:
			- **Pure ALOHA**
				- When frame arrives, transmit immediately
				- Collision probability increases with no synchronization
				- Efficiency is 18%
			- **Slotted ALOHA**
				- Time divided into equal size required to send one frame (assuming all frames are same size)
				- Synchronized nodes that transmit only at the beginning of slot
				- When a node gets a frame, transmit in next slot
				- If many transmits, collision - node retransmits frame in each subsequent slot with a probability p (randomization) until success
				![[Pasted image 20240118211611.png]]
				- Pros:
					- Single active nodes can transfer continously at full rate
					- Highly decentralized, only slots in the nodes need to be in sync
					- Simple
				- Cons
					- Collisions wastes slots + devices may not need the entire slot to detect the collision
					- Idle slots
					- Clock synchronization
					- Max efficiency is 37%
			- **Carrier Sense Multiple Access (CSMA)**
				- **Listen before transmit**
					- if channel is idle: transmit entire frame
					- if channel is busy: defer transmission
				- Collisions can still happen due to propagation delay
				- Variations of CSMA
					- CSMA/CD
					- CSMA/CA
					- These variations use different algorithms to determine when to initiate transmission onto the shared medium
						- 1-persistent - used in CSMA/CD systems including ethernet
```
while true // continous monitoring
  if busy continue
  if idle
    transmit
    if collision wait(random_time)
```
``
						- P-persistent - used in CSMA/CA systems like WIFI
```
while true // continous monitoring
	if busy continue
	if idle
		transmit with probability p
```
``
						- Non persistent
```
while true
  if busy don't transmit
  if idle
     trasmit
  wait(ransom_time) // doesn't check continously
```
``
						- O-persistent- Each node is assigned a transmission order by a supervisory node. When the transmission medium goes idle, nodes wait for their time slot in accordance with their assigned transmission order. The node assigned to transmit first transmits immediately. The node assigned to transmit second waits one time slot (but by that time the first node has already started transmitting). Nodes monitor the medium for transmissions from other nodes and update their assigned order with each detected transmission (i.e. they move one position closer to the front of the queue).
```

```
``
			- **CSMA with Collision Detection (CSMA/CD)**
				- Collision detected within a short time
				- If NIC detects another transmission while sending
					- abort, send jam signal
					- enter binary (exponential) backoff
					- try CSMA again
				- Easy in wired (used in Ethernet), difficult in wireless
			- **CSMA with Collision Avoidance (CSMA/CA)**
				- Used in 802.11
	- **Taking Turns Protocols/Control Access Protocols**
		- Partitioning protocols works well for high loads
		- Random access works well for low loads
		- Taking turns is the best of both worlds
		- Examples
			- **Polling**
				- Master node invites other nodes to transmit in turn
				- Typically used with "dumb" devices
				- Issues:
					- Polling overhead
					- Latency
					- Single point of failure (master)
			- **Token Passing**
				- Control token passed from one node to next sequentially
				- Issues:
					- Token overhead
					- Latency
					- Single point of failure (token)
			- **Cable Access Network (CAN)**
				- Multiple downstream FDM channels
					- single cable modem termination system CMTS transmits into channels
				- Multiple upstream channels blah blah blah...
				Yo go through this shit again i didnt understand anything
			- Bluetooth, FDDI, token ring

### MAC Addressing
- Why the need for addressing
	- Shared delivery mechanism
	- Multiplexing and demultiplexing
- 48-bit address (1A-2F-BB-76-09-AD)
	- Allocation administered by IEEE
- Burned in NIC ROM, sometimes software settable
- Types:
	- Broadcast -  To everyone in the network
	- Unicast
	- Multicast
	- Anycast - Doesn't care about who does the task required

### Address Resolution Protocol (ARP)
- ARP Table - Each host has a <IP,MAC,TTL> address mapping table
	- TTL is the time after which the mapping will be forgotten (typically 20 min)
	
	>💻
	>To view the arp table: `arp`
	>To clear the arp table: `ip -s -s neigh flush all`

### Ethernet
- Physical topology
	- Bus
		- All nodes in the same collision domain
		![[Pasted image 20240215134038.png]]
	- Switched
		- Each spoke runs a seperate ethernet protocol (nodes do not collide)
		![[Pasted image 20240215134102.png]]
- Ethernet frame structure **common**
	- preamble
		- used to synchronize reciever, sender clock rates
		- 7 bytes of 10101010 followed by 1 bytes of 10101011
	- dest MAC
	- src MAC
	- type
		- indicates the higher layer protocol (mostly IP and NOvell IPX, AppleTalk)
	- CRC
		- when error is detected, frame is droppped
- connectionless: no handshaking between sending and receiving NICs
- unreliable: no ACKs or NAKs
- The MAC protocol used is unslotted CSMA/CD with binary backoff **common**
- different speeds: 2Mbps, ..., 40 Gbps
- different physical layer media: fiber (100BASE-FX, 100BASE-SX, 100BASE-BX), cable (100BASE-TX, 100BASE-T2, 100BASE-T4)

### Switches
- Stores (buffers) and **selectively** forwards ethernet frames
- When the frame is to be forwarded on segment, uses CSMA/CD to access segment
- Ethernet protocol used on each incoming link
	- no collisions; full duplex
	- each link is its own collision domain
- Switch has a switch table: each entry contains MAC, Switch Port and Timestamp
	- This table is updated using MAC learning
	- If entry found for destination
		- if destination on segment from which frame arrived drop frame
		- else selective forward
	- else flood
- Self learning multi switches
	![[Pasted image 20240215141948.png]]
	- A switch can have multiple MAC addresses associated with a single port. This is common when the switch is connected to another switch

Switching Methods (#TODO where does this fall into)
- Circuit Switching
- Packet Switching
- MPLS - Allows IP packets to be forwarded at layer 2-switching level without being passed up to layer 3
	- Inserted between layer 3 and 2 headers
	- This allows routers to act like switches on a local network
	- MPLS enabled routers have a seperate MPLS forwarding table and forwards packets to outgoing interface based only on label value
		- Faster lookup using fixed length indentifier

## VLAN
Why VLANS?
- Scaling up with a single broadcast domain reduces efficiency, security, privacy
- Administrative issues - device physicall moves into different area of network but wants to be connected to the old LAN

VLAN frames are different from normal ethernet frames
![[Pasted image 20240215142738.png]]

Subnet:VLAN
1:1 - This is what should be done ✅
N:1 - Large broadcast domain 
1:N - Not in the same broadcast domain
N:M

Two types of ports in a switch
- Access - Carries traffic in assigned VLAN
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


## Datacenter Networks
10s to 100s to thousands of hosts, often closely coupled in close proximity
- ebuisiness
- content servers (youtube)
- search engines, data mining
![[Pasted image 20240215144336.png]]
