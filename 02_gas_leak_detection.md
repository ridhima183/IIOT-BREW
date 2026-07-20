# Problem Statement 2: Industrial Gas Leak Detection System

## Problem Statement
Chemical plants and manufacturing units need real-time detection of flammable/toxic gas leaks (LPG, methane, smoke) to trigger evacuation alarms before concentration reaches dangerous levels.

## Description
An MQ-2 gas sensor continuously monitors air quality. When gas concentration crosses a calibrated threshold, the system sounds a buzzer, lights a warning LED, and could trigger an exhaust fan relay in a real deployment.

## Components Required
- Arduino Uno
- MQ-2 Gas Sensor Module
- Buzzer
- Red LED
- Relay Module (optional, for exhaust fan)
- Breadboard + jumper wires

## Tinkercad Circuit Explanation
In Tinkercad, search for 'Gas Sensor' in components (it appears as MQ-2 or a generic gas sensor with a slider). Connect VCC to 5V, GND to GND, and AOUT to Arduino analog pin A0. Wire a buzzer to digital pin 9 and a red LED (with 220 ohm resistor) to pin 8, both returning to GND. Paste the code in Text mode, start simulation, and drag the gas sensor's concentration slider upward to simulate a leak — observe the buzzer/LED activate once the analog reading crosses the threshold in Serial Monitor.

## Arduino Code

```cpp
const int gasSensorPin = A0;
const int buzzerPin = 9;
const int ledPin = 8;

const int GAS_THRESHOLD = 300;

void setup() {
  Serial.begin(9600);
  pinMode(buzzerPin, OUTPUT);
  pinMode(ledPin, OUTPUT);
}

void loop() {
  int gasLevel = analogRead(gasSensorPin);
  Serial.print("Gas Level: ");
  Serial.println(gasLevel);

  if (gasLevel > GAS_THRESHOLD) {
    digitalWrite(buzzerPin, HIGH);
    digitalWrite(ledPin, HIGH);
    Serial.println("WARNING: Gas leak detected!");
  } else {
    digitalWrite(buzzerPin, LOW);
    digitalWrite(ledPin, LOW);
  }

  delay(1000);
}

```

