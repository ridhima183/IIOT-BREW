# Problem Statement 3: Machine Vibration Monitoring for Predictive Maintenance

## Problem Statement
Rotating machinery (motors, pumps, compressors) can fail suddenly due to bearing wear or misalignment. Continuous vibration monitoring allows early detection of abnormal vibration patterns before catastrophic failure.

## Description
An SW-420 vibration sensor detects mechanical vibration/shock. The Arduino counts vibration events over a time window; if the count exceeds a normal operating threshold, it flags a possible fault condition and raises an alert, simulating a predictive maintenance trigger.

## Components Required
- Arduino Uno
- SW-420 Vibration Sensor Module
- Buzzer
- LED
- Breadboard + jumper wires

## Tinkercad Circuit Explanation
In Tinkercad, add a 'Vibration Sensor' (SW-420 style, or use a tilt/shock sensor from the components search). Connect VCC to 5V, GND to GND, DO (digital out) to Arduino pin 2. Add an LED on pin 7 and buzzer on pin 8. Since Tinkercad's physical shake simulation is limited, use the sensor's built-in toggle/click feature during simulation to simulate vibration pulses, and watch the vibration counter and alert logic respond in the Serial Monitor.

## Arduino Code

```cpp
const int vibrationPin = 2;
const int ledPin = 7;
const int buzzerPin = 8;

volatile int vibrationCount = 0;
unsigned long windowStart = 0;
const unsigned long WINDOW_MS = 5000;
const int VIBRATION_THRESHOLD = 5;

void countVibration() {
  vibrationCount++;
}

void setup() {
  Serial.begin(9600);
  pinMode(vibrationPin, INPUT);
  pinMode(ledPin, OUTPUT);
  pinMode(buzzerPin, OUTPUT);
  attachInterrupt(digitalPinToInterrupt(vibrationPin), countVibration, RISING);
  windowStart = millis();
}

void loop() {
  if (millis() - windowStart >= WINDOW_MS) {
    Serial.print("Vibration events in window: ");
    Serial.println(vibrationCount);

    if (vibrationCount > VIBRATION_THRESHOLD) {
      digitalWrite(ledPin, HIGH);
      digitalWrite(buzzerPin, HIGH);
      Serial.println("ALERT: Abnormal vibration - schedule maintenance!");
    } else {
      digitalWrite(ledPin, LOW);
      digitalWrite(buzzerPin, LOW);
    }

    vibrationCount = 0;
    windowStart = millis();
  }
}

```

