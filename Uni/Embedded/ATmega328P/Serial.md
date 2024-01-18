Serial communication requires
 - Byte is converted to serial bits using a parallel in serial out shift register
 - Bits are received and converted to a byte using a serial in parallel out shift register
 - Modems: modulate/demodulate serial bits to/from audio tones

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

### RS232
- 25 pin or 9 pin connectors
- Baud rate upto 20Kbps, cable length upto 15m
- Data Terminal Equipment (DTE) - computer, terminals, printers
- Data communication Equipment (DCE) - modem
- ![[Pasted image 20240117003344.png]]
- ![[Pasted image 20240117003507.png]]
- RS232 uses different voltage levels than TTL, this conversion is done by the MAX232 chip
- ![[Pasted image 20240117003455.png]]

https://www.virtual-null-modem.com/help/tid_null-modem_schematics.htm
**Null Modem Configuration**
- To connect two DTE directly
- Different ways to connect
	- No handshake
		![[Pasted image 20240117100750.png]]
		- No hardware flow control can be implemented only with software flow control using the XOFF and XON characters
		- If either of the two devices checks the DSR or CD inputs. These signals normally define the ability of the other side to communicate. As they are not connected, their signal level will never go high
		- The same holds for the RTS/CTS handshaking sequence. If the software on both sides is well structured, the RTS output is set high and then a waiting cycle is started until a ready signal is received on the CTS line. This causes the software to hang because no physical connection is present to either CTS line to make this possible.
		- Used for
			- data-only traffic on the cross connected RxD/TxD lines
			- when communicating with devices which do not have modem control signals like electronic measuring equipment
	- Loop Back Handshaking
			![[Pasted image 20240117102657.png]]
	- Partial Handhaking
			![[Pasted image 20240117102828.png]]
	- Full Handshaking
			![[Pasted image 20240117102848.png]]
### USART (Universal Synchronous Asynchronous Receiver/Transmitter)

- Baud rates from 960bps to 57.6kbps (common - 19200, 9600, 4800, 2400, 1200 bps)
- character size: 5 to 9 bits
- 1 start bit
- 1 or 2 stop bits
- optional parity bit

For serial usart
- 3 groups of registers
	- Baud Rate Registers (UBRRH, UBRRL)
		- ![[Pasted image 20240117004411.png]]
	- Control Status Registers (UCSRA, UCSRB, UCSRC)
	- Data Register (UDR)

Steps for serial usart
1. Initialising port
```c

void  usart_init(){

    /* set UBRR value = 16000000/(16 * 9600) - 1*/
    unsigned char ubrr = 103;

    /*set baud rate*/
    UBRR0H = (unsigned char) (ubrr >> 8);
    UBRR0L = (unsigned char) (ubrr);

    /*Enable receiver and transmitter*/
    UCSR0B = (1 << RXEN0) | (1 << TXEN0);

    /*Set frame format: 8 data, 1 stop bit*/
    UCSR0C = (1 << UCSZ01) | (3 << UCSZ00);
}
```
2. Sending a character
```c
void usart_send(unsigned char data){

    /* Wait for empty transmit buffer */
    while(!(UCSR0A & (1 << UDRE0) ));

    /* Put the data into buffer, sends the  data */
    UDR0 = data;
}
```
3. Receiving a character
```c
unsigned char usart_receive(){
    /* Wait for the data to be received */
    while(!(UCSR0A & (1 << RXC0)));

    /* Get and return received data from buffer */
    return UDR0;
}
```

### Setting Standard Streams in C for USART
```c
#include <stdio.h>

int main(){
	usart_init();
	stdout = fdevopen(usart_send, NULL);
	stdin = fdevopen(NULL, usart_receive);

	// use printf and scanf as usual
}
```