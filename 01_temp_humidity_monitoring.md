# Problem Statement 1: Industrial Temperature & Humidity Monitoring System

## Problem Statement
Factories need continuous monitoring of ambient temperature and humidity in server rooms, cold storage, or production floors to prevent equipment damage and product spoilage, with automatic alerts when readings cross safe thresholds.

## Description
This system uses a DHT11 sensor to continuously read temperature and humidity. If either value exceeds a preset safe range, a buzzer and LED alert is triggered, and readings are printed to the Serial Monitor to simulate data being pushed to a cloud dashboard (e.g., via ESP8266/MQTT in a real deployment).

## Components Required
- Arduino Uno
- DHT11 Temperature & Humidity Sensor
- 16x2 LCD (optional, or use Serial Monitor)
- Buzzer
- Red LED (alert) + Green LED (normal)
- 220 ohm resistors x2
- Breadboard + jumper wires

## Tinkercad Circuit Explanation
In Tinkercad: drag an Arduino Uno onto the canvas, then add a DHT11 sensor from the components panel (search 'temperature sensor'). Connect DHT11 VCC to 5V, GND to GND, and the data (OUT) pin to Arduino digital pin 2. Add a buzzer with its positive leg to pin 8 and negative to GND. Add a red LED (through a 220 ohm resistor) to pin 7 and a green LED (through a resistor) to pin 6, both grounded. In the Code editor, switch to 'Text' mode and paste the sketch below; you'll need to add the DHT library via the Tinkercad library manager (search 'DHT sensor library' by Adafruit) since Tinkercad supports a subset of Arduino libraries natively for this sensor. Start the simulation and drag the DHT11 slider to change temperature/humidity and watch the LEDs and Serial Monitor respond.

## Arduino Code

```cpp
#include <DHT.h>

#define DHTPIN 2
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

const int buzzerPin = 8;
const int redLED = 7;
const int greenLED = 6;

const float TEMP_HIGH_LIMIT = 35.0;
const float HUM_HIGH_LIMIT = 70.0;

void setup() {
  Serial.begin(9600);
  dht.begin();
  pinMode(buzzerPin, OUTPUT);
  pinMode(redLED, OUTPUT);
  pinMode(greenLED, OUTPUT);
}

void loop() {
  float temp = dht.readTemperature();
  float hum = dht.readHumidity();

  if (isnan(temp) || isnan(hum)) {
    Serial.println("Sensor read failed!");
    delay(2000);
    return;
  }

  Serial.print("Temperature: ");
  Serial.print(temp);
  Serial.print(" C  Humidity: ");
  Serial.print(hum);
  Serial.println(" %");

  if (temp > TEMP_HIGH_LIMIT || hum > HUM_HIGH_LIMIT) {
    digitalWrite(redLED, HIGH);
    digitalWrite(greenLED, LOW);
    digitalWrite(buzzerPin, HIGH);
    Serial.println("ALERT: Threshold exceeded!");
  } else {
    digitalWrite(redLED, LOW);
    digitalWrite(greenLED, HIGH);
    digitalWrite(buzzerPin, LOW);
  }

  delay(2000);
}

```

