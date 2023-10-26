# IOT based Single Phase Power Failure Monitoring Fault Detection and Mitigation with SMS Alerts.

**Project Objective:** To develop a IoT-based system for single-phase power supply to detect faults & mitigate damage by breaking the circuit in realtime to enhance grid reliability and protect equipment.To develop a IoT-based system for single-phase power supply to detect faults & mitigate damage by breaking the circuit in realtime to enhance grid reliability and protect equipment.

## Tools & technologies used:

**Programming Language :** Embedded C.

**Tools & technologies used:** Embedded C, Node-red.

**Hardware used:** Arduino UNO, OLED Display, ZMPT101B Voltage Sensor, ACS712B Current Sensor, 5V Relay,
DC-DC Buck Converter, Over-current Protection AC Current Detection Sensor, SIM800L GSM GPRS Module.

## Design Approach and Details

Our approach to solving the major common problem of electrical faults and prevent damage is to measure Voltage , Current and Frequency in real time using different methods and sending that data to Arduino microcontroller for analyzing it and operating a Relay to either break the circuit or allowing the current to flow based on the analysis and preset tolerances.

**To measure Voltage :-**
We can measure voltage using the analog pins of Arduino. In Arduino Uno , there are a total of 6 analog pins, we will use one of the pins to measure AC voltage. The analog input pins will map voltages between 0 and 5V into integer values between 0 and 1023 with resolution of 4.9mV per unit (5.00V/1023 units). Changing the voltage polarity can damage the pins.

<p align="center">

<img src ="https://github.com/Wiggledbabe06/IOT-based-Single-Phase-Power-Failure-Monitoring-Fault-Detection-and-Mitigation-with-SMS-Alerts/assets/98098708/c1941856-5905-4c30-ae7a-f7f34b0440dd" width="400" height = "250">

</p>
Single phase voltage sensor modules like ZMPT101B can measure up to 250v and convert that waveform in a form that can be read through Analog pins of a microcontroller like Arduino.

The output signal of ZMPT101B is a waveform of analog values from 0 to 1023. Since the amplitude is variable, the analog signal needs to be calibrated. To calibrate the output, we need a multimeter that can measure AC voltage (RMS) for reference. The maximum Root Mean Square value ZMPT101B can measure is 250 Vac but it can measure amplitude as high as 353.55 Vac peak.

The Output value will fluctuate between 0 to 1023 (0V to 5V). It will send a analog signal at half the supply voltage (ex-2.5V) , which is close to 512 , when no voltage is detected. Modules can have different deviation errors; some might output slightly more or slightly less value than 512 in 0 voltage conditions. We will have to adjust the offset using no voltage as a reference.

Averaging and Initial offset must be done to greatly reduce the phenomenon of electrical noises and fake readings.

Our code is designed to sample the input at a rate of 1000 samples in a second , which means a sample of the input will be taken at every 1 milli second. Each sample value is being squared and once the 1000 sample values are collected, the average value from 1000 samples will be square-rooted to calculate the Root Mean Square (RMS) voltage which will be displaced on the OLED display, due to this averaging , the fluctuation is greatly reduce.
<p align="center">

<img src ="https://github.com/Wiggledbabe06/IOT-based-Single-Phase-Power-Failure-Monitoring-Fault-Detection-and-Mitigation-with-SMS-Alerts/assets/98098708/18c26129-2631-40cb-8946-07e404502d49" width="400" height = "250">

</p>
  
**To measure Current:-**
For measuring current, we are using the ACS712 Current Sensor Module. It utilizes hall-effect phenomenon in which voltage is produced from the movement of current within the region of magnetic field. The applied current is directly proportional to the voltage produced by hall effect.
<p align="center">

<img src ="https://github.com/Wiggledbabe06/IOT-based-Single-Phase-Power-Failure-Monitoring-Fault-Detection-and-Mitigation-with-SMS-Alerts/assets/98098708/b11c983a-6aa4-4ef5-85a7-2cebaba214d0" width="400" height = "250">

</p>

As you can see in the above image , when current flows it produces a magnetic field across the copper conductor path which is proportional to the amount of current passed.

The sensor can measure bidirectionally, As Arduino can only read positive integer values. We have to take zero reference point at half point of the total voltage range(0 to 5V) which is 2.5V.

When 5V is supplied to a 5V ACS712 module it provides an output at 2.5V +/- 0.925V at no load

ACS712 will produce an Analog Signal which will fluctuate from 0 to 1023 (0 to 5V). It’s Output will decrease by 185mV for every drop in ampere.

**To measure frequency**

Power line frequency is oscillation cycles of AC current in electric grid. In India the frequency is around 50Hz. In order to determine the AC frequency, we will be using the same ZMPT101B Ac voltage sensing module to create a waveform signal and then we will calculate the length and period of the waveform.

Arduino can measure AC voltage or AC current using its analog pins. Analog pins map input voltages between 0 and 5v to integer values from 0 to 1023 with a resolution of 4.9mV unit.

We are using the voltage sensor instead of current sensor because signal will be available even in no load condition (no current).

We can see the output of voltage sensor by typing the following code

<p align="center">

<img src ="https://github.com/Wiggledbabe06/IOT-based-Single-Phase-Power-Failure-Monitoring-Fault-Detection-and-Mitigation-with-SMS-Alerts/assets/98098708/a6d38790-3b6b-4c72-aefa-23c59767e9b8" width="400" height = "250">

</p>

It will generate an output as the following diagram below
<p align="center">

<img src ="https://github.com/Wiggledbabe06/IOT-based-Single-Phase-Power-Failure-Monitoring-Fault-Detection-and-Mitigation-with-SMS-Alerts/assets/98098708/5aecf233-f0e4-4eeb-a290-570b4a9c9211" width="400" height = "250">

</p>
First we will set the reference point at 0 , so that will make the magnitude fluctuate from -512 to 512 and the wave magnitude is not important because we are not measuring the AC voltage.

Next, we will set the counting time and quantity of sample. The counting quantity sample will start when the value will be larger than 0 which is the initial point of the wave. Now when the next sample will be taken for the averaging calculation, the last count will become the “previous count” of the next set of reading so it is not included in the next set of counting.

<p align="center">

<img src ="https://github.com/Wiggledbabe06/IOT-based-Single-Phase-Power-Failure-Monitoring-Fault-Detection-and-Mitigation-with-SMS-Alerts/assets/98098708/43709c3d-70ac-4858-be9b-c611ac138863" width="700" height = "250">

</p>

Total accumulated time of the set divided by the total no. of count ( quantity of the samples) will be the frequency value.  

**Conclusion**


Now we will use the current , voltage and frequency data accumulated from the sensors and set parameters according to the device we will be using this system on to automatically break the circuit to prevent any damage to device due to faults because of fluctuate of voltage , current of frequency.

## Methodology

### Schematic Diagram of the circuit

<p align="center">

<img src ="https://github.com/Wiggledbabe06/IOT-based-Single-Phase-Power-Failure-Monitoring-Fault-Detection-and-Mitigation-with-SMS-Alerts/assets/98098708/968ce3d7-5fcb-4a29-9e2b-bb3cd7c11528" width="700" height = "500">

</p>

### Connections

<div align="center">
  
**Arduino Uno**

|Serial no.|	Pin no. / Name	|Pin no. / Name|
|:-------------: | :-------------: | :------------:|
|1.|	Vin	|Positive terminal of 12V supply|
|2.|	GND	|Negative terminal of 12V supply|
|3.|	A1	|Output of ZMPT101B current sensor|
|4.|	A2	|Output of ACS712B current sensor|
|5.|	D2	|Rx of SIM800L GSM GPRs module|
|6.|	D3	|Tx of SIM800L GSM GPRS module|
|7.|	D7	|Signal In of relay|
|8.|	D13	|RES of OLED display|
|9.|	D12	|CS of OLED display|
|10.|	D11	|DC of OLED display|
|11.|	D10	|SCK of OLED display|
|12.|	D9	|SDA of OLED display|


**ZMPT101B**

|Serial no.	|Pin no. / Name|	Pin no. / Name|
|:-------------: | :-------------: | :------------:|
|1.|	Source V in|	In parallel with the Main Supply|
|2.|	Source V out|	In parallel with the Main Supply|
|3.|	5V|	5V|
|4.|GND|	GND|
|5.|	Signal Out	A1 of the Arduino|


**ACS712B**

|Serial no.|	Pin no. / Name|	Pin no. / Name|
|:-------------: | :-------------: | :------------:|
|1.|	Source V in|	In series with positive terminal of the main Supply|
|2.|	Source V out|	To relay|
|3.|	5V|	5V|
|4.|	GND	|GND|
|5.|	Signal Out|	A2 of the Arduino|


**OLED display**

|Serial no.	|Pin no. / Name	|Pin no. / Name|
|:-------------: | :-------------: | :------------:|
|1	|GND	|GND|
|2.|	VDD|	5V|
|3.|	SCK|	10|
|4.|	SDA|	9|
|5.|	REC|	13|
|6.	|DC|	11|
|7.	|CS	|12|


**5V Relay**

|Serial No.|	Pin no. / Name	 |Pin no. / Name|
|:-------------: | :-------------: | :------------:|
|1.|	Normally Open	|To load|
|2.|	Common	|From Overcurrent Protection AC current Detection Sensor|
|3.|	Normally Closed	|None|
|4.|	5V	|5V|
|5.|	GND	|GND|
|6.	|Signal In|	D7 of the Arduino|


**DC-DC buck converter**

|Serial No. |	Pin no. / Name |	Pin no. / Name |
|:-------------: | :-------------: | :------------:|
|1.|	V in + |	From 12 V + supply|
|2.|	V in - |	From 12V – supply |
|3.|	V out + |	To GSM VIN |
|4.|	V out -	|To GSM GND |


**Overcurrent Protection AC Current Detection Sensor**

|Serial No. |	Pin no. / Name |	Pin no. / Name|
|:-------------: | :-------------: | :------------:|
|1.	|Normally Open |	To load|
|2.	|Common	| From 5V|
|3.	|Normally Closed |	None|
|4.	|5V	| 5V|
|5.|	GND	|GND|



**SIM800L GSM GPRS Module**

| Serial No.     |	Pin no. / Name |	Pin no. / Name            |
|:-------------: | :-------------: | :------------:             |
|     1.         |	NET            |	Attached a Helical antenna|
|     2.	       |  VCC            |	5V                        |
|     3.	       |  RST            |	None                      |
|     4.         |	RXD            |	D2 of the Arduino         |
|     5.	       |  TXD	           | D3 of the Arduino          |
|     6.         |	GND	           | GND                        |

</div>
