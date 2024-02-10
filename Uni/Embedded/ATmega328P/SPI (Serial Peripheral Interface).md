# Introduction
![[Pasted image 20240209103654.png]]
- Only one master, multiple slaves
- Full duplex
- Chip Select - Select the peripheral device for communication. 
	- Each peripheral has a seperate chip select for independent communication
	- Daisy Chainining
		![[Pasted image 20240209132124.png]]
		This method is not possible with all SPI peripherals. Because, the slave must, moreover, only execute the command written to it on the rising edge of active-low CS. This means that as long as active-low CS remains low, the slave ignores the command and outputs it at DOUT on the following command-cycle.
- SCLK (Serial Clock) - Reading of the data may occur on the rising or falling edge of the clock pulse
- MOSI/SDO - Master Out Slave In
- MISO/SDI- Master In Slave Out.
- SPI devices have 8-bit shift registers to send and receive data
![[Pasted image 20240209104219.png]]
- Depending on the CPOL and CPHA there are 4 different SPI modes
![[Pasted image 20240209103456.png]]
![[Pasted image 20240209103540.png]]
## SPI in ATmega328P
### Registers
**SPCR: SPI Control Register**
![[Pasted image 20240209104357.png]]
- SPIE: SPI Interrupt Enable bit
- SPE: SPI Enable bit
- DORD: Data Order bit
	- 1 = LSB out first
	- 0 = MSB out first
- MSTR: Master/Slave Select bit
	- 1 = Master mode
	- 0 = Slave mode
- CPOL: Clock Polarity Select bit
	- 1 = Idle at 1
	- 0 = Idle at 0
- CPHA: Clock Phase Select bit
	- 1 = Trailing Edge
	- 0 = Leading Edge
	![[Pasted image 20240209112057.png]]
- SPR0:1 - Clock Rate Select Bits

**SPSR: SPI Status Register**
![[Pasted image 20240209104634.png]]
- SPIF: SPI interrupt flag bit
	- Set when serial transfer is complete
- WCOL: Write Collision Flag bit
- SPI2X: Double SPI Speed bit

**SPDR: SPI Data Register**
![[Pasted image 20240209104742.png]]
Writing to SPDR initiates data transmission

The chip select in master can be used as a GPIO pin. If the DDR is input then it must be set to high for master operation. Otherwise SPI system recognizes this as another master selecting the MCU an clear the MSTR bit going into the slave mode
In slave mode the chip select is always configured as input

Code
Master
```c
/* SPI Initialize function */
void SPI_Init(){
  /* Make MOSI, SCK, SS as Output pin */
  DDRB |= (1<<MOSI)|(1<<SCK)|(1<<SS);
  /* MISO as Input pin */
  DDRB &= ~(1<<MISO);
  /* Set chip select to high on idle */
  PORTB |= (1<<SS);

  /* Enable SPI, set as Master, Clock rate = Fosc/16 */
  SPCR = (1<<SPE)|(1<<MSTR)|(1<<SPR0);
  /* Disable speed doubler */
  SPSR &= ~(1<<SPI2X);
}

/* SPI write data function */
void SPI_Write(char data){
  char flush_buffer;
  SPDR = data; 
  /* Write data to SPI data register */
  while(!(SPSR & (1<<SPIF)));
 /* Wait till transmission complete */
 /* Flush received data */
 flush_buffer = SPDR;
 /* Note: SPIF flag is cleared by first
 reading SPSR (with SPIF set) and then
 accessing SPDR hence flush buffer used here
 to access SPDR after SPSR read */
 /* SPIF can also be set by H/W in the ISR */
}

/* SPI read data function */
char SPI_Read(){
  /* Write a dummy value to SPDR of master to start transmission */
  SPDR = 0xFF;
  /* Wait till reception complete */
  while(!(SPSR & (1<<SPIF)));
  /* Return received data */
  return (SPDR);
}

int main(){
	SPI_Init();
	SPI_Write(...)
}

```

Slave
```c
/* SPI Initialize function */
void SPI_Init(){
  /* Make MOSI, SCK, SS as input pins */
  DDRB &= ~((1<<MOSI)|(1<<SCK)|(1<<SS));
  /* Make MISO pin as output pin */
  DDRB |= (1<<MISO);
  /* Enable SPI in slave mode */
  SPCR = (1<<SPE);
}

/* SPI write data function */
void SPI_Write(char data){
  char flush_buffer;
  SPDR = data; 
  /* Write data to SPI data register */
  while(!(SPSR & (1<<SPIF)));
 /* Wait till transmission complete */
 /* Flush received data */
 flush_buffer = SPDR;
 /* Note: SPIF flag is cleared by first
 reading SPSR (with SPIF set) and then
 accessing SPDR hence flush buffer used here
 to access SPDR after SPSR read */
 /* SPIF can also be set by H/W in the ISR */
}

/* SPI Receive data function */
char SPI_Receive(){
  /* Wait till reception complete */
  while(!(SPSR & (1<<SPIF)));
  /* Return received data */
  return (SPDR);
}

int main(){
	SPI_Init();
	char command = SPI_Receive();
	// ... do work
}
```
