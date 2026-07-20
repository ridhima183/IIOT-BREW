# Problem Statement 5: Industrial Water/Chemical Tank Level Monitoring

## Problem Statement
Factories need to monitor liquid levels in storage tanks (water, chemicals, raw materials) to prevent overflow or running dry, and automatically control an inlet pump.

## Description
An HC-SR04 ultrasonic sensor mounted at the top of a tank measures the distance to the liquid surface, which is converted into a fill percentage. A relay-controlled pump is automatically switched on when the level is low and off when full.

## Components Required
- Arduino Uno
- HC-SR04 Ultrasonic Sensor
- Relay Module (pump control)
- LEDs (Low/Full indicators)
- Breadboard + jumper wires

## Tinkercad Circuit Explanation
In Tinkercad, add an Ultrasonic Distance Sensor from components. Connect VCC to 5V, GND to GND, TRIG to pin 9, ECHO to pin 10. Add a relay module on pin 7 to simulate pump control, plus a blue LED (Full) on pin 6 and yellow LED (Low) on pin 5. During simulation, use the sensor's distance slider (representing tank empty-to-full distance) to simulate liquid level changes and observe pump relay and LED behavior in Serial Monitor.

## Arduino Code

```cpp
const int trigPin = 9;
const int echoPin = 10;
const int pumpRelay = 7;
const int fullLED = 6;
const int lowLED = 5;

const int TANK_HEIGHT_CM = 100;
const int LOW_LEVEL_PERCENT = 20;
const int FULL_LEVEL_PERCENT = 90;

void setup() {
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(pumpRelay, OUTPUT);
  pinMode(fullLED, OUTPUT);
  pinMode(lowLED, OUTPUT);
}

long readDistanceCM() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  long duration = pulseIn(echoPin, HIGH);
  return duration * 0.034 / 2;
}

void loop() {
  long distance = readDistanceCM();
  int fillPercent = map(distance, TANK_HEIGHT_CM, 0, 0, 100);
  fillPercent = constrain(fillPercent, 0, 100);

  Serial.print("Tank Level: ");
  Serial.print(fillPercent);
  Serial.println(" %");

  if (fillPercent <= LOW_LEVEL_PERCENT) {
    digitalWrite(pumpRelay, HIGH); // turn pump ON
    digitalWrite(lowLED, HIGH);
    digitalWrite(fullLED, LOW);
    Serial.println("Low level - Pump ON");
  } else if (fillPercent >= FULL_LEVEL_PERCENT) {
    digitalWrite(pumpRelay, LOW); // turn pump OFF
    digitalWrite(fullLED, HIGH);
    digitalWrite(lowLED, LOW);
    Serial.println("Tank full - Pump OFF");
  } else {
    digitalWrite(lowLED, LOW);
    digitalWrite(fullLED, LOW);
  }

  delay(1500);
}

```

