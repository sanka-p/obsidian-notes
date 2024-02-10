A 2 line bus
- SDA (Serial Data)
- SCL (Serial Clock)
- Open drain connection
![[Pasted image 20240209083840.png]]
 Atmel chips have a slew rate limiter on output driver of SDA and SCL. The slew rate control helps to limit the rate of change of the voltage (rise or fall) on these lines.
 Fast voltage changes can lead to signal integrity issues such as overshoot, undershoot, or ringing
![[Pasted image 20240209083904.png]]

I2C transmission occurs in messages
- Start bit
- 8 data bits
- ACK/NACK bit

![[Pasted image 20240209090935.png]]

Transmitted data bit is 1 if SDA is high between the SCL pulse (rising and falling edge)
Transmitted data bit is 0 if SDA is low between the SCL pulse (rising and falling edge)
SDA transitions when SCL is low (except during START condition)
![[Pasted image 20240209091228.png]]
![[Pasted image 20240209091243.png]]
![[Pasted image 20240209091340.png]]
![[Pasted image 20240209091401.png]]
If the bit remains high here it is known as a NACK
In addition to the start condition at the beginning of each message, the I2C protocol also defines a so-called repeated start condition. This allows the master to send a new message without releasing the bus, ensuring that the operation is not interrupted4. This is particularly useful in multi-master systems where there is a need to send a command and then read back an answer right away
![[Pasted image 20240209091439.png]]
![[Pasted image 20240209091508.png]]

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

Can have multiples slaves or multiples masters. But only one master can initiate communication. (Bi directional + Half duplex) (Arbitration detection unit)

Identify different devices uniquely using an 7 bit address. Bus supports 128 devices

Address 0x00 is the general call address. It is used to address all the other mcs on the bus. During a general call slaves can only read not write.

## I2C in ATmega328P
### Block Diagram

### Registers
**TWBR: TWI Bit Rate Register**
Determines the SCL frequency
![[Pasted image 20240209094452.png]]
**TWCR: TWI Control Register**
![[Pasted image 20240209094542.png]]
- TWINT (TWI Interrupt) - This bit gets set whenever TWI completes its current event (like start, stop, transmit, receive, etc)
- TWEA (TWI enable acknowledgment bit) - This is TWI acknowledgment enable bit, it is set in receiver mode to generate acknowledgment and cleared in transmit mode.
- TWSTA (TWI START condition bit) - The master device set this bit to generate START condition by monitoring free bus status to take control over the TWI bus.
- TWSTO (TWI STOP condition bit) - The master device set this bit to generate STOP condition to leave control over the TWI bus.
-  TWWC (TWI write collision) - This bit gets set when writing to the TWDR register before the current transmission not complete i.e. TWINT is low.
- TWEN (TWI enable bit) - This bit set to enables the TWI interface in the device and takes control over the I/O pins.
- TWIE (TWI interrupt enable) - This bit is used to enable TWI to interrupt routine while the I-bit of SREG is set as long as the TWINT flag is high.
**TWSR: TWI Status Register**
![[Pasted image 20240209094923.png]]
- TWS7:3 (TWI status bits)
- TWPS0:1 (TWI Pre-scaler bits)
**TWDR: TWI Data Register**
TWDR contains data to be transmitted or received. It’s not writable while TWI is in process of shifting a byte. The data remains stable as long as TWINT is set.
![[Pasted image 20240209095028.png]]
**TWAR: TWI Address Register**
 TWAR register contains the address of the TWI unit in slave mode.
 ![[Pasted image 20240209095127.png]]
 - TWGCE (TWI general call enable bit) - TWI general call enable bit when set it enables recognition of general call over the TWI bus

```c
/* Define bit rate */
#define BITRATE(TWSR ((F_CPU/SCL_CLK)-16)/(2*pow(4,(TWSR ((1<<TWPS0)| (1<<TWPS1)))))
void I2C_Init() /* I2C initialize function */
{
TWBR = BITRATE(TWSR=0x00); /* Get bit rate register value by formula */
}

uint8_t I2C_Start(charwrite_address)/* I2C start function */
{
uint8_t status;
 /* Declare variable */
TWCR=(1<<TWSTA)|(1<<TWEN)|(1<<TWINT); /* Enable TWI, generate START */
while(!(TWCR&(1<<TWINT))); /* Wait until TWI finish its current job */
status=TWSR&0xF8;
 /* Read TWI status register */
if(status!=0x08)
 /* Check weather START transmitted or not? */
return 0;
 /* Return 0 to indicate start condition fail */
TWDR=write_address;
 /* Write SLA+W in TWI data register */
 TWCR=(1<<TWEN)|(1<<TWINT); /* Enable TWI & clear interrupt flag */
while(!(TWCR&(1<<TWINT))); /* Wait until TWI finish its current job */
status=TWSR&0xF8;
 /* Read TWI status register */
if(status==0x18)
 /* Check for SLA+W transmitted &ack received */
return 1;
 /* Return 1 to indicate ack received */
if(status==0x20)
 /* Check for SLA+W transmitted &nack received */
return 2;
 /* Return 2 to indicate nack received */
else
return3;
 /* Else return 3 to indicate SLA+W failed */
}
```
REPEATED START condition issued to write and read during the same tranmission
```c
uint8_t I2C_Repeated_Start(charread_address) /* I2C repeated start function */
{
uint8_t status;
 /* Declare variable */
TWCR=(1<<TWSTA)|(1<<TWEN)|(1<<TWINT);/* Enable TWI, generate start */
while(!(TWCR&(1<<TWINT))); /* Wait until TWI finish its current job */
status=TWSR&0xF8;
 /* Read TWI status register */
if(status!=0x10)
 /* Check for repeated start transmitted */
return 0;
 /* Return 0 for repeated start condition fail */
TWDR=read_address;
 /* Write SLA+R in TWI data register */
TWCR=(1<<TWEN)|(1<<TWINT); /* Enable TWI and clear interrupt flag */
while(!(TWCR&(1<<TWINT))); /* Wait until TWI finish its current job */
status=TWSR&0xF8;
 /* Read TWI status register */
if(status==0x40)
 /* Check for SLA+R transmitted &ack received */
return 1;
 /* Return 1 to indicate ack received */
if(status==0x48)
 /* Check for SLA+R transmitted &nack received */
return 2;
 /* Return 2 to indicate nack received */
else
return 3;
 /* Else return 3 to indicate SLA+W failed */
}```

WRITE
```c
uint8_t I2C_Write(chardata)
 /* I2C write function */
{
uint8_tstatus;
 /* Declare variable */
TWDR=data;
 /* Copy data in TWI data register */
TWCR=(1<<TWEN)|(1<<TWINT); /* Enable TWI and clear interrupt flag */
while(!(TWCR&(1<<TWINT))); /* Wait until TWI finish its current job */
status=TWSR&0xF8;
 /* Read TWI status register */
if(status==0x28)
 /* Check for data transmitted &ack received */
return 0;
 /* Return 0 to indicate ack received */
if(status==0x30)
 /* Check for data transmitted &nack received */
return 1;
 /* Return 1 to indicate nack received */
else
return 2;
 /* Else return 2 for data transmission failure */
}
```

READ
after repetead start condition
```c
char I2C_Read_Ack()
 /* I2C read ack function */
{
TWCR=(1<<TWEN)|(1<<TWINT)|(1<<TWEA); /* Enable TWI, generation of ack */
while(!(TWCR&(1<<TWINT))); /* Wait until TWI finish its current job */
returnTWDR;
 /* Return received data */
}
```
This function read last needed data byte available on the SDA line but does not
returns an acknowledgment of it. It used to indicate slave that master don’t want
next data and want to stop communication.
```c
char I2C_Read_Nack()
 /* I2C read nack function */
{
TWCR=(1<<TWEN)|(1<<TWINT); /* Enable TWI and clear interrupt flag */
while(!(TWCR&(1<<TWINT))); /* Wait until TWI finish its current job */
return TWDR;
 /* Return received data */
}
```

STOP
```c
void I2C_Stop()
 /* I2C stop function */
{
TWCR=(1<<TWSTO)|(1<<TWINT)|(1<<TWEN);/* Enable TWI, generate stop */
while(TWCR&(1<<TWSTO));
 /* Wait until stop condition execution */
}
```