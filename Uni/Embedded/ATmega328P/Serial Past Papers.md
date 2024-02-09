### E2023-FEB
Q3
(a) On certain occasions, we need to use asynchronous protocols. Explain such an occasion with an example while referring to the features of the protocol.

- Different sensors have different processing and update rates
- No need of a seperate line to carry the clock signal
- The USART is meant to do all of the “heavy lifting” serial communication during periods of “high” energy consumption. When the microcontroller is asleep and in a low power mode, though, the UART peripheral can handle low speed communications while offering a reduced energy footprint.

(c) The Baud rate of a USART in ATmega16 microcontroller is 1200bps. The intended data transmission rate is 1000bps. If the protocol is asynchronous, explain the possible use of remaining 200bps
- Start bit
- Parity bits
- Stop bits

### E2022-SEP

Q3
(a). Give an example of framing in synchronous and asynchronous communication. Explain their
attributes with relevant requirements.

(b). Explain the need to have a data register in USART unit of an embedded system.
(c). State and explain the requirement to have a baud rate register in a synchronous communication
unit.

### E2019-OCT
(a).  In synchronous serial communication a block of characters enclosed by synchronizing bytes is
sent at a time instead of a single character as in asynchronous serial communication. Explain the
reason.
 (b).  Serial Peripheral Interface (SPI) is an interface specification developed by Motorola, used for
high data rate and short distance communication. Do you expect the clocks of the sender and
receiver to be synchronized? Elaborate on this. Give three devices using SPI.
 (c). 

### E2017-JUN
(a). Despite of data framing requirement serial communication channels are preferred over parallel for certain scenarios. Justify the statement with reasons
(b). Write down the control word pattern required to initialize the serial port with the following characterisitics. The control register, baud rate register information of a USART is provided
- Asynchronous mode
- Even parity
- 2 stop bits
- 6 data bits
- Double transmission speed
- Disable multiprocessor mode
- Enable Tx and Rx
- Baud rate 1200 bps assuming a clock rate of 2MHz
facilitate the transmission and reception with interrupt routines
Name the registers in your answer based on the information given to you
(c) Assume you are writing an interrupt routing to receive a character. Explain the details you need to include in the interrupt routing to ensure error free reception