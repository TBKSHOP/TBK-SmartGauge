# PROJECT_CONTEXT.md

## Project Name

TBK SmartGauge V1.0

## Overview

TBK SmartGauge เป็นโปรเจกต์ ESP32 + TFT Touch Display สำหรับรถจักรยานยนต์

ระบบหลักประกอบด้วย

* Dashboard
* AFR Monitor
* GPS Speed
* DYNO ROAD
* VIP Menu
* License System
* Data Logging

UI ใช้ TFT_eSPI และ Touch Screen

---

# Hardware

## MCU

ESP32

## Display

320x240 TFT Touch Screen

Library:

* TFT_eSPI

## Sensors

### ECU Data

อ่านข้อมูลจาก ECU

ค่าที่ใช้งาน

* RPM
* TPS
* ECT
* IAT
* Battery
* AFR

### GPS

ใช้ GPS Module

ค่าที่ใช้งาน

* Speed
* Latitude
* Longitude

---

# Page Structure

## Page 1

Boot Logo

## Page 2

Connect Screen

## Page 3

Dashboard

## Page 4

AFR Monitor

## Page 5

VIP Menu

## Page 8

DYNO ROAD

## Page 22

DYNO LOGS

## Page 23

RUN001 Detail

---

# DYNO ROAD SYSTEM

Current Page ID:

```cpp
currentPage = 8;
```

Purpose:

คำนวณสมรรถนะรถจาก

* RPM
* GPS Speed
* Gear Ratio

ไม่ใช้

* Hall Sensor
* น้ำหนักรถ

---

## DYNO Variables

```cpp
bool dynoRunning = false;

unsigned long dynoStartTime = 0;

int dynoMaxRPM = 0;

float run1Time = 0;

int run1RPM = 0;
```

---

## DYNO Buttons

### START

เริ่มจับเวลา

```cpp
dynoRunning = true;
dynoStartTime = millis();
```

### SAVE

บันทึก Run ปัจจุบัน

```cpp
run1RPM = dynoMaxRPM;

run1Time =
(millis() - dynoStartTime) / 1000.0;
```

### LOGS

เปิดหน้า Log

```cpp
currentPage = 22;
drawDynoLogsPage();
```

### BACK

กลับ VIP Menu

```cpp
currentPage = 5;
drawMENUUI();
```

---

# DYNO LOGS PAGE

Page ID

```cpp
22
```

Function

```cpp
drawDynoLogsPage()
```

Current Layout

```text
DYNO LOGS

RUN001
RUN002
RUN003

BACK
```

---

# RUN DETAIL PAGE

Page ID

```cpp
23
```

Function

```cpp
drawRun1Detail()
```

Layout

```text
RUN001

MAX RPM : xxxx

TIME : xx.x

DELETE
BACK
```

---

# Delete Logic

```cpp
run1RPM = 0;
run1Time = 0;
```

---

# Future Features

## LittleFS Logging

Replace RAM storage

Current

```cpp
run1RPM
run1Time
```

Future

```cpp
LittleFS
```

Requirements

* Save after power off
* Save multiple runs
* Load automatically at boot

---

## Screenshot Save

Future Feature

Save current TFT screen

Store as

```text
SCREEN001
SCREEN002
SCREEN003
```

Using

* SD Card
  or
* LittleFS

---

## GPS Page

Future Feature

Display

```text
GPS SPEED

Current Speed

Max Speed

Trip Distance
```

---

## Power Calculation

Future Feature

Estimate

```text
Horsepower (HP)

Torque (Nm)
```

Using

* RPM
* GPS Speed
* Gear Ratio

Approximation only

No vehicle weight used

---

# Coding Rules

Do NOT modify

* Boot Logo
* License System
* ECU Communication

Focus only on

* DYNO
* LOGS
* GPS
* LittleFS

---

# Current Status

Completed

* START
* SAVE
* LOGS
* BACK
* DYNO PAGE
* LOGS PAGE

In Progressแ

* RUN001 Detail Page
* DELETE Function

Planned

* LittleFS
* Multi Run Storage
* Screenshot Save
* GPS Logger
* Dyno Graph
