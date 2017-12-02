# Lab 6: Precision Control
## Mathew Philippou and Michael Lonetto

## PWM Part 2
![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/Precision%20Control/LowPassFilter.JPG)

**Figure 1:** Schematic of Integrator Circuit Built on Breadboard

![alt text]( https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/Precision%20Control/square1.png)

**Figure 2:** PWM signal Going Through Integrator

![alt text]( https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/Precision%20Control/10khz%20tri1.png)

**Figure 3:** 10 kHz Square Wave Throguh Integrator

![alt text]( https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/Precision%20Control/40khz%20tri1.png)

**Figure 4:** 20 kHz Square Wave Throguh Integrator

![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/Precision%20Control/100%20load1.png)

**Figure 5:** 60 kHz Square Wave Throguh Integrator

As seen in Figure 1, an Integrator circuit was built. A square wave outputted by the PWM of the MSP430 would
pass through this Integrator and would become a triangle wave at higher frequencies. A Integrator attenuagtes
signals above certain frequency threshold. We expect the output of the Integrator to look something like this:
![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/Precision%20Control/whattheyshouldlooklike.JPG)
So, at low frequencies, the signal should remain a square wave and this can be seen in Figure 2. At medium frequencies,
the signal should begin to attenuate higher frequencies and resemble dull spikes rather than squares. This can be seen in Figure 3.
Finally, at higher frequenices the signal reduces in amplitude and fully attenuates higher frequencies and will resemble a triangle function. This can be seen in 5.

## Loading Effects
![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/Precision%20Control/60khz%20tri.png)

**Figure 6:** 100 ohm Load 

![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/Precision%20Control/500k%20load2.png)

**Figure 7:** 500 kohm Load

We used the same circuit in Figure 1 to analyze the loading effect by placing a resistor on the output. However instead of using a PWM
signal from the MSP430, we used the waveform generator. The input of the circuit was a 1V peak-to-
peak square wave at a frequency of 40kHz. In addition we added a load to the output of our Integrator to
observe what would happen. We used loads of 100 Ohms and 500 kOhms.
As we can see from Figure 6, with a 100 ohm load there is virtually no difference in the triangle
signal, compared with the same signal and no load, seen in Figure 5. But as the load increased the signal began to turn
into a sine wave. We noticed it very clearly with a resistor value of 500 kOhms as shown above in Figure 7.
In addition we can see that as the load increased the peak-to- peak voltage also decreased.

## R2R DAC
![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/Precision%20Control/Resistor%20circuit.JPG)

**Figure 8:** R2R DAC Circuit Schematic

An R2R DAC circuit was designed to convert the digital output from the MSP430 to an
analog DC voltage signal.
The R2R DAC works by splitting the incoming voltage by a power of 2. The resistor that
is farthest from the output will be controlled by the least significant bit, and vice versa for the
resistor that is closest corresponding to the most significant bit. Each resistor contributes the total
voltage divided by 2 to the power of whatever bits place the resistor represents. Our input of
01101100 should be 108/256 of the total voltage, (4/256+8/256+32/256+64/256 = 108/256).
Therefore with a 3.3V input our theoretical value with that number input would be 1.392 V. 
Our result of 1.457 V is relatively close
