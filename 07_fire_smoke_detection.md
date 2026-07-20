# Problem Statement 7: Industrial Fire & Smoke Detection System

## Problem Statement
Warehouses and factories require early fire detection combining flame and smoke sensing to trigger alarms and sprinkler/exhaust systems before fire spreads.

## Description
A flame sensor detects infrared light from open flames while an MQ-2 sensor detects smoke/combustible gas. If either sensor crosses its threshold, the system activates a buzzer, warning LED, and a relay that could trigger a sprinkler system.

## Components Required
- Arduino Uno
- Flame Sensor Module
- MQ-2 Smoke Sensor
- Buzzer
- Red LED
- Relay Module (sprinkler simulation)
- Breadboard + jumper wires

## Tinkercad Circuit Explanation
In Tinkercad, add a 'Flame Sensor' component (VCC to 5V, GND to GND, DO to pin 3) and a 'Gas Sensor' (MQ-2, AOUT to A0). Add a buzzer to pin 8, red LED to pin 7, and a relay module to pin 6. Start simulation; toggle the flame sensor and drag the gas sensor slider to simulate a fire event, and confirm the buzzer, LED, and relay all activate together in Serial Monitor output.

## Arduino Code

```cpp
const int flamePin = 3;
const int smokePin = A0;
const int buzzerPin = 8;
const int ledPin = 7;
const int sprinklerRelay = 6;

const int SMOKE_THRESHOLD = 300;

void setup() {
  Serial.begin(9600);
  pinMode(flamePin, INPUT);
  pinMode(buzzerPin, OUTPUT);
  pinMode(ledPin, OUTPUT);
  pinMode(sprinklerRelay, OUTPUT);
}

void loop() {
  int flameDetected = digitalRead(flamePin); // LOW usually means flame detected
  int smokeLevel = analogRead(smokePin);

  Serial.print("Flame: ");
  Serial.print(flameDetected == LOW ? "YES" : "NO");
  Serial.print("  Smoke Level: ");
  Serial.println(smokeLevel);

  if (flameDetected == LOW || smokeLevel > SMOKE_THRESHOLD) {
    digitalWrite(buzzerPin, HIGH);
    digitalWrite(ledPin, HIGH);
    digitalWrite(sprinklerRelay, HIGH);
    Serial.println("FIRE ALERT! Sprinkler activated.");
  } else {
    digitalWrite(buzzerPin, LOW);
    digitalWrite(ledPin, LOW);
    digitalWrite(sprinklerRelay, LOW);
  }

  delay(1000);
}

```

