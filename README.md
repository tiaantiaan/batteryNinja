# BatteryNinja!
A thing that monitors your UPS battery health and helps it not explode.

![BatteryNinja!](images/batteryninjathumb.jpg)

## Demo video
[![Demo video]](https://www.youtube.com/watch?v=UMRNiSmD5Bg "Demo")

## Components
- NodeMCU or ESP8622 module
- ds18b20 Temperature Sensor
- Resistors for a resistor divider with a 1:10 ratio

## Wiring Diagram
                                                   +---------------------------------------------------------To inverter/
                                                   |                                                            charger
                                                   |                                                  +------
                                                   |                                                  |
                                                   |                                                  |
    +----------------------------------------------+                     +----------+                 +----------+
    |                                              |                     |          |                 |          |
    |                                             |                     |          |                 |          |
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

## Voltage selection
This was designed to measure the voltage from two 12V batteries in series (24V). But it can be easily adapted to meaure the voltage from 12V battery by setting the `numberOfBatteries` parameter in the code to `1`.

The voltage divider

## Programming the device
Program the device using the Arduino IDE. Make sure that you have the ESP8266 libraries installed (search the web for a tutorial).

## The enclosure
The case is based based on this model https://www.thingiverse.com/thing:541876
It was resized and a rectangular hole was made into the face plate eto accommodate the OLED display.

## Future Improvements
The logic that determines if the battery is discharging is buggy especially when the battery voltage fluctuates a lot between measurements.
