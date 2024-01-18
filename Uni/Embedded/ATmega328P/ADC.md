Transducer/Sensor - Converts a physical quantity to electrical signal
ADC - Converts analog/continous electrical signals to binary/discrete values
 - Resolution - Expressed by the bitsize (n) of output. This value is fixed at design.
 - Step Size - It is smallest change in the signal that is sensed by the ADC.
	 - $Step\;Size = \frac{V_{ref}}{2^n}$
 - Conversion time - Time it takes the ADC to convert the analog input to a digital values. It depends on
	 - Clock source
	 - Data conversion method
	 - Technology used in fabrication: TTL or MOS
 - Digital Data Output
	 - $D_{out} = \frac{V_{in}}{stepsize}$
 - An on chip ADC reduces extra connections needed for and external ADC module
 - For multiple analog inputs we can multiplex and use the same ADC
	 - Due to multiple input channels a start conversion (SC) signal is needed
	 - Once the conversion is done the end-of-conversion (EOC) signal notifies the CPU to read the output

ATmega328P ADC Features and Registers
- 10 bit successive approximation ADC
- ADMUX Register
	![[Pasted image 20231231153907.png]]
	- Choose between 8 analog input channels from MUX3:0 bits
	- 3 options for Vref: AVCC (Analog Vcc), Internal 1.1V ref or external AREF
		- Set option using REFS1:0 pins
		- ADC Connection Diagram
			![[Pasted image 20231231153415.png]]
			- Decoupling AVCC from Vcc for more stability
			- Make Vref more stable by connecting a capacitor between Vref and GND
- Output held in two 8 bit registers ADCL (ADC low) and ADCH (ADC high)
	- User can choose which 6 bits (upper/low goes unused) using the ADLAR bit of ADMUX Register
		![[Pasted image 20231231154454.png]]
- ADCSRA Register
	![[Pasted image 20231231155332.png]]
	ADEN - Enable/Disable ADC. Can be used mid conversion as well
	ADSC - Start Conversion bit
	ADATE - ADC Auto Triggerring. Automatically trigger ADC when input changes
	ADIF - Set when ADC conversion is complete and data registers are updated
	ADPS2:0 - ADC Prescaler Select Bits - Division factor between XTAL frequency and input clock to ADC
		For the AVR, ADC requires an input clock frequency less than 200 kHz for maximum accuracy

Acquisition Time - After an ADC channel is selected, the ADC allows some time for the sample-and-hold capacitor to charge fully to the input voltage level present at the channel. In the AVR, the first conversion takes 25 ADC clock cycles in order to initialize the analog circuitry and pass the sample and hold time. Each consecutive conversion takes 13 ADC clock cycles

Steps for using ADC
1. Set ADC channel pin as an input pin
2. Turn on the ADC module (it is disabled upon power-on to save power)
3. Select conversion speed using ADPS2:0
4. Select voltage reference and ADC input channel
5. Start the conversion usign ADSC bit
6. Wait for conversion to finish by polling the ADIF bit
7. Read ADCL and ADCH registers
8. Repeat from step 5 or 4 depending on the channel or references