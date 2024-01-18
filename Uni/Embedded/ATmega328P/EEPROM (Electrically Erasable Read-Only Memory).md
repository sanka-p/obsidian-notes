- Non volatile
- Erased using electrical pulses
	- Limited number of re-writes (ONLY WRITE WHEN ABSOLUTELY NECESSARY )
- 3 Registers
	- EEPROM Address Register (EEARH, EEARL)
		- Initial value is undefined
		- Used to access the EEPROM byte space
	- EEPROM Data Register
		- Contains the data to be written/read to the EEPROM
	- EEPROM Control Register
		- Programming Mode Bits - Defines which programming action that will be triggered when writing EEPE 
		- EEPROM Ready Interrupt Enable - Generates a constant interrupt when EEPE is cleared
		- EEPROM Master Write Enable
			- After setting this setting EEPE within four clock cycles will write data to the EEPROM
		- EEPROM Write Enable
		- EEPROM Read Enable

To write to the EEPROM
1. Wait until previous write is finished (EEPE becomes zero)
2. Write new EEPROM address
3. Write new EEPROM data
4. Write a logical one to EEMPE bit while writing a zero to EEPE in EECR
5. Within four clock cycles after setting EEMPE, write a logical one to EEPE

To read from the EEPROM
1. Wait until the previous EEPROM write is finished
2. Write new EEPROM address
3. Start EEEPROM reading by writing to EERE
4. Read the EEPROM data from data register