Services provided by the transport layer
1. process to process delivery (multiplexing and demultiplexing)
2. ordered transport
3. reliable data transfer
4. flow control
5. congestion control
- connection oriented transport (TCP) (implements all 5)
- connectionless transport (UPD) (only implements 1 and 2(very lightly))

Services that are not provided
- delay guarantees
- bandwidth guarantees

### Multiplexing and Demultiplexing
- Done using port numbers (16 * 2 bits src/dest)

### Socket Programming
TCP socket is identified by 4-tuple when demuxing:
- src IP
- src port
- dest IP
- dest port
UDP socket is identified only by destination port when demuxing

### UDP (User Datagram Protocol)
- Best effort service. That is, segments maybe
	- lost
	- delivered out of order
- Connectionless (simpler and faster than TCP, small header size)
- No congestion control
- Can function when network service is compromised (When the network service is poor for any reason. TCP will fail to work in these conditions. UDP can still get packets through, without worrying about packets in-order, duplicates, etc.)
- Uses:
	- streaming
	- DNS
	- SNMP
	- HTTP/3 (uses reliability and congestion control in application layer)
![[Pasted image 20240121201157.png]]
- Purpose of the cheksum is to detect errors
	- treat contents of UDP segment (including header fields and IP addresses) as a sequence of 16-bit integers
	- add these integers and put into checksum field
		- if while adding two integers thet create a carry bit, add it back to the sum

### TCP (Transmission Control Protocol)
- The protocol depends on the characteristics of the unreliable channel
	- If channel is perfectly reliable (no bit errors, no packets losses)
		![[Pasted image 20240215164808.png]]
	- If the channel now has bit errors - add checksum + ACK and NACK packets (which do not have bit errors)
		![[Pasted image 20240121203916.png]]
	- If the ACK and NACKs can get corrupted
		- Sender retransmits - creates another problem: Duplicates!
		- Add sequence number to each packet to handle duplicates
		![[Pasted image 20240121204157.png]]
		![[Pasted image 20240121204247.png]]
	- Making the protocol NAK free (**STOP AND WAIT** Protocol)
		![[Pasted image 20240121204528.png]]
		![[Pasted image 20240121204542.png]]
	- Now the channel has packet losses - transmit after a reasonable time
		![[Pasted image 20240121212325.png]]
			Stop and wait is slow. The sender waits about a RTT until the ACK is received before doing anything else
	- Pipelining for performance (Sliding window)
		- Send a range (N) of yet to be acknowledged packets = increase sequence numbers (Go-Back-N)
			- If the acknowledgment of a frame is not received within the timeout, all frames in the current window are transmitted
		- Selective repeat - maintains a timer for each unACKed packet and retransmits only them after the timeout. this protocol has issues with the sliding window stuck in place at the sender due to reusing sequence numbers. To avoid this The size of the sending and receiving windows must be equal, and half the maximum sequence number (assuming that sequence numbers are numbered from 0 to n−1) to avoid miscommunication in all cases of packets being dropped
			![[Pasted image 20240121214537.png]]

## TCP

![[Pasted image 20240121215031.png]]
- Point to point
- Reliable, in order byte stream
	- no message boundaries unline udp
- Full duplex
	- MSS - Maximum segment size
- Sequence number: A device initiating a TCP connection must choose a **random initial sequence number**, which is then **incremented according to the number of transmitted bytes**.
	- Why a random initial sequence?
- Acknowledgment number – The receiving device maintains an acknowledgment number starting with zero. It increments this number according to the number of bytes received.

TCP timeout
Defining the round trip time: Exponential Weighted Moving Average (EWMA)
$$EstimatedRTT = (1-\alpha).EstimatedRTT + \alpha.SampleRTT$$

$$TimeoutInterval = EstimatedRTT + 4*DevRTT$$

$$DevRTT = (1-\beta).DevRTT + \beta.|SampleRTT-EstimatedRTT|$$


TCP Reliability
_Is TCP a GBN or an SR protocol? Recall that TCP acknowledgments are cumulative and correctly received but out-of-order segments are not individually ACKed by the receiver. Consequently, the TCP sender need only maintain the smallest sequence number of a transmitted but unacknowledged byte (SendBase) and the sequence number of the next byte to be sent (NextSeqNum). In this sense, TCP looks a lot like a GBN-style protocol. But there are some striking differences between TCP and Go-Back-N. Many TCP implementations will buffer correctly received but out-of-order segments [Stevens 1994]. Consider also what happens when the sender sends a sequence of segments 1, 2, . . . , N, and all of the segments arrive in order without error at the receiver. Further suppose that the acknowledgment for packet n < N gets lost, but the remaining N – 1 acknowledgments arrive at the sender before their respective timeouts. In this example, GBN would retransmit not only packet n, but also all of the subsequent packets n + 1, n + 2, . . . , N. TCP, on the other hand, would retransmit at most one segment, namely, segment n. Moreover, TCP would not even retransmit segment n if the acknowledgment for segment n + 1 arrived before the timeout for segment n._
![[Pasted image 20240215220513.png]]
![[Pasted image 20240215220657.png]]
![[Pasted image 20240215220731.png]]


TCP Fast Retransmit
![[Pasted image 20240215220847.png]]


Flow Control
Receiver advertises free buffer space in rwnd flow control bits in header so the sender won't overflow receiver buffer. Then sender limits amount of unACKed data ro receive rwnd guaranteeing receive buffer will not overflow

TCP Handshake
- agree to establish connection
- agree on parameters:
	- starting seq #s
	- rwnd
- Why a three way handshake
	- Allows for a mechanism called "simultaneous open," where both the client and server can initiate a connection simultaneously. In a simple two-way handshake, it might not handle simultaneous initiation well. One party might start sending data assuming the connection is established, while the other is still in the process of initiating the connection.
	- Ensures that both the client and server agree on the initial sequence numbers and the connection state is synchronized.Without the explicit acknowledgment from the server, there may be uncertainty about the connection's state and sequence numbers.
	- Provides a mechanism for handling half-open connections, where one party initiates a connection, but the other party does not respond or completes the connection setup.In a simple two-way handshake, if one party does not explicitly acknowledge the connection, it might lead to confusion about the state of the connection.
- Steps
	- Establish Connection
		- SYN (Sender)
			- SYNbit = 1
			- Seq = x (chosen at random by sender)
		- SYN-ACK (Receiver)
			- SYNbit = 1
			- Seq = y (chosen at random by receiver)
			- ACKbit=1
			- ACKnum = x+1
		- ACK (Sender)
			- ACKbit=1
			- ACKnum=y+1
	- Close Connection
		- FIN (Sender)
			- FINbit=1
		- FIN-ACK (Receiver)
			- FINbit=1
			- ACKbit=1

Congestion Control
- End to end congestion
	- Inferred by TCP from observed loss, delay
- Network assisted congestion control
	- Routers provide direct feedback to sending/receiving hosts with flows passing through the congested router

AIMD (Additive Increase Multiplicative Decrease)
![[Pasted image 20240215191855.png]]
- Multiplicative decrease - the sending rate is
	- Cut in half on loss detected by triple duplicate ACK (TCP reno)
	- Cut to 1 MSS (maximum segment size) when loss detected by timeout (TCP tahoe)
- Why AIMD
	- distributed, asynchronous
	- optimize congested flow rates network wide
	- have desirable stability properties

Congestion Control Details
_LastByteSent - LastByteAcked <= min{CongWin, RcvWin}._