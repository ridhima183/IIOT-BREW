# Problem Statement 13: Load Cell Based Weight Monitoring for Material Handling

## Problem Statement
Manufacturing lines need to verify that packaged goods or raw material batches meet target weight specifications, flagging underweight or overweight items for quality control.

## Description
A load cell with an HX711 amplifier measures the weight of an item placed on a platform. The Arduino compares the reading against a target weight range and signals Pass/Underweight/Overweight via LEDs, useful for automated quality gates.

## Components Required
- Arduino Uno
- Load Cell (e.g., 5kg) + HX711 Amplifier Module
- 3 LEDs (Pass/Under/Over)
- Breadboard + jumper wires

## Tinkercad Circuit Explanation
Tinkercad's component library doesn't include a native load cell/HX711, so for simulation purposes substitute a potentiometer on A0 to represent the scaled weight reading (documented as a stand-in in your report). Connect the potentiometer's wiper to A0, outer legs to 5V and GND. Add green LED (pin 5, Pass), yellow LED (pin 6, Underweight), and red LED (pin 7, Overweight). Turn the potentiometer during simulation to sweep through weight values and observe the correct LED lighting for each band via Serial Monitor.

## Arduino Code

```cpp
// Note: For real hardware, use the HX711 library with a load cell.
// This Tinkercad-simulated version uses a potentiometer on A0 as a stand-in
// for the calibrated weight reading (0-1023 mapped to grams).

const int weightPin = A0;
const int passLED = 5;
const int underLED = 6;
const int overLED = 7;

const float TARGET_WEIGHT = 500.0; // grams
const float TOLERANCE = 20.0;      // +/- grams

void setup() {
  Serial.begin(9600);
  pinMode(passLED, OUTPUT);
  pinMode(underLED, OUTPUT);
  pinMode(overLED, OUTPUT);
}

void loop() {
  int raw = analogRead(weightPin);
  float weight = map(raw, 0, 1023, 0, 1000); // simulated grams

  Serial.print("Weight: ");
  Serial.print(weight);
  Serial.println(" g");

  digitalWrite(passLED, LOW);
  digitalWrite(underLED, LOW);
  digitalWrite(overLED, LOW);

  if (weight < TARGET_WEIGHT - TOLERANCE) {
    digitalWrite(underLED, HIGH);
    Serial.println("Status: UNDERWEIGHT");
  } else if (weight > TARGET_WEIGHT + TOLERANCE) {
    digitalWrite(overLED, HIGH);
    Serial.println("Status: OVERWEIGHT");
  } else {
    digitalWrite(passLED, HIGH);
    Serial.println("Status: PASS");
  }

  delay(1000);
}

```

