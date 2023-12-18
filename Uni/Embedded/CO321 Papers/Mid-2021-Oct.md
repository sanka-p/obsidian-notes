![[Pasted image 20231217195515.png]]
```c
// M. S. Peeris (E/19/275)

#include <avr/io.h>
#include <avr/interrupt.h>
#include <util/delay.h>

#define LED_COUNT 5
const int leds[LED_COUNT] = {PB1, PB2, PB3, PB4, PB5}; // A -> E LED pins
int curr_led = 0; // LED that is currently lit

int continue_seq = 1; // flag to indicate if sequene is paused

void init_leds();
void reset_leds();
void light_led();
void init_timer0();
void init_timer2();
void reset_timer2();
void init_int1();
void fade_led_c();

int main(){
  init_leds();
  init_timer0();
  init_int1();
  sei();
  light_led(); // initial call to light first LED
  while(1){
    if(continue_seq) continue;
    // fade LED C if paused on it
    if (!continue_seq && curr_led==3){
      init_timer2();
      while(!continue_seq) fade_led_c();
      reset_timer2();
    };
  }
  return 0;
}

// set LED pins as output
void init_leds(){
  for(int i=0; i<LED_COUNT; i++)
    DDRB |= (1<<leds[i]);
}

// turn off all LEDs
void reset_leds(){
  for(int i=0; i<LED_COUNT; i++)
    PORTB &= ~(1<<leds[i]);
}

// turn on current LED in sequence
void light_led(){
  // ascending = A -> D
  // not ascending = D -> A 
  static int is_ascending; // current direction of sequence

  reset_leds();

  PORTB |= (1<<leds[curr_led]); // turn on current LED
  
  // switch direction at edge LED
  if(curr_led == 0 || curr_led == 4)
    is_ascending ^= 1;

  // set next LED to light up in next func call
  curr_led = is_ascending ? ++curr_led : --curr_led;
}

// timer 0 will create a 8 ms delay
void init_timer0(){
  // Tclock = (N=1024)/(Fxtal=16MHz) = 64us
  // increments = (Tdelay=8ms)/(Tclock=64us) = 125
  // initial val = (1+255) - increments=(125) = 131
  TCNT0 = 131;
  TCCR0A = 0;
  TCCR0B |= (1<<CS02) | (1<<CS00); // 1024 bit prescaler
  TIMSK0 |= (1<<TOIE0); // enable timer 0 overflow interrupt
}

ISR(TIMER0_OVF_vect){
  static int overflow_count = 0;
  overflow_count++;
  // 8 * 25 = 200 ms delay
  if(overflow_count>25){
    light_led(); // light next led in sequence
    overflow_count = 0;
  }
  // restart timer
  TCNT0 = 131;
}

// ext int 1 listens to pushbutton
void init_int1(){
  DDRD &= ~(1<<PD3); // set interrupt pin as input
  EIMSK |= (1<<INT1); // enable external interrupt 1
  EICRA |= (1<<ISC11); // trigger interrupt on falling edge
}

// resume/pause sequence on pushbutton press
ISR(INT1_vect){
  continue_seq ^= 1;
  if (continue_seq)
    TCCR0B |= (1<<CS02) | (1<<CS00); // restart timer
  else
    TCCR0B = 0x00; // pause timer on push button

  _delay_ms(200); // handle debounce;
}

// timer 2 will fade LED C
void init_timer2(){
  TCCR2A |= (1<<WGM21) | (1<<WGM20); // fast PWM Mode
  TCCR2A |= (1<<COM2A1); // non inverting mode
  TCCR2B |= (1<<CS21) | (1<<CS20); // 64 factor prescaler (976.56Hz freq)
}

// stop fading
void reset_timer2(){
  TCCR2A = 0;
  TCCR2B = 0;
}

void fade_led_c(){
  // increase brightness by increasing duty cycle 0 - 100%
  for(int i=0; i<256; i++){
    // stop fading if sequene is resumed
    if (continue_seq) return;
    OCR2A = i;
    _delay_ms(5);
  }
  // decrease brightness by decreasing duty cycle
  for(int i=255; i>-1; i--){
    if (continue_seq) return;
    OCR2A = i;
    _delay_ms(5);
  }
}

```