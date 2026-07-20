# Problem Statement 11: Smart Warehouse/Street Lighting with Occupancy Sensing

## Problem Statement
Industrial facilities waste energy running lights at full brightness in empty or well-lit areas. An automated system should dim/turn off lights when no motion is detected or ambient light is sufficient, and turn on when needed.

## Description
An LDR measures ambient light while a PIR sensor detects human presence. Lights (simulated by an LED, PWM-dimmable) turn on only when it's dark AND motion is detected, and turn off automatically after a period of no motion, saving energy.

## Components Required
- Arduino Uno
- LDR (Light Dependent Resistor) + 10k resistor
- PIR Motion Sensor
- LED (representing the light, on a PWM pin)
- Breadboard + jumper wires

## Tinkercad Circuit Explanation
In Tinkercad, wire the LDR in a voltage divider with a 10k resistor between 5V and GND, with the midpoint going to A0. Add a PIR Motion Sensor with VCC to 5V, GND to GND, OUT to pin 2. Connect an LED (through a resistor) to PWM pin 9 to represent the light. Run simulation, click the LDR to darken it and click the PIR sensor's motion trigger to simulate a person entering, and observe the LED (light) brightness responding in Serial Monitor and visually.

## Arduino Code

```cpp
const int ldrPin = A0;
const int pirPin = 2;
const int lightPin = 9;

const int DARK_THRESHOLD = 400;
unsigned long lastMotionTime = 0;
const unsigned long LIGHT_TIMEOUT_MS = 10000;

void setup() {
  Serial.begin(9600);
  pinMode(pirPin, INPUT);
  pinMode(lightPin, OUTPUT);
}

void loop() {
  int lightLevel = analogRead(ldrPin);
  int motion = digitalRead(pirPin);

  bool isDark = lightLevel < DARK_THRESHOLD;

  if (motion == HIGH) {
    lastMotionTime = millis();
  }

  bool recentMotion = (millis() - lastMotionTime) < LIGHT_TIMEOUT_MS;

  if (isDark && recentMotion) {
    analogWrite(lightPin, 255); // full brightness
    Serial.println("Light: ON (dark + motion)");
  } else if (isDark && !recentMotion) {
    analogWrite(lightPin, 40); // dim standby
    Serial.println("Light: DIM (dark, no motion)");
  } else {
    analogWrite(lightPin, 0); // off, enough ambient light
    Serial.println("Light: OFF (sufficient daylight)");
  }

  delay(1000);
}

```

