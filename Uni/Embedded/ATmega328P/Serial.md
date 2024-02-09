Serial communication requires
 - Byte is converted to serial bits using a parallel in serial out shift register
 - Bits are received and converted to a byte using a serial in parallel out shift register
 - Modems: modulate/demodulate serial bits to/from audio tones

Protocols:
- Slow
	- RS232 (moderate distance)
	- RS485 (long distance)
- Fast (Short Distance)
	- USB
	- I2C
	- FireWire

Two modes of serial communication:
 - Synchronous
	 - Clocks of sender and receiver are synchronised
	 - A block of characters enclosed by synchronising bytes is sent at a time
	 - faster transfer and less overhead
	 - examples: SPI
 - Asynchronous
	 - Clocks of sender and receiver are not synchronised
	 - one character is sent at a time, enclosed between start bit and one or two stop bits with or without parity
	 - examples: Rs232, USART

Data Transfer Rate / Baud Rate
 - Stated in bps
