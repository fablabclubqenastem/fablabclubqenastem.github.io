---
showonlyimage: true
title:      "Chess-bird"
subtitle:   "The bird that sees chess!"
description: "A drone that uses various sensors to determine the properties of the scanned area"
date:       2022-01-11
author: Ola Hamdy
image: "/projects/chess-bird/chess-bird.jpg"
published: true 
tags:
    - ESP
    - Drone

categories: [ Flying ]
URL: "/projects/chess-bird/"
---

# Overview
The drone is made to satisfy the learning objectives of the FabLab club’s
students. With various functions such as weather predicting and fire detecting,
the drone’s purposes justify the students’ intention to help Egypt overcome
many environmental crises. 

Remotely controlled, the students took advantage of great tools like a flutter
programming language, mainly to send data from an Esp32 chip. With a high
accuracy level, the drone’s sensors (DHT11 - HC-SR04 - KY-026) draw a
negligible range of errors, making it the best life-saver in most situations. 

Supported by both batteries and PV panels, the drone is expected to store
electricity in batteries in the mornings while working using the PV panels and
use those batteries to supply the drone itself at night. 

# Goals
The project is application of what the students learned in the club, so it matches: 
1. The aim of learning through practicing. 
2. The desire of the students to help their country overcome many crises. 
3. The purpose of getting a solid understanding of microcontrollers and chips. 
4. The goal of the club in teaching the students how to code Embedded Systems and to work independently from scratch. 

# Tools
Building this project, the students worked on various concepts from drawing and
sketching to coding and 3D CAD designing depending on these tools: 
* ESP-wroom-32 chip
![projects chess bird esp jpg](/projects/chess-bird/esp.jpg)
* 4 DC Motors
![projects chess bird dc motors jpg](/projects/chess-bird/dc-motors.jpg)
* DHT11 (Humidity Sensor)
![projects chess bird dht11 jpg](/projects/chess-bird/dht11.jpg)
* KY-026 (Flame Sensor)
![projects chess bird KY 026 jpg](/projects/chess-bird/ky-026.jpg)
* Double-paged PV panel
![projects chess bird pv jpg](/projects/chess-bird/pv.jpg)
* Jumper wires
![projects chess bird jumper wires jpg](/projects/chess-bird/jumper-wires.jpg)

# Specifications
## Led
![projects chess bird led connections jpg](/projects/chess-bird/led-connections.jpg)

## Sensor
![projects chess bird sensor connections jpg](/projects/chess-bird/sensor-connections.jpg)

# Functions
## Sensor
```Arduino
#include "DHT.h"
#define echoPin 4
#define trigPin 3
#define DHTPIN 2
long duration;
float distance;
DHT dht(DHT11, DHTTYPE);

void setup() {
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  dht.begin();
}
void loop() {

  float h = dht.readHumidity();
  // Read temperature as Celsius (the default)
  float t = dht.readTemperature();
  // Read temperature as Fahrenheit (isFahrenheit = true)
  float f = dht.readTemperature(true);

  // Check if any reads failed and exit early (to try again).
  if (isnan(h) || isnan(t) || isnan(f)) {
    Serial.println(F("Failed to read from DHT sensor!"));
    return;
  }
  // Compute heat index in Fahrenheit (the default)
  float hif = dht.computeHeatIndex(f, h);
  // Compute heat index in Celsius (isFahreheit = false)
  float hic = dht.computeHeatIndex(t, h, false);

  Serial.print(F("Humidity: "));
  Serial.print(h);
  Serial.print(F("%  Temperature: "));
  Serial.print(t);
  Serial.print(F("°C "));
  Serial.print(f);
  Serial.print(F("°F  Heat index: "));
  Serial.print(hic);
  Serial.print(F("°C "));
  Serial.print(hif);
  Serial.println(F("°F"));

  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  // Sets the trigPin HIGH (ACTIVE) for 10 microseconds
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration = pulseIn(echoPin, HIGH);
  // Calculating the distance
  distance = duration * 0.034 / 2; // Speed of sound wave divided by 2 (go and back)
  // Displays the distance on the Serial Monitor
  Serial.print("Drone Height: ");
  Serial.print(distance / 100);
  Serial.println(" m");
  Serial.println("\t ---- **** ----");
  delay(1000);

  float sensorValue = analogRead(A0);
  if (sensorValue < 570) {
    Serial.println("There is a fire out there! ");
    Serial.print("The value is: ");
    Serial.print(sensorValue);
    Serial.println(" nm");

    delay(500);
  }
  else
  {
    Serial.println("There is no fire! Stay safe :)");
    Serial.print("The value is: ");
    Serial.print(sensorValue);
    Serial.println(" nm");
    delay(500);
  }
}
```

## Led
```Arduino
int LEDs = 4;
// 
void setup()
{
  pinMode(LEDs, OUTPUT);
}

void loop()
{
  digitalWrite(LEDs, HIGH);
  delay(1000); // Wait for 1000 millisecond(s)
  digitalWrite(LEDs, LOW);
  delay(1000); // Wait for 1000 millisecond(s)
}
```

## DC Motors
```Arduino
#include <DC_Motor.h>
DC_Motor MotorA(9,A0, A1);
DC_Motor MotorB(3,A2, A3);

// MotorA
 
int enA = 9;
int in1 = A0;
int in2 = A1;
 
// MotorB
 
int enB = 3;
int in3 = A2;
int in4 = A3;
 
void setup()
 
{
 
  // Set all the motor control pins to outputs
 
  pinMode(enA, OUTPUT);
  pinMode(enB, OUTPUT);
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(in4, OUTPUT);
  
}
 
void loop() {

  if (F)
  {
    // This is Forward
 
    // Set Motor A Forward
 MotorA.set_speed(40);

 MotorA.forward_with_set_speed();
   
 
    // Set Motor B Forward
 MotorB.set_speed(40);

 MotorB.forward_with_set_speed();
  }
  else if (B)
  {
    // This is Backward
 
    // Set Motor A Backward
MotorA.set_speed(80);

MotorA.reverse_with_set_speed()
   
 
    // Set Motor B Backward
 
 MotorB.set_speed(80);

 MotorB.reverse_with_set_speed()  
  }
 
  if (L)
  {
    // Move Left
 // Set Motor A Forward
 MotorA.set_speed(40);

 MotorA.forward_with_set_speed();
   
 
  }
  else if (R)
  {
    // Move Right
 
    // Map the number to a value of 255 maximum
 
    // Set Motor A Forward
 MotorA.set_speed(40);

 MotorA.forward_with_set_speed();
 
  }
}
```
