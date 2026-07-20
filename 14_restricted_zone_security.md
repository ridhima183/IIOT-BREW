# Problem Statement 14: Restricted Zone Intrusion Detection & Security Alert

## Problem Statement
Industrial facilities have restricted zones (high-voltage areas, hazardous storage) that require automated intrusion detection outside working hours, alerting security instantly.

## Description
A PIR motion sensor monitors a restricted zone. When motion is detected while the system is 'armed' (toggled by a switch/button representing after-hours mode), it triggers a buzzer and flashing LED alarm, and logs the intrusion timestamp over Serial.

## Components Required
- Arduino Uno
- PIR Motion Sensor
- Pushbutton (Arm/Disarm toggle)
- Buzzer
- Red LED
- Breadboard + jumper wires

## Tinkercad Circuit Explanation
In Tinkercad, add a PIR Motion Sensor (VCC 5V, GND, OUT to pin 2) and a pushbutton wired with a pull-down resistor to pin 4 to represent an Arm/Disarm switch. Add a buzzer on pin 8 and red LED on pin 7. Start simulation, click the pushbutton once to arm the system, then trigger the PIR sensor's motion event to confirm the buzzer/LED alarm activates only while armed, and clears when disarmed via the button again.

## Arduino Code

```cpp
const int pirPin = 2;
const int buttonPin = 4;
const int buzzerPin = 8;
const int ledPin = 7;

bool armed = false;
bool lastButtonState = LOW;

void setup() {
  Serial.begin(9600);
  pinMode(pirPin, INPUT);
  pinMode(buttonPin, INPUT);
  pinMode(buzzerPin, OUTPUT);
  pinMode(ledPin, OUTPUT);
}

void loop() {
  bool buttonState = digitalRead(buttonPin);
  if (buttonState == HIGH && lastButtonState == LOW) {
    armed = !armed;
    Serial.println(armed ? "System ARMED" : "System DISARMED");
    delay(300); // debounce
  }
  lastButtonState = buttonState;

  if (armed) {
    int motion = digitalRead(pirPin);
    if (motion == HIGH) {
      digitalWrite(buzzerPin, HIGH);
      digitalWrite(ledPin, HIGH);
      Serial.println("INTRUSION DETECTED! Security alerted.");
    } else {
      digitalWrite(buzzerPin, LOW);
      digitalWrite(ledPin, LOW);
    }
  } else {
    digitalWrite(buzzerPin, LOW);
    digitalWrite(ledPin, LOW);
  }

  delay(200);
}

```

