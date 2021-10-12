# BatteryNinja!
A thing that monitors your UPS battery health and helps it not explode.

![BatteryNinja!](images/batteryninjathumb.jpg)

## Demo video
[![Demo video]](https://www.youtube.com/watch?v=UMRNiSmD5Bg "Demo")

## Components
- NodeMCU or ESP8622 module
- ds18b20 Temperature Sensor
- Resistor for a resistor divider with a 1:10 ratio

## Programming the device
Program the device using the Arduino IDE. Make sure that you have the ESP8266 libraries installed (search the web for a tutorial).


## Future Improvements
The logic that determines if the battery is discharging is buggy especially when the battery voltage fluctuates a lot between measurements.
