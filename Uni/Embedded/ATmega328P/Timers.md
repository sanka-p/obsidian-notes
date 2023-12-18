### Introduction
- Two types of clock sources for the timer to tick:
	- Internal/Timer - Crystal Oscillator
	- External/Counter
		- Feed external event using a I/O pin (Input Capture)
		- Counts as a preset event (rising edge, falling edge,level  change) occurs
- ATmega328P has 3 timers: 
	- Timer0
		- 8 bit
		- 2 independent output compare units
		- PWM support
		- 10-bit prescaler: 8, 64, 256, and 1024
	- Timer1
		- 16 bit
		- 2 independent output compare units
		- PWM support
		- Input Capture
		- 10-bit prescaler: 8, 64, 256, and 1024
	- Timer2
		- 8 bit
		- 2 independent output compare units
		- PWM support
		- 10-bit prescaler: 8, 64, 256, and 1024
		- Asynchronous timer - this means Timer2 can continue to count even when the microcontroller is in sleep mode
	- Watchdog timer
- Each timer has the following registers (n - 0, 1, 2)  located in the I/O register memory
	- **TCNTn** - Timer/Counter Register
		- Contains 0 upon reset
		- Counts up with each pulse/event
		- Data can be both read/written to this register
	- **TCCRnA , TCCRnB**- Timer/Counter Control Register A and B
		![[Pasted image 20231217182223.png]]
		![[Pasted image 20231217182248.png]]
		- COMnA1:COMnA0 - Control the Output Compare Pin behavior (OCnA)
			- Note that the Data Direction Register (DDR) bit corresponding to the OC0A pin must be set in order to enable the output driver
		- WGMn0, WGMn1 - (Waveform Generation Modes) Timer Modes
		- CS02:CS00 - Clock Source
			- 000 - Counter is stopped
			- 001 - clk no prescaling
			- 010 - clk / 8
			- 011 - clk / 64
			- 100 - clk / 256
			- 101 - clk / 1024
			- 110 - External clock source on T0 pin. Clock on falling edge
			- 111 - External clock source on T0 pin. Clock on rising edge
		- FOCnA/B - Force Compare Match
			- Forcing compare match will not set the OCFnA/B Flag or reload/clear the timer, but the OCnA/B pin will be updated as if a real compare match had occurred
	- **OCRnA/B** (Output Compare Register A/B)
		- Value of OCRnA/B is continuously compared with the TCNTn register
		- A match is used for
			- Output compare interrupt
			- Generate waveform output (PWM)
				- Using the Output Compare Output OCOnA/B
	- **TIMSKn** - Timer/Counter Interrupt Mask Register
		![[Pasted image 20231217182819.png]]
		- OCIEnA - Enable/Disable the Timer/Counter Compare Match A interrupt
		- OCIEnB - Enable/Disable the Timer/Counter Compare Match B interrupt
		- TOIE0 - Enable/Disable Timer/Counter0 Overflow interrupt
	- **TIFRn** - Timer/Counter Interrupt Flag Register
		![[Pasted image 20231217182837.png]]
		- TOVn (Timer Overflow) Flag
			- Cleared either in ISR by hardware or manually cleared by writing a logic one to the flag
- Additionally Timer 1 has the following registers
	- 

### Delay Using Timers
- Delay depends on
	- Crystal Oscillators Frequency $F_{XTAL}$​
	- Prescale Factor $N$
	- Compiler which affects overhead due to instructions
- To create a delay function
	- Load initial counter value to TCNTn
		- $T_{clock} = \frac{N}{F_{XTAL}}$
		- $Increments = \frac{T_{delay}}{T_{clock}}​$
		- $Counter_{InitialVal} = (1+255)-Increments​$
	- Set values in TCCRnA/B
	- Continously monitor TOVn and break out of loop when it is high
	- Stop timer by disconnecting the clock source
	- Clear TOVn flag (add 1 to it)
	- Repeat
- Generic Delay Function
```c
#include <avr/io.h>
#include <avr/interrupt.h>

// use timer0 to create a 1ms delay
void delay_1ms(){
  TCNT0 = 6; // 250 * (64 / 16Mhz) = 1ms
  TCCR0A = 0;
  TCCR0B |= (1<<CS01) | (1<<CS00); // 64 prescaler

  while(!(TIFR0 & (1<<TOV0))); // wait until TOV0 flag is set

  TCCR0B = 0; // turn off timer
  
  TIFR0 |= (1<<TOV0); // clear TOV0 flag
}

// create a delay of any amount(val) of miliseconds
void delay(int val){
  for(int i=0; i<val; i++)
    delay_1ms();
}

int main(){
  // blink in the built in led in arduino
  DDRB |= (1<<PB5);
  while(1){
    PORTB ^= (1<<PB5);
    delay(1000);
  }

  return 0;
}

```
- Custom Delay Function
```c
#include <avr/io.h>
#include <avr/interrupt.h>

void init_timer0(){
  TCNT0 = 6; // 250 * 1024 / 16Mhz = 16ms
  TCCR0A = 0;
  TCCR0B = (1<<CS02) | (1<<CS00); // 1024 prescaler
  TIMSK0 |= (1 << TOIE0);
}

ISR(TIMER0_OVF_vect){
  static overflow_count = 0;
  overflow_count++;
  // 16ms * 125 = 2s
  if (overflow_count > 125){
	overflow_count = 0;
	PORTB ^= (1<<PB5); // do work
  }
  TCNT0 = 6;
}

int main(){
  init_timer0();
  sei();

  DDRB |= (1<<PB5);

  while(1);

  return 0;
}
```

### Measuring Elapsed Time using Timers
```c
#include <avr/io.h> 
#include <avr/interrupt.h> 
#include <inttypes.h> 
uint32_t int n; 

ISR(TIMER1_OVF_vect){ 
	n++; 
} 

int main(){ 
	n = 0; 
	TCTNT0 = 0; 
	TCCR0A = 0x00; 
	TCCR0B = 0x01; 
	TIMSK0 |= (1 << TOIE0); 
	sei(); 
	// do work
	// ... 
	uint32_t elapsed_time = n * 256 + (uint32_t) TCTNT0; 
	// disable interrupt subsystem globally
	cli();  
	return 0; }
```

### Input Capture - Measuring the period of a Square Wave
```c
#include <avr/io.h> 
#include <avr/interrupt.h> 
#include <inttypes.h> 

uint16_t period; 

ISR(TIMER1_CAPT_vect){ 
	period = ICR1; 
	TCNT1 = 0; 
} 

int main(){ 
	TCNT1 = 0;
	TCCR1A = 0x00; // normal mode 
	TCCR1B = 0x01; // no prescaler, rising edge 
	TIMSK1 |= (1 << TOIE1); 
	sei(); 
	while(1) ; 
	return 0; 
}
```

