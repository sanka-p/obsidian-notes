A 2 line bus
- SDA (Serial Data)
- SCL (Serial Clock)
- Open drain connection
![[Pasted image 20240209083840.png]]
![[Pasted image 20240209083904.png]]

Two modes
- Master Mode
- Slave Mode

Different modes

| Mode | Speed |
| ---- | ---- |
| Standard Mode | 100 kbps |
| Fast Mode | 400 kbps |
| Fast Mode Plus | 1 Mbps |
| High Speed Mode | 3.4 Mbps |
| Ultra-Fast Mode | 5 Mbps |

The choice of resistor values depend on the bus capacitance and the necessary rise and fall times. Very high speeds might need active pullups

Can have multiples slaves or multiples masters. But only one master can initiate communication. (Bi directional + Half duplex)