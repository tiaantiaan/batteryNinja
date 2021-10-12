# BatteryNinja!
A thing that monitors your UPS battery health and helps it not explode.

![BatteryNinja!](images/batteryninjathumb.jpg)

## Alpha version
This project is very much in an alpha state, so there are bugs and the code looks quite rough. Please consider contributing improvements by submitting a PR.

## Demo video
![[Demo video]](https://www.youtube.com/watch?v=UMRNiSmD5Bg "Demo")

## Components
- NodeMCU (or raw ESP8622 module but you'll have to change the resistor values to match the 1V maximum of the ADC)
- ds18b20 Texas Instruments Temperature Sensor
- 4k7 Ohm resistor for the temperature sensor
- Resistors for a resistor divider with a 1:10 ratio (see below)

### Resistor divider
The 10 bit ADC (Analogue to digital converter) of the NodeMCU has a maximum voltage of 3.3V (a raw ESP8622 has a 1V maximum). In other words, it reads voltages between 0 and 3.3V and turns it into a number between 0 and 1024. This number is then converted into voltage in the code. The limitation of the ADC does, however, mean that the battery voltage has to be scaled down to a maximum of 3.3V. We use a resistor divider for this (https://ohmslawcalculator.com/voltage-divider-calculator). The maximum voltage of the battery is not 24V because a single lead-acid battery charges at around 13.8V (thus 27.6V for two). Assuming a maximum battery value of 33V (just to be safe and to make the math easier), the resistor divider has to scale the voltage down by a factor of 10. To achieve this, I chose a value of 1kOhm for R1 and 9kOhm for R2. I actually used nine 1kOhm resistors because I had them around and you don't get a 9kOhm resistor value in most standard resistor ranges (see this thread for other solutions https://forum.arduino.cc/t/a-perfect-10-1-voltage-divider-using-only-one-resistor-value/475575). 

To get better accuracy from the resistor divider, make sure to use low tolerance resistors. E.g. the blue 1% tolerance resistors instead of the typical ones that have 5% tolerance.

## Wiring Diagram
```
                                                   +---------------------------------------------------------To inverter/
                                                   |                                                            charger
                                                   |                                                  +------
                                                   |                                                  |
                                                   |                                                  |
    +----------------------------------------------+                     +----------+                 +----------+
    |                                              |                     |          |                 |          |
    |                                              |                     |          |                 |          |
    |                                             +++                   +++        +++               +++         |
    |             +-------------------+        +--+-+-------------------+-+--+  +--+-+---------------+-+----+    |
    |             |                   |        |                             |  |                           |    |
    |             |                   |        |                             |  |                           |    |
+---+---+         |                   |        |                             |  |                           |    |
|       |         |                   |        |                             |  |                           |    |
|       |         |                   |        |             12V             |  |            12V            |    |
|       |         |                   |        |                             |  |                           |    |
| R2*   |         |     NodeMCU       |        |      Lead Acid Battery      |  |      Lead Acid Battery    |    |
| 9kOhm |         |                   |        |                             |  |                           |    |
|       |         |    (ESP8266)      |        |                             |  |                           |    |
|       |         |                   |        +-----------------------------+  +---------------------------+    |
|       |         |                   |                                                                          |
+---+---+         |                3V +----------+------------+                                                  |
    |             |                   |          |            |                                                  |
    +-------------+ A0                |       +--+-+          |                                                  |
    |             |                   |       |    |     +----+------+                                           |
    |             |                   |       |4k7 |     |   Vdd     |                                           |
    |             |                   |       |Ohm |     |           |                                           |
+---+---+         |                   |       |    |     |           |                                           |
|       |         |                   |       |    |     |  ds18b20  |                                           |
|       |         |                   |       +-+--+     |           |                                           |
|       |         |                   |         |        |           |                                           |
| R1*   |         |                D4 +---------+--------+DATA    GND|                                           |
| 1kOhm |         |     +--------+    |                  +---------+-+                                           |
|       |         |     |        |    |                            |                                             |
|       |         |     | 5V USB |    |                            |                                             |
+---+---+         +-----+--------+----+                            |                                             |
    |                                                              |                                             |
    |                                                              |                                             |
    +--------------------------------------------------------------+---------------------------------------------+
```
Schematic made with http://asciiflow.com/

## Voltage selection
This was designed to measure the voltage from two 12V batteries in series (24V). But it can be easily adapted to measure the voltage from the 12V battery by setting the `numberOfBatteries` parameter in the code to `1`.

When using one battery, the resistor values used in the schematic will still work, but you can get twice the resolution from the ADC when you use a resistor divider with a 5:1 voltage ratio.

## Programming the device
Program the device using the Arduino IDE. Make sure that you have the ESP8266 libraries installed (search the web for a tutorial).

## The enclosure
The case is based on this model https://www.thingiverse.com/thing:541876
It was resized and a rectangular hole was made into the faceplate to accommodate the OLED display.

## Future Improvements
The logic that determines if the battery is discharging needs improvement. It is buggy especially when the battery voltage fluctuates a lot between measurements.
