![[Pasted image 20240211120405.png]]
Data Acquisition occurs in 5 stages
1. Transducer - Converts physical variables into measurable electrical signals
2. Signal Conditioning - Removes external noise and smoothens the analog signal
3. Multiplexer - Analog inputs can come from multiple channels but the ADC can perform conversions only on one channel at a time
4. Amplifier - Used to amplify the input signal if it is too small
5. ADC - Converts the analog signal to a digital signal

Transducer/Sensor - Converts a physical quantity to electrical signal
Two steps in A to D conversion
1. Sampling - extract signal usually at regularly spaced time instants (sampling period)
2. Quantisation - quantize the samples to discrete levels
![[Pasted image 20240211121817.png]]

Sampling theorem
	An analogue signal x(f) with frequencies of no more than Fmax (here we say maximum frequency because the wave can be a summation of multiple frequencies) can be reconstructed exactly from its samples if the sampling rate satisfies
	$$F_s \ge 2.F_{max}$$
	If maximum frequency of the signal is Fmax, the sampling rate should be atleast
	Nyquist rate = 2 x Fmax
	If the sampling rate if Fs, the maximum frequency in the signal must not exceed
	Nyquist Frequency = 0.5Fs

Consider an n bit ADC
$$d=floor(\frac{V_{in}-V{min}}{step\;size})$$

Quantisation error is the average difference between the analogue input and the quantised value. The quantisation error of an ideal ADC is half of the step size

FLASH ADC
![[Pasted image 20240211122429.png]]
Fast because all comparators work parallelly
Bust cost a lot dues to complex circuitry

SUCCESSIVE APPROXIMATION ADC
![[Pasted image 20240211122734.png]]
![[Pasted image 20240211122945.png]]
Sequence for binary search
example 3 bit ADC
start from MSB
if comparator outputs 1, leave as it is and go to next bit
if comparator outputs 0, set to 0 and go to next bit
repeat for all bits upto LSB

Errors in ADC
Gain Error
Offset Error
INL (Integral Nonlinearity)
![[Pasted image 20240211220249.png]]
Difference between actual and measured value
DNL (Differential Nonlinearity)
![[Pasted image 20240211215600.png]]
Variable step sizes. In an ideal ADC, DNL should be 1LSB
The maximum DNL

To reduce the error, the quantized signal is shifted 0.5LSB to the left
![[Pasted image 20240211215300.png]]
![[Pasted image 20240211215209.png]]

ADC - Converts analog/continous electrical signals to binary/discrete values
 - Resolution - Expressed by the bitsize (n) of output. This value is fixed at design.
 - Step Size - It is smallest change in the signal that is sensed by the ADC.
	 - $Step\;Size = \frac{V_{ref}-V{min}}{2^n}$
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

Which one to choose left or right align??
1. **Right Alignment:**
    
    - If you need all 10 bits of the conversion result, you would typically use right alignment.
    - Reading both low and high values in a right-aligned format allows you to capture the entire 10-bit precision of the conversion result.
2. **Left Alignment:**
    
    - If you only need 8 bits of precision, left alignment can be advantageous.
    - By using left alignment and taking only the high value (most significant bits), you effectively reduce the precision to 8 bits, which might be sufficient for certain applications.
3. **Advantages of Left Alignment:**
    
    - Left alignment can provide an advantage in terms of memory usage. By taking only the most significant byte of the register (high value), you use less memory compared to storing the entire 10-bit result.
    - This can be particularly useful when you have RAM constraints and need to store a large number of samples. Using only the necessary bits helps conserve memory.
4. **Advantages of Right Alignment:**
    
    - Right-justified values can be used directly without the scaling that a left-justified value might require.
    - If you need the full precision of the A/D conversion result and don't have memory constraints, right alignment allows you to work directly with the entire 10-bit value.

If the measured voltages are close to 0 then right align will be better but if the values are closer to 5v then left will be better. But generally the left align will have the closes accuracy to the value

Example:
Write a c program to configure the ADC as follows:
source = ADC1
AVcc = 5v
Left Align
Disable Auto trigger
Disable Interrupt
Prescaler = 2
```c
void setupADC(){
	ADCMUX |= (1<<ADLAR);
	ADCMUX |= (1<<REFS0);
	ADSCRA |= (1<<ADEN);
	ADCSRA &= ~(1<<ADIE);
	ADCSRA &= ~(1<<ADSC);
}

int main(){
	// start conversion
	ADCSRA |= (1<<ADSC);
	// wait until the conversion completes
	while (ADCSRA & (1 << ADSC)) ;
	result = ADCH;
}
```