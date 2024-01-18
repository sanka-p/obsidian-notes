- Timer n is continously compared to OCRnA/B and when a match occurs it can
	- trigger an output compare interrupt
	- change output compare pins OCnA/B
		- toggled
		- set to 1
		- cleared to 0
		- no change
- Control analog variable using rectangular digital waveform
- Duty Cycle - Proportion of time from the period that the pulse is high ( “on” time/pulse width)
- Modulating Frequency = 1/(2 * “on” time)
- Common range for modulating frequency: 1kHz - 200kHz
- ATmega328P supports two PWM modes
	- Fast PWM - Timer goes from 0 to TOP and repeats
		- Fast PWM is faster than phase correct PWM because fast PWM performs a single slope
		![[Pasted image 20240116232602.png]]
	- Phase Correct PWM - Timer counts up and down between 0 and TOP
		![[Pasted image 20240116232737.png]]
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

### Fast PWM
```c
#include <avr/io.h>
#include <avr/interrupt.h>
#include <util/delay.h>

int main(){
  DDRD |= (1<<PD6);

  TCCR0A |= (1<<WGM01) | (1<<WGM00); // fast PWM mode
  TCCR0A |= (1<<COM0A1); // non-inverting mode
  TCCR0B |= (1<<CS01) | (1<<CS00); // 64 factor prescaler (976.56Hz freq)
  
  while(1) {
    for(int i=0; i<256; i++){
      OCR0A = i;
      _delay_ms(10);
    }
    for(int i=255; i>-1; i--){
      OCR0A = i;
      _delay_ms(10);
    }
  }

  return 0;
}
```

### Variable Frequency PWM
```c
#include <avr/io.h>
#include <avr/interrupt.h>

#define TIM1_PRESCALER 8
#define DUTY_CYCLE 10

// Use timer0 to create a 1ms delay
void delay_1ms(){
	TCNT0 = 6; // 250 * (64 / 16Mhz) = 1ms
	TCCR0A = 0;
	TCCR0B |= (1<<CS01) | (1<<CS00); // 64 prescaler
	while(!(TIFR0 & (1<<TOV0))); // Wait until TOV0 flag is set
	TCCR0B = 0; // Turn off timer
	TIFR0 |= (1<<TOV0); // Clear TOV0 flag
}

// Create a delay of any amount(val) of miliseconds
void delay(int val){
	for(int i=0; i<val; i++)
		delay_1ms();
}

void tone(uint16_t freq, int dur) {
	// Set PB1 to be an output (Pin 9 Arduino UNO)
	DDRB |= (1 << PB1);
	// Set non-inverting mode
	TCCR1A |= (1 << COM1A1);
	// Set fast PWM Mode with
	// TOP value in ICR1
	// OCR1A updated at bottom
	// TOV1 set on TOP
	TCCR1A |= (1 << WGM11);
	TCCR1B |= (1 << WGM12);
	TCCR1B |= (1 << WGM13);
	// Set prescaler to 8 and starts PWM
	TCCR1B |= (1 << CS11);
	// Set PWM frequency/top value
	ICR1 = (F_CPU / (TIM1_PRESCALER*freq)) - 1;
	// Set duty cycle
	OCR1A = ICR1 / (100 / DUTY_CYCLE);
	delay(dur);
	// Turn off PWM Signal
	TCCR1A = 0x00;
	TCCR1B = 0x00;  
}

int main() {
	// Play A4 (note = 440Hz) for 1s
	tone(440, 1000);
}
```
