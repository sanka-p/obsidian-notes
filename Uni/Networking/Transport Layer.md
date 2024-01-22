Services provided by the transport layer
- process to process delivery (multiplexing and demultiplexing)
- reliable data transfer
- flow control
- congestion control
- connection oriented transport (TCP)
- connectionless transport (UPD)

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
- Can function when network service is compromised (no need to worry about SYN flood attacks)
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
		![[Pasted image 20240121203835.png]]
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
			Stop and wait is slow
	- Pipelining for performance (Sliding window)
		- Send a range (N) of yet to be acknowledged packets = increase sequence numbers (Go-Back-N)
			- If the acknowledgment of a frame is not received within the timeout, all frames in the current window are transmitted
		- Selective repeat - maintains a timer for each unACKed packet and retransmits only them after the timeout. this protocol has issues with the sliding window stuck in place at the sender due to reusing sequence numbers. To avoid this The size of the sending and receiving windows must be equal, and half the maximum sequence number (assuming that sequence numbers are numbered from 0 to n−1) to avoid miscommunication in all cases of packets being dropped
			![[Pasted image 20240121214537.png]]

![[Pasted image 20240121215031.png]]
- Sequence number: A device initiating a TCP connection must choose a random initial sequence number, which is then incremented according to the number of transmitted bytes.
- Acknowledgment number – The receiving device maintains an acknowledgment number starting with zero. It increments this number according to the number of bytes received.

Defining the round trip time: Exponential Weighted Moving Average (EWMA)