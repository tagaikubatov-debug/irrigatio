⭐ Overview

This project is an autonomous soil-moisture–controlled watering system built using Arduino Uno, a soil moisture sensor, LCD display, MOSFET-controlled water pump, and external 6V power source.
The system continuously monitors soil moisture and automatically activates the water pump when the soil becomes dry.

📦 Features

Real-time soil moisture measurement

Automatic pump control via MOSFET (LR7843 module)

LCD 1602 (I2C) displaying live moisture percentages and pump state

Safe dual-power system (Arduino 5V logic + isolated 6V pump power)

Fully open-source hardware & code

🔧 Hardware Components

Arduino Uno

1602 LCD display with I2C backpack

Soil Moisture Sensor (AO output)

LR7843 MOSFET Module

6V water pump (powered by 4×AA batteries)

470 µF capacitor

Jumper wires

Breadboard

🛠️ Wiring Diagram / Schematic

(Insert photo or upload wiring diagram image here — JPEG/PNG/Draw.io export)

Connections summary:

Soil Sensor
Component	Arduino
AO	A0
VCC	5V
GND	GND
LCD 1602 I2C
Component	Arduino
SDA	A4
SCL	A5
VCC	5V
GND	GND
MOSFET Module (LR7843)
MOSFET Pin	Arduino
PWM	D9
VCC	5V
GND	GND
Pump Power

+6V Battery → MOSFET LOAD+

Pump − → MOSFET LOAD−

Pump + → +6V Battery

Battery GND → Arduino GND (shared ground)

