- Control analog variable using rectangular digital waveform
- Duty Cycle - Proportion of time from the period that the pulse is high ( “on” time/pulse width)
- Modulating Frequency = 1/(2 * “on” time)
- Common range for modulating frequency: 1kHz - 200kHz
- ATmega328P supports two PWM modes
	- Fast PWM
	- Phase Correct PWM
- Fast PWM
	- When TCNTn <= OCRnA/B, OCOn is high
	- When TCNTn > OCRnA/B, OCOn is low
- Steps to create a PWM signal
	- Set the PWM (OCnA/B) pin as output
	- Set the top value related to the duty cycle in OCRnA/OCRnB register
		- $Duty\;Cycle = \frac{OCRnA/B+1}{256} *100$
	- Set control bits
		- Waveform Generation Pins (WGMn2:0) as 011
		- Set non inverting mode COMnA/B1:0 as 10
			- Here the OCnA/B output is cleared (set to 0) each time a compare match occurs, and it is set to 1 when the timer reaches the BOTTOM (0)
	- Select prescaler value according to the modulating frequency
		- $T_{timer} = N* T_{xtal}$
		- $T_{gen,wave}=T_{timer}*256$
		- $T_{gen,wave}=N*256*T_{xtal}$
		- $F_{gen,wave}=\frac{F_{xtal}}{N*256}$

### Variable Frequency PWM
#TODO
