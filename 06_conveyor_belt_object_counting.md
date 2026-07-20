# Problem Statement 6: Conveyor Belt Object Counting & Jam Detection

## Problem Statement
Production lines need automated counting of items passing on a conveyor belt for inventory tracking, and detection of jams when items stop moving unexpectedly.

## Description
An IR obstacle sensor placed across the conveyor belt detects each object as it passes, incrementing a counter. If no object is detected for an extended period despite the belt motor running, a jam alert is raised.

## Components Required
- Arduino Uno
- IR Obstacle Avoidance Sensor Module
- 16x2 LCD or Serial Monitor for count display
- Buzzer (jam alert)
- Breadboard + jumper wires

## Tinkercad Circuit Explanation
In Tinkercad, add an 'IR Sensor' (or 'IR Distance Sensor') from components. Connect VCC to 5V, GND to GND, OUT to Arduino pin 2. Add a buzzer on pin 8. Run the simulation and toggle the IR sensor's detection (many Tinkercad IR sensors have a clickable trigger or distance slider) repeatedly to simulate objects passing; pause toggling for a while to simulate a jam and observe the buzzer trigger after the timeout in Serial Monitor.

## Arduino Code

```cpp
const int irPin = 2;
const int buzzerPin = 8;

int objectCount = 0;
int lastState = HIGH;
unsigned long lastDetectionTime = 0;
const unsigned long JAM_TIMEOUT_MS = 8000;

void setup() {
  Serial.begin(9600);
  pinMode(irPin, INPUT);
  pinMode(buzzerPin, OUTPUT);
  lastDetectionTime = millis();
}

void loop() {
  int state = digitalRead(irPin);

  if (state == LOW && lastState == HIGH) {
    objectCount++;
    lastDetectionTime = millis();
    Serial.print("Object detected! Count: ");
    Serial.println(objectCount);
  }
  lastState = state;

  if (millis() - lastDetectionTime > JAM_TIMEOUT_MS) {
    digitalWrite(buzzerPin, HIGH);
    Serial.println("JAM ALERT: No object detected recently!");
  } else {
    digitalWrite(buzzerPin, LOW);
  }

  delay(50);
}

```

