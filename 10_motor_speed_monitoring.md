# Problem Statement 10: Motor Speed (RPM) Monitoring System

## Problem Statement
Industrial motors need continuous RPM monitoring to detect abnormal speed variations that indicate load imbalance, belt slippage, or motor degradation.

## Description
An IR slotted sensor (or IR reflective sensor with a slotted disc on the motor shaft) generates pulses as the motor rotates. The Arduino counts pulses over a time window and calculates RPM, flagging an alert if RPM falls outside the expected operating range.

## Components Required
- Arduino Uno
- IR Slotted Optical Sensor (or IR reflective sensor + encoder disc)
- Buzzer
- LED
- Breadboard + jumper wires

## Tinkercad Circuit Explanation
In Tinkercad, add an 'IR Sensor' component to represent the slotted/encoder sensor. Connect VCC to 5V, GND to GND, OUT to Arduino pin 2 (interrupt pin). Add an LED to pin 7 and buzzer to pin 8. Since Tinkercad can't physically spin a motor with a slotted disc, simulate pulses by rapidly toggling the IR sensor's trigger during the simulation run, and observe calculated RPM values printed to Serial Monitor.

## Arduino Code

```cpp
const int irPin = 2;
const int ledPin = 7;
const int buzzerPin = 8;

volatile unsigned long pulseCount = 0;
unsigned long lastCalcTime = 0;
const unsigned long CALC_INTERVAL_MS = 2000;
const int PULSES_PER_REV = 1;

const int MIN_RPM = 500;
const int MAX_RPM = 3000;

void countPulse() {
  pulseCount++;
}

void setup() {
  Serial.begin(9600);
  pinMode(irPin, INPUT);
  pinMode(ledPin, OUTPUT);
  pinMode(buzzerPin, OUTPUT);
  attachInterrupt(digitalPinToInterrupt(irPin), countPulse, FALLING);
  lastCalcTime = millis();
}

void loop() {
  if (millis() - lastCalcTime >= CALC_INTERVAL_MS) {
    noInterrupts();
    unsigned long count = pulseCount;
    pulseCount = 0;
    interrupts();

    float rpm = (count / (float)PULSES_PER_REV) * (60000.0 / CALC_INTERVAL_MS);

    Serial.print("Motor RPM: ");
    Serial.println(rpm);

    if (rpm < MIN_RPM || rpm > MAX_RPM) {
      digitalWrite(ledPin, HIGH);
      digitalWrite(buzzerPin, HIGH);
      Serial.println("ALERT: RPM out of safe range!");
    } else {
      digitalWrite(ledPin, LOW);
      digitalWrite(buzzerPin, LOW);
    }

    lastCalcTime = millis();
  }
}

```

