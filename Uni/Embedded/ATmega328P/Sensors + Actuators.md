Sensors (Input Transducers) convert
Physical to Electrical like temp or pressure
Electrical to Electrical like voltage, current

Actuators (Output Transducer)
Electrical to physical like sound/pressure, rotation, linear movement

## Sensors

Contact sensors
- Typically do not require power
- Can handle more current
- Tolerate power supply disturbances
- Easier to understand and diagnose
- Examples
	- Encoders - Machine motion to signals and data
	- Limit Switches - Detect physical contact (Detech if the fridge door is open for a light to turn on)
	- Safety Switches - Temper resistance switch
	- Reed switch


Non contact sensors
- No physical contact
- No moving parts to wear or break therefore less maintenance
- Operate faster
- Greater application flexibility
- Susceptible to energy radiated by other devices and processes
- Examples
	- Photoelectric
	- Inductive
	- Capacitive
	- Ultrasonic
	- Magnetic

Ideal Sensor properties
- Do not disturb the system it measures
- Do no absorb enery from the system it measures
- Act as an ideal V or I source (Zero Impedance)
- Linear output
- No offset
- can response to any stimulus no matter how small
- Does not add noise of its own
- Monotonic - sensitivity never changes the sign
- Only response to indended stimulus
- Does not need time to respond

Actucal sensors
- Need time to respond (response time)
- Hysterisis
	- Some time useful to avoid noise (Schmidt trigger)
- Cross talk or cross sensitivity
	- The output is not only depend on the stimulus
	- For example the thermistor needs V and we measure I. But the flowing I could heat up the thermistor
- Resolution - Smallest detectable change
- Accuracy - difference between the actual value and the measured value
- Precision - the repeatability or reproducibility of the measurement
- Dynamic range - ratio between the maximum and minimum signal

Why a zero offset is useful in the transfer function of a sensor:
1. Including a zero-offset parameter allows for calibration of the sensor. Calibration involves adjusting the sensor's output to match the expected output when the input is zero. This helps enhance the accuracy of the sensor's measurements across its entire range.
2. Sensors may experience drift or changes in their output over time due to environmental factors or aging. The zero-offset parameter helps compensate for this drift by allowing adjustments to be made to the baseline measurement, ensuring accurate readings even when the input stimulus is minimal

Passive sensors
Use own power
- Photovoltaic
![[Pasted image 20240211135020.png]]
- Thermovoltaic
	- Two different types of materials in contact (metal and a semiconductor)
	- Sensors
		- Thermocouples (seebeck effect)
	- Actuators (Peltier effect)
		- Heaters (Forward bias)
		- Coolers (reverse bias)
- Piezoelectric
	- Polarised material that develops voltage proportional to strain
	- Used as diplacement transducers
- Electromagnetic
	- Coils
	- Sensors
		- Field sensors
		- Motion sensors
	- Actuators
		- Motors

Model as a controlled source (a controlled source is a component that produces an output voltage or current that depends on a separate input voltage or current.)
• Source is a function of the stimulus
• Internal impedance is important
• (may be a function of the stimulus)
• Need to do impedance matching to maximize SNR
Impedance matching is a technique used in electronics to maximize the transfer of power between a source and a load. In the context of passive sensors, achieving impedance matching is crucial for optimizing the signal-to-noise ratio (SNR). SNR is a measure of the quality of the signal relative to background noise. Maximizing SNR ensures that the sensor's output signal is more discernible and reliable in the presence of noise or interference

Active sensors
Need external power to measure
- Variable resistance
	- Resistance temperature detectors
	- Magnetoresistance (Hall, GMR)
		![[Pasted image 20240211135653.png]]
	- Photoconductors
- Variable Reactance
	- Capacitance
	![[Pasted image 20240211135722.png]]
	- Inductance
	![[Pasted image 20240211135745.png]]

In sensors current ouput is better
Current is equal at all points regardless of voltage drops during transmission

## Actuators
Motors
- DC Motors
	- Permanent Magnet Type
		- Brushed DC Motor
			- Current flowing through armature generates a magnetic field. The permanent magnets torque the armature
			- Adding multiple poles allow the motor to smooth output torquean to start from any position
		- Brushless DC Motor
			- Advantages
				- Better Speed vs Torque
				- High Efficiency
				- Long operating life (no brushes)
				- Noise less
				- Reduce EMI
				- Cooling
- Stepper Motors
	- A stepper motor possesses the ability to move a specified number of revolutions or a fraction of a revolution in order to achieve a fixed and consistent angular movement
	- Advantages
		- High accuracy of motion
		- Cheaper and effective in open loop systems
		- Brushless construction
	- Disadvantages
		- Low torque capacity compare to DC motors
		- Limited speed
		- High Vibrational levels
		- Large errors and vibrations is a pulse is missed
- Servo Motors
	- Two types
		- Positional Control
			![[Pasted image 20240211140858.png]]
			Send an ordinary logic square wave to your servo at a specific wave length and the servo goes to a particular angle. The wavelength directly maps to servo angle
			![[Pasted image 20240211141127.png]]
		- Velocity Control - Instead of position sensor, hall sensors are used
- Linear Motors
	- MagLev

Pneumatics
Advantages
- Weight: Much lighter than motors (as long as several used)
- Simple
	- Much easier to mount than motors
	- Much simpler, easier, more durable than rack and pinion
- More rugged
	- Cylinders can be stalled indefinitely without damage
	- Resistant to impacts
Disadvantages
- All the way in or all the way out

Hydraulics
- Much simpler, easier, more durable than rack and pinion
- Can maintain constant force


Motor Drivers

Safety Concerns with Actuators
Never connect two devices
- without finding voltages
- maximum sink/source currents
- load current
Galvanic isolation is **used where two or more electric circuits must communicate, but their grounds may be at different potentials**. It is an effective method of breaking ground loops by preventing unwanted current from flowing between two units sharing a ground conductor.

