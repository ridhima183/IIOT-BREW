# Problem Statement 8: Industrial Air Quality Monitoring System

## Problem Statement
Factories with chemical processes need to monitor indoor air quality (CO2, VOCs, ammonia) to ensure worker safety and trigger ventilation when pollution levels rise.

## Description
An MQ-135 air quality sensor measures overall air pollutant concentration. The Arduino classifies air quality into Good/Moderate/Poor bands and automatically switches on a ventilation fan relay when air quality degrades.

## Components Required
- Arduino Uno
- MQ-135 Air Quality Sensor
- Relay Module (ventilation fan)
- 3 LEDs (Green/Yellow/Red)
- Breadboard + jumper wires

## Tinkercad Circuit Explanation
In Tinkercad, add a 'Gas Sensor' component configured as MQ-135 (or reuse MQ-2 for simulation purposes since Tinkercad's library is limited). Connect AOUT to A0, VCC to 5V, GND to GND. Add green LED (pin 5), yellow LED (pin 6), red LED (pin 7), and a relay on pin 8. Run simulation and move the sensor's pollutant slider through low/medium/high ranges to see the LED band and fan relay respond accordingly in Serial Monitor.

## Arduino Code

```cpp
const int airSensorPin = A0;
const int greenLED = 5;
const int yellowLED = 6;
const int redLED = 7;
const int fanRelay = 8;

const int GOOD_LIMIT = 200;
const int MODERATE_LIMIT = 400;

void setup() {
  Serial.begin(9600);
  pinMode(greenLED, OUTPUT);
  pinMode(yellowLED, OUTPUT);
  pinMode(redLED, OUTPUT);
  pinMode(fanRelay, OUTPUT);
}

void loop() {
  int airQuality = analogRead(airSensorPin);
  Serial.print("Air Quality Reading: ");
  Serial.println(airQuality);

  digitalWrite(greenLED, LOW);
  digitalWrite(yellowLED, LOW);
  digitalWrite(redLED, LOW);

  if (airQuality <= GOOD_LIMIT) {
    digitalWrite(greenLED, HIGH);
    digitalWrite(fanRelay, LOW);
    Serial.println("Air Quality: GOOD");
  } else if (airQuality <= MODERATE_LIMIT) {
    digitalWrite(yellowLED, HIGH);
    digitalWrite(fanRelay, HIGH);
    Serial.println("Air Quality: MODERATE - Fan ON");
  } else {
    digitalWrite(redLED, HIGH);
    digitalWrite(fanRelay, HIGH);
    Serial.println("Air Quality: POOR - Fan ON, evacuate if worsening");
  }

  delay(1500);
}

```

