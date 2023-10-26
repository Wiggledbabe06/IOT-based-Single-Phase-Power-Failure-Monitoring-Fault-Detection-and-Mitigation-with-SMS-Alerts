# IOT-based-Single-Phase-Power-Failure-Monitoring-Fault-Detection-and-Mitigation-with-SMS-Alerts.

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

<img src ="https://github.com/Wiggledbabe06/IOT-based-Single-Phase-Power-Failure-Monitoring-Fault-Detection-and-Mitigation-with-SMS-Alerts/assets/98098708/c1941856-5905-4c30-ae7a-f7f34b0440dd" width="500" height = "300">

</p>
Single phase voltage sensor modules like ZMPT101B can measure up to 250v and convert that waveform in a form that can be read through Analog pins of a microcontroller like Arduino.

The output signal of ZMPT101B is a waveform of analog values from 0 to 1023. Since the amplitude is variable, the analog signal needs to be calibrated. To calibrate the output, we need a multimeter that can measure AC voltage (RMS) for reference. The maximum Root Mean Square value ZMPT101B can measure is 250 Vac but it can measure amplitude as high as 353.55 Vac peak.

The Output value will fluctuate between 0 to 1023 (0V to 5V). It will send a analog signal at half the supply voltage (ex-2.5V) , which is close to 512 , when no voltage is detected. Modules can have different deviation errors; some might output slightly more or slightly less value than 512 in 0 voltage conditions. We will have to adjust the offset using no voltage as a reference.

Averaging and Initial offset must be done to greatly reduce the phenomenon of electrical noises and fake readings.

Our code is designed to sample the input at a rate of 1000 samples in a second , which means a sample of the input will be taken at every 1 milli second. Each sample value is being squared and once the 1000 sample values are collected, the average value from 1000 samples will be square-rooted to calculate the Root Mean Square (RMS) voltage which will be displaced on the OLED display, due to this averaging , the fluctuation is greatly reduce.
<p align="center">

<img src ="https://github.com/Wiggledbabe06/IOT-based-Single-Phase-Power-Failure-Monitoring-Fault-Detection-and-Mitigation-with-SMS-Alerts/assets/98098708/18c26129-2631-40cb-8946-07e404502d49" width="500" height = "300">

</p>
  
**To measure Current:-**
For measuring current, we are using the ACS712 Current Sensor Module. It utilizes hall-effect phenomenon in which voltage is produced from the movement of current within the region of magnetic field. The applied current is directly proportional to the voltage produced by hall effect.
<p align="center">

<img src ="https://github.com/Wiggledbabe06/IOT-based-Single-Phase-Power-Failure-Monitoring-Fault-Detection-and-Mitigation-with-SMS-Alerts/assets/98098708/b11c983a-6aa4-4ef5-85a7-2cebaba214d0" width="500" height = "300">

</p>

