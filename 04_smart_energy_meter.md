# Problem Statement 4: Smart Energy Meter with Overload Protection

## Problem Statement
Industries need to monitor real-time current draw of machines to track energy consumption and automatically cut power in case of overload to protect equipment.

## Description
An ACS712 current sensor measures the AC/DC current drawn by a load. The Arduino calculates the current draw and, if it exceeds a safe limit, switches off a relay controlling the load while logging consumption data over Serial for dashboard visualization.

## Components Required
- Arduino Uno
- ACS712 Current Sensor Module
- Relay Module
- LED (overload indicator)
- Breadboard + jumper wires

## Tinkercad Circuit Explanation
In Tinkercad, search for 'Current Sensor' (ACS712) in the components panel. Connect VCC to 5V, GND to GND, and OUT to Arduino A0. Wire a relay module's IN pin to Arduino pin 8, VCC to 5V, GND to GND (the relay's switched side can connect to a simulated load/motor). Add a red LED on pin 7 for overload indication. Run simulation and use the current sensor's slider to simulate varying load current, observing the relay switch off and LED light when it crosses the overload threshold.

## Arduino Code

```cpp
const int currentSensorPin = A0;
const int relayPin = 8;
const int overloadLED = 7;

const float SENSITIVITY = 0.185; // V per Amp for ACS712-05B
const float VREF = 2.5;
const float OVERLOAD_CURRENT = 5.0; // Amps

void setup() {
  Serial.begin(9600);
  pinMode(relayPin, OUTPUT);
  pinMode(overloadLED, OUTPUT);
  digitalWrite(relayPin, HIGH); // relay ON (load powered)
}

void loop() {
  int raw = analogRead(currentSensorPin);
  float voltage = (raw / 1023.0) * 5.0;
  float current = (voltage - VREF) / SENSITIVITY;
  current = abs(current);

  Serial.print("Current: ");
  Serial.print(current);
  Serial.println(" A");

  if (current > OVERLOAD_CURRENT) {
    digitalWrite(relayPin, LOW); // cut power
    digitalWrite(overloadLED, HIGH);
    Serial.println("OVERLOAD! Load disconnected.");
  } else {
    digitalWrite(relayPin, HIGH);
    digitalWrite(overloadLED, LOW);
  }

  delay(1000);
}

```

