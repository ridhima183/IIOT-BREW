# Problem Statement 15: Predictive Maintenance: Combined Temperature + Vibration Health Monitor

## Problem Statement
Critical rotating machinery needs a combined health index using multiple sensor inputs (temperature and vibration) rather than a single metric, to reduce false alarms and better predict impending failure for scheduled maintenance.

## Description
This system combines a DHT11 (motor casing temperature) and an SW-420 vibration sensor to compute a simple composite 'health score.' If both readings trend into abnormal ranges simultaneously, it raises a high-confidence predictive maintenance alert, versus a lower-confidence warning if only one sensor is abnormal.

## Components Required
- Arduino Uno
- DHT11 Sensor
- SW-420 Vibration Sensor
- Green/Yellow/Red LEDs (health status)
- Buzzer
- Breadboard + jumper wires

## Tinkercad Circuit Explanation
In Tinkercad, add a DHT11 (OUT to pin 2) and an SW-420-style vibration/shock sensor (OUT to pin 3). Add green LED (pin 5), yellow LED (pin 6), red LED (pin 7), and buzzer (pin 8). Include the Adafruit DHT library. During simulation, adjust the DHT11 temperature slider and click/trigger the vibration sensor together to test all combinations (normal, one abnormal, both abnormal) and confirm the health status LEDs and buzzer match expectations in Serial Monitor.

## Arduino Code

```cpp
#include <DHT.h>

#define DHTPIN 2
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

const int vibrationPin = 3;
const int greenLED = 5;
const int yellowLED = 6;
const int redLED = 7;
const int buzzerPin = 8;

const float TEMP_LIMIT = 40.0;
volatile int vibrationEvents = 0;
unsigned long windowStart = 0;
const unsigned long WINDOW_MS = 5000;
const int VIBRATION_LIMIT = 4;

void countVibration() {
  vibrationEvents++;
}

void setup() {
  Serial.begin(9600);
  dht.begin();
  pinMode(vibrationPin, INPUT);
  pinMode(greenLED, OUTPUT);
  pinMode(yellowLED, OUTPUT);
  pinMode(redLED, OUTPUT);
  pinMode(buzzerPin, OUTPUT);
  attachInterrupt(digitalPinToInterrupt(vibrationPin), countVibration, RISING);
  windowStart = millis();
}

void loop() {
  if (millis() - windowStart >= WINDOW_MS) {
    float temp = dht.readTemperature();
    noInterrupts();
    int vibCount = vibrationEvents;
    vibrationEvents = 0;
    interrupts();

    bool tempAbnormal = !isnan(temp) && temp > TEMP_LIMIT;
    bool vibAbnormal = vibCount > VIBRATION_LIMIT;

    Serial.print("Temp: ");
    Serial.print(temp);
    Serial.print(" C | Vibration events: ");
    Serial.println(vibCount);

    digitalWrite(greenLED, LOW);
    digitalWrite(yellowLED, LOW);
    digitalWrite(redLED, LOW);
    digitalWrite(buzzerPin, LOW);

    if (tempAbnormal && vibAbnormal) {
      digitalWrite(redLED, HIGH);
      digitalWrite(buzzerPin, HIGH);
      Serial.println("HIGH RISK: Schedule immediate maintenance!");
    } else if (tempAbnormal || vibAbnormal) {
      digitalWrite(yellowLED, HIGH);
      Serial.println("MODERATE RISK: Monitor closely.");
    } else {
      digitalWrite(greenLED, HIGH);
      Serial.println("Machine Health: NORMAL");
    }

    windowStart = millis();
  }
}

```

