# Carenuity SGP30 Piggyback Adapter (V1.1)

![image alt](https://raw.githubusercontent.com/0mollo/Carenuity-SGP30-Piggyback/refs/heads/main/GY-SGP30%20Adapter%20top%20view.png) | ![image alt](https://github.com/0mollo/Carenuity-SGP30-Piggyback/blob/main/GY-SGP30%20Adapter%20bottom%20view.png)

Carenuity ecosystem plug-and-play adapter for the SGP30 Air Quality Sensor module.

This PCB allows easy integration of the SGP30 VOC + eCO2 sensor into development platforms such as ESP8266 (D1 Mini), C3 Mini, and similar 3.3V microcontroller systems.


##  Overview

The SGP30 is a digital gas sensor designed for:

- Total Volatile Organic Compounds (TVOC)
- Equivalent CO₂ (eCO2)

This adapter provides:

- Clean I2C breakout
- 3.3V operation
- Dual 8-pin side headers (D1 Mini compatible layout)
- Dedicated 4-pin SGP30 module header
- Carenuity branded ecosystem module


##  Electrical Specifications

| Parameter | Value |
|------------|--------|
| Operating Voltage | 3.3V |
| Communication | I2C |
| Logic Level | 3.3V |
| Pull-up Resistors | Not included |
| I2C Address | Determined by SGP30 module |
| PCB Version | V1.1 |




### SGP30 Module Header (4-Pin)

| Pin | Function |
|------|----------|
| VIN  | 3.3V |
| GND  | Ground |
| SCL  | I2C Clock |
| SDA  | I2C Data |



##  PCB Mechanical Specifications

| Parameter | Value |
|------------|--------|
| Form Factor | C3 Mini Shield Compatible |
| Mount Type | Through-hole headers |
| Module Interface | 4-pin vertical header |
| Branding | Carenuity Ecosystem |
| Version | V1.1 |



##  Typical Wiring (ESP8266 C3 Mini)

| SGP30 | D1 Mini |
|--------|---------|
| VIN | 3V3 |
| GND | GND |
| SCL | D1 |
| SDA | D2 |

## File Schematics

[See](https://github.com/0mollo/Carenuity-SGP30-Piggyback/blob/main/GY-SGP30%20Adapter.pdf)

##  Arduino Example Code

```cpp
#include <Wire.h>
#include "Adafruit_SGP30.h"

Adafruit_SGP30 sgp;

void setup() {
  Serial.begin(115200);
  Wire.begin(D2, D1); // SDA, SCL

  if (!sgp.begin()) {
    Serial.println("Sensor not found");
    while (1);
  }
}

void loop() {
  if (!sgp.IAQmeasure()) {
    Serial.println("Measurement failed");
    return;
  }

  Serial.print("TVOC: ");
  Serial.print(sgp.TVOC);
  Serial.print(" ppb\t");

  Serial.print("eCO2: ");
  Serial.print(sgp.eCO2);
  Serial.println(" ppm");

  delay(1000);
}
