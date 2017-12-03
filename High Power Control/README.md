# Lab 6: “High Power” Control
## Mathew Philippou and Michael Lonetto
In real world applications that require “high power”, the MSP430’s will not be capable of driving them directly and doing so would most likely result in damage to the boards. Instead, there are some interesting switching circuits using mosfets and relays that will allow us to drive loads that require high voltage or high current without worrying about damage to the boards.

## Switching
Switching is a commonly used technique in circuit design where some external phenomena, whether it be change in voltage, current, resistance, heat, light, etc., causes the switch to either open or close.
 
## Code
The only thing we needed the MSP430 to do was output a PWM signal for a square wave, and therefore, for any given board, the below code may be used with slight alterations.
```c
   TA0CCTL1 = OUTMOD_7;        
    TA0CTL = TASSEL_2 + MC_1 +TACLR ;
    TA0CCR0 = 1000;		   // Sets CCR0, Max Period
    TA0CCR1 = 500;                        // Sets CCR1 and therefore Duty Cycle at 50% (square wave)
    P1SEL0 |= BIT0;                         // TA0CCR1 output to P1.0
    P1SEL1 &= ~BIT0;                    // Configure P1.0 as select bit
```

## Mosfet Switch
An n-channel mosfet may be configured to act as a switch. N-channel enhancement-mode mosfets operate using a positive input voltage and have near infinite impedance. This makes NMOS’s capable of interfacing with anything that can produce a positive output. A positive gate voltage will turn the switch “on” (closed switch) and a zero gate voltage will turn the switch “off” (open switch). To further explain, in order to keep the mosfet on (switch open) the gate voltage must exceed a certain threshold. This threshold can be determined from the v-I transfer curves seen in Figure 1. When Vin (gate voltage) is equal to or greater than VDD(drain voltage), the point A moves along the line. When this happens, the drain current increases to it’s maximum, RDS (on channel resistance) reduces to near zero, and the NMOS acts as a closed switch. 
 ![alt text]( https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/High_Power_Control/Mosfet%20Switching/blah.gif)

**Figure 1:** Mosfet Characteristics Curve

When Vin (gate voltage) is less than VDD (drain voltage), the point A moves along the line towards B. When this happens, RDS (on channel resistance) goes very high, the drain current reduces to zero, and the NMOS acts as an open switch. 

Therefore, if the gate voltage of the mosfet was attached to a PWM that switched from *ON* to *OFF*, the mosfet will act as a solid state switch. This effect was tested at ranging frequencies from 1 Hz, 100 kHz, 500 kHz, 3 MHz, and 10 MHz and the results may be seen in the Figures below. The circuit built for testing may be seen in Figure 2.


 ![alt text]( https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/High_Power_Control/Mosfet%20Switching/High_Power_Control_Mosfet_Tina_Schematic.JPG)

**Figure 2:** Schematic of Mosfet Switching Circuit

**Note :** The title of every photo is below the photo, I apoligize for the akward spacing

 ![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/High_Power_Control/Mosfet%20Switching/1Hz.png)

**Figure 3:** 1 Hz Square Wave

 ![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/High_Power_Control/Mosfet%20Switching/100KHz1.png)

**Figure 4:** 100 kHz Square Wave


 ![alt text]( https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/High_Power_Control/Mosfet%20Switching/500KHz2.png)

**Figure 5:** 500 kHz Square Wave


 ![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/High_Power_Control/Mosfet%20Switching/3MHz3.png)

**Figure 6:** 3 MHz Square Wave


 ![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/High_Power_Control/Mosfet%20Switching/10MHz4.png)

**Figure 7:** 10 MHz Square Wave

## Possible Mosfet Switching Circuit to Control Lamp
![alt text]( https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/High_Power_Control/Mosfet%20Switching/Possible%20mosfet%20circuit%20(the%20other%20one%20is%20the%20relay%20and%20mosfet%20combined).JPG)



## Relay Switch
Relays are electromechanical devices that use an electromagnet to operate a pair of movable contacts from an open position to a closed position.  As can be seen in Figure 8, the relay has a coil within it that has the advantage of requiring a small amount of power to operate. However, the relay, similar to the mosfet switching circuit, is capable of driving loads that require a large amount of power such as heaters, lamps or motors. Electro-mechanical relays come in a host of shapes, sizes, and internal circuit designs and have a wide variety of uses. Electrical relays can be used to control low power electronic or computer type circuits to switch high currents or voltages *ON* (closed switch) to *OFF* (open switch), however, a relay still needs some form of relay switch circuit to power it. While many kinds of these relay switch circuits exist, the one we will be focusing on today is the N-Channel mosfet relay switch circuit; a schematic of which may be seen in Figure 8.
 ![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/High_Power_Control/Relay%20Switching/High_Power_Control_Relay_Schematic.JPG)

**Figure 8:** Schematic of Relay Switching Circuit – Connected in Common-     Source Configuration.

Mosfet relay switching and BJT relay switching are very similar aside from some key differences. Fist, mosfets, as opposed to BJT’s, are voltage operated devices, meaning as the gate is electrically isolated from the Drain – Source channel, the input impedances are high, so the gate current of the mosfet is zero. Further, as voltage is applied to the gate, the conductive channel (that is initially *OFF* (open switch)) gradually increases in conductive width. The mosfet *ENHANCES* the channel as the gate voltage increases, explaining why we are using an Enhancement NMOS.
Due to the positive voltage on the gate terminal switching the mosfet *ON* and *OFF* it is ideal to use in combination with the relay in a switching circuit. Given an input voltage of zero, the value of VGS is not sufficient enough to increase the conductive channel width and the transistor is *OFF*. When VGS is increased above the mosfets lower threshold, VT, the conductive channel width increases and current flows allowing the relay coil to operate.  Below is the results of testing the circuit built in Figure 8.
 ![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/High_Power_Control/Relay%20Switching/IMG-4949%20(2).PNG)

**Figure 9:** Oscilloscope Probed on the Output of the Relay Circuit at low frequencies.
 ![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/High_Power_Control/Relay%20Switching/IMG-4950%20(2).PNG)

**Figure 10:** Oscilloscope Probed on the Output of the Relay Circuit at high frequencies.

## Possible NMOS and Relay Combined Circuit to Control Flywheel
![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/High_Power_Control/Mosfet%20Switching/Possible%20mosfet%20switching%20circuit.JPG)

