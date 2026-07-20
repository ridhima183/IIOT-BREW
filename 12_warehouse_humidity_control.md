# Problem Statement 12: Automated Warehouse Humidity & Ventilation Control

## Problem Statement
Stored goods (electronics, textiles, grains) degrade in high humidity. A warehouse needs automatic activation of a dehumidifier/exhaust fan when humidity crosses a safe limit.

## Description
A DHT11 sensor measures humidity continuously. The Arduino switches on a relay-controlled exhaust fan/dehumidifier when humidity exceeds the safe limit and switches it off once it returns to normal, with hysteresis to avoid relay chattering.

## Components Required
- Arduino Uno
- DHT11 Sensor
- Relay Module (fan/dehumidifier)
- LED indicator
- Breadboard + jumper wires

## Tinkercad Circuit Explanation
In Tinkercad, add a DHT11 sensor (VCC to 5V, GND to GND, OUT to pin 2). Add a relay module on pin 7 and an LED indicator on pin 6. Add the Adafruit DHT library via Tinkercad's library manager. Run the simulation and use the DHT11's humidity slider to push values above and below the threshold, observing the relay (fan) switch on/off with hysteresis in the Serial Monitor.

## Arduino Code

```cpp
#include <DHT.h>

#define DHTPIN 2
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

const int fanRelay = 7;
const int indicatorLED = 6;

const float HUMIDITY_ON = 65.0;
const float HUMIDITY_OFF = 55.0; // hysteresis band

bool fanRunning = false;

void setup() {
  Serial.begin(9600);
  dht.begin();
  pinMode(fanRelay, OUTPUT);
  pinMode(indicatorLED, OUTPUT);
}

void loop() {
  float humidity = dht.readHumidity();

  if (isnan(humidity)) {
    Serial.println("Sensor error");
    delay(2000);
    return;
  }

  Serial.print("Humidity: ");
  Serial.print(humidity);
  Serial.println(" %");

  if (!fanRunning && humidity > HUMIDITY_ON) {
    fanRunning = true;
  } else if (fanRunning && humidity < HUMIDITY_OFF) {
    fanRunning = false;
  }

  digitalWrite(fanRelay, fanRunning ? HIGH : LOW);
  digitalWrite(indicatorLED, fanRunning ? HIGH : LOW);
  Serial.println(fanRunning ? "Fan: ON" : "Fan: OFF");

  delay(2000);
}

```

