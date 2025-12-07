⭐ Overview

This project is an autonomous soil-moisture–controlled watering system built using Arduino Uno, a soil moisture sensor, LCD display, MOSFET-controlled water pump, and external 6V power source.
The system continuously monitors soil moisture and automatically activates the water pump when the soil becomes dry.

📦 Features

🚀 Features

🌡️ Real-time soil moisture monitoring

💧 Automatic pump activation when soil is dry

🖥️ LCD display for live status (moisture %, pump state)

🔋 Dual-power design: 5V logic + isolated 6V pump supply

🛡️ Safe MOSFET switching (LR7843 module)

🪛 Fully reproducible, open-source hardware & software

🛠️ Wiring Diagram / Schematic

<img width="1681" height="1183" alt="image" src="https://github.com/user-attachments/assets/4bcd4524-c485-4c73-af53-7e4163c69ec4" />


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

💻 Arduino Code

Full code stored here:
#include <Wire.h> 
#include <LiquidCrystal_I2C.h> // Библиотека Франка (Frank de Brabander)

// --- ПИНЫ ---
const int PIN_SENSOR = A0; // Красный датчик
const int PIN_PUMP = 9;    // Насос

// --- НАСТРОЙКИ ПОЛИВА (0-100%) ---
const int START_LEVEL = 30; // Включить, если влажность ниже 30%
const int STOP_LEVEL  = 70; // Выключить, если влажность выше 70%

// --- КАЛИБРОВКА (Для красного датчика) ---
// Сухой датчик = 0
// Полностью в воде = около 600 (зависит от глубины погружения)
const int VAL_DRY = 0;   
const int VAL_WET = 600; 

bool isPumping = false;

// Запускаем экран (адрес 0x27)
LiquidCrystal_I2C lcd(0x27, 16, 2); 

void setup() {
  Serial.begin(9600);
  
  pinMode(PIN_PUMP, OUTPUT);
  pinMode(PIN_SENSOR, INPUT);

  // Инициализация экрана
  lcd.init();
  lcd.backlight();
  
  lcd.setCursor(0, 0);
  lcd.print("RED SENSOR SYS");
  delay(2000);
  lcd.clear();
}

void loop() {
  // 1. Читаем данные (0...600)
  int raw = analogRead(PIN_SENSOR);

  // 2. Переводим в проценты
  int percent = map(raw, VAL_DRY, VAL_WET, 0, 100);
  percent = constrain(percent, 0, 100);

  // 3. Логика (Если сухо -> качаем)
  if (percent < START_LEVEL) {
    isPumping = true;
  } 
  else if (percent > STOP_LEVEL) {
    isPumping = false;
  }

  // 4. Управление Мотором
  if (isPumping) {
    digitalWrite(PIN_PUMP, HIGH);
  } else {
    digitalWrite(PIN_PUMP, LOW);
  }

  // 5. Вывод на экран
  // Строка 1: Статус
  lcd.setCursor(0, 0);
  if (isPumping) {
    lcd.print("Watering...     ");
  } else {
    lcd.print("Soil Good       "); 
  }

  // Строка 2: Проценты
  lcd.setCursor(0, 1);
  lcd.print("Level: ");
  lcd.print(percent);
  lcd.print("%   ");
  
  // Вывод в порт для проверки
  Serial.print("Raw: "); Serial.print(raw);
  Serial.print(" | Percent: "); Serial.println(percent);

  delay(1000);
}

🎥 Video Demonstration:



📊 Project Outcomes

Completed a functioning autonomous irrigation system

Achieved stable sensor readings

Eliminated LCD interference using proper grounding and capacitor

Pump control works reliably through MOSFET module

