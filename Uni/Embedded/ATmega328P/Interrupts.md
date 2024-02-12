- Polling - Microcontroller (MC) continuously monitors the status of the device
- Interupt - Device notifies the MC. Then the MC stops whatever it is doing and serves the device
- Advantages of interrupts over polling
	- Assign priorities to devices. This cannot be done in polling because MC checks all devices in a round-robin fashion
	- Ignore requests (masking)
	- Saves time polling for devices that do not require services
- Interrupt Service Routine (ISR) / Interrupt Handler
	- MC runs the ISR when an interrupt is invoked
	- Each interrupt has a fixed location in memory that holds the address of its ISR (Interrupt Vector Table). When the memory allocated is not enough, a JUMP instruction is placed in the vector table to point to the ISR
- Steps in executing an interrupt
	1. Finishes the instruction it is currently executing and saves PC to stack
	2. Jumps to ISR using vector table here the global interrupt enable flag will be reset so interrupt nesting will not happen
	3. Executes until the last instruction of ISR which is RETI (return from interrupt)
	4. On RETI, pop PC from top of stack (Care must be taken when using the stack inside the ISR i.e., the number of pushes and pops must be equal)
- Sources of Interrupts
	- At least two interrupts (overflow and compare match) for each timer.
	- External hardware interrupt pins (PD2, PD3, PB2) corresponding to (INT0, INT1, INT2)
	- USART
	- SPI
	- ADC
- Enabling/Disabling Interrupts
	- Upon reset all interrupts are disabled
	- Enable D7 bit of SREG (Status Register) to activate interrupts globally using sei() function. 
	- This exists so we can disable all interrupts during a critical task easily using cli().
	- After enabling interrupts globally, enable each individual interrupt flags from the I/O registers holding the interrupt enable bits

### External Interrupts
| Interrupt  | Activation  | Default (Upon Reset) |
|---|---|---|
| INT0  | Level/Edge  | Low-Level |
| INT1  | Level/Edge  | Low-Level |
| INT2  | Edge  | Falling |

- Note: Consider using a push button to trigger an external interrupt, if the button is still pressed by the time RETI is executed, the interrupt is initiated again. Therefore if we want the ISR to be executed once, make the interrupt edge triggered
- In an edge triggered interrupt INTx, the interrupt flag INTFx becomes HIGH
- The pulse must last at least 1 instruction cycle to ensure that the transition is seen by the microcontroller
- Vector Table
	- Lower the vector no. higher the priority
	- CPU runs instruction at the given program address (fixed memory location)
![[Pasted image 20231218093256.png]]
```c
#include <avr/io.h> 
#include <avr/interrupt.h> 

int main(){ 
	// enable external interrupt 
	EIMSK |= (1 << INT0); 
	// INT1 defined in avr/io.h 
	// interrupt trigger as falling edge
	EICRA |= (1 << ISC01); 
	EICRA &= ~(1 << ISC00); 
	sei(); 
	while(1) ; 
} 
// ISR Macro 
ISR(INT0_vect){ 
	// do work 
}
```

Programmed IO vs Interrupt Driven IO
 PIO is a less efficient method of communication because it requires the microcontroller to
actively poll the input and output devices to check their status and control their behavior. However, it
can be useful in certain cases such as working with simple and low-speed devices, devices without
interrupt capability, and systems with limited memory resources.

Describe the required set of conditions to enable nested interrupt service routines to be executed. [08
marks]
In•
•
•
•
•
order for nested interrupt service routines (ISRs) to be executed, the following conditions must be met:
Hardware support: The microcontroller must have the capability to handle nested interrupts. This means
that the microcontroller must have enough interrupt priority levels to allow multiple interrupts to be
executed simultaneously.
Software support: The operating system or firmware of the microcontroller must also support nested
interrupts. This means that the system must have the ability to save and restore the context of the current
ISR before executing the nested ISR.
Interrupt priority levels: The interrupts that are to be nested must have different priority levels. Interrupts
with higher priority should be serviced first so that the lower priority interrupts can be enabled after the
higher priority interrupt is serviced.
Interrupt masking: The system should have a way to mask lower-priority interrupts during execution of
higher-priority interrupts. This is to prevent lower-priority interrupts from interrupting the execution of
higher-priority interrupts.
Interrupt-safe code: The ISRs must be written in a way that does not corrupt the execution of other ISRs.
This means that the ISRs must be designed to be as short and simple as possible and should avoid using
shared resources or performing operations that may cause conflicts with other ISRs.
•
Interrupt disabled state: Before entering the nested ISR, the interrupts should be disabled so that the
higher priority ISR is not interrupted before it is completed.