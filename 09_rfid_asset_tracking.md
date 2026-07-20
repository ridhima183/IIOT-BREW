# Problem Statement 9: RFID-Based Industrial Asset Tracking & Access Control

## Problem Statement
Factories need to track movement of tools, equipment, or personnel access at restricted zones using unique ID tags, logging every scan event for auditing.

## Description
An RC522 RFID reader scans tag/card UIDs. The Arduino compares the scanned UID against an authorized list; if matched, it grants access (green LED + relay to unlock a door/gate) and logs the event, otherwise it denies access (red LED + buzzer).

## Components Required
- Arduino Uno
- MFRC522 RFID Reader Module + Tags/Cards
- Green LED + Red LED
- Buzzer
- Relay Module (door lock)
- Breadboard + jumper wires

## Tinkercad Circuit Explanation
In Tinkercad, add the 'RFID Reader' component (search 'RFID'), which comes paired with a virtual RFID card/tag you can select and 'scan' by clicking during simulation. Wiring: SDA to pin 10, SCK to pin 13, MOSI to pin 11, MISO to pin 12, RST to pin 9, VCC to 3.3V, GND to GND (SPI pins are fixed on Uno). Add green LED on pin 5, red LED on pin 4, buzzer on pin 3. Note the tag's UID printed in Serial Monitor on first scan, hardcode it into the authorized list in code, then re-run to see access granted vs. an unregistered tag being denied.

## Arduino Code

```cpp
#include <SPI.h>
#include <MFRC522.h>

#define SS_PIN 10
#define RST_PIN 9
MFRC522 rfid(SS_PIN, RST_PIN);

const int greenLED = 5;
const int redLED = 4;
const int buzzerPin = 3;
const int doorRelay = 6;

// Replace with your actual scanned UID bytes
byte authorizedUID[4] = {0xDE, 0xAD, 0xBE, 0xEF};

void setup() {
  Serial.begin(9600);
  SPI.begin();
  rfid.PCD_Init();
  pinMode(greenLED, OUTPUT);
  pinMode(redLED, OUTPUT);
  pinMode(buzzerPin, OUTPUT);
  pinMode(doorRelay, OUTPUT);
  Serial.println("Scan an RFID tag...");
}

bool isAuthorized(byte *uid, byte size) {
  if (size != 4) return false;
  for (byte i = 0; i < 4; i++) {
    if (uid[i] != authorizedUID[i]) return false;
  }
  return true;
}

void loop() {
  if (!rfid.PICC_IsNewCardPresent() || !rfid.PICC_ReadCardSerial()) {
    return;
  }

  Serial.print("Tag UID: ");
  for (byte i = 0; i < rfid.uid.size; i++) {
    Serial.print(rfid.uid.uidByte[i], HEX);
    Serial.print(" ");
  }
  Serial.println();

  if (isAuthorized(rfid.uid.uidByte, rfid.uid.size)) {
    Serial.println("Access GRANTED");
    digitalWrite(greenLED, HIGH);
    digitalWrite(doorRelay, HIGH);
    delay(3000);
    digitalWrite(greenLED, LOW);
    digitalWrite(doorRelay, LOW);
  } else {
    Serial.println("Access DENIED");
    digitalWrite(redLED, HIGH);
    digitalWrite(buzzerPin, HIGH);
    delay(1500);
    digitalWrite(redLED, LOW);
    digitalWrite(buzzerPin, LOW);
  }

  rfid.PICC_HaltA();
}

```

