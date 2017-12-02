# Lab 6: Open Loop Systems
## Mathew  Philippou and Michael Lonetto

## Fan Control
A fan was used to maintain the temperature of a voltage regulator through a PWM signal of varying duty cycles. The output a PWM signal from our MSP430 connects to the fan in order to control the speed. A circuit was build, that may be seen in the figure below, to connect the pwm output of the MSP430 to the fan.
![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/Open_Loop_Systems/first%20pic.jpg)
**Figure 1:** P1.0 connects to the circuit. The Yellow wire was to be the input to power the fan. The white wire was our PWM signal from Pin. 

## Voltage Regulator and Temperature Reading
The temperature sensor we used represents every degree Celsius in intervals of 10 mV. We tested the temperature sensor by overloading a 5V regulator with 20 Volts so that it would dissipate heat for the 
temperature sensor to detect. The ADC value is determined from the output voltage on the temperature sensor shown in the figure below. Using the formula below in our code we are able to convert the ADC 
value into a temperature and display it on the MSP430FR6989 LCD display.
```c
adc_value = ((ADC12MEM0*3.35)/4096)*100; // converts to temp value
```
![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/Open_Loop_Systems/Second%20Pic.jpg)
**Figure 2:** The circuit with the red wire being powered by 20 Volts and the blue wire going into Pin 1.3 of our ADC.

## System Modeling
The temperature of the voltage regulator was recorded at ranging pwm signals. The table and graph of this may be seen below.
![alt text](https://github.com/RU09342/lab-6taking-control-over-your-embedded-life-jordan-and-pippen/blob/master/Pictures/Open_Loop_Systems/temp%20vs%20duty%20chart.JPG)

We were able to model a system of equations from this table in order to maintain a given temperature of the voltage regulator, by using different PWM signals to power the fan. Below is the function of our code which determines what PWM to set the fan to in order to maintain a certain temperature. 

```c
void tempToPWM(int temp)
{
    int pwm;
    if(temp < 10)
    {
        pwm = -0.56*temp + 33.62;
    }
    else if(temp >= 10 & temp < 20)
    {
        pwm = -3.03*temp + 136.67;
    }
    else if(temp >= 20 & temp < 30)
    {
        pwm = -6.25*temp + 260.63;
    }
    else if(temp >= 30 & temp <= 100)
    {
        pwm = -38.89*temp + 1465;
    }

    TA0CCR1 = pwm;

}
```
