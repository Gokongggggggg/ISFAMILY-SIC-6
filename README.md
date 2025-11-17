# Smart Sleep & Energy Lamp (Samsung Innovation Campus Side Project)

This project is a "Smart Lamp System" prototype developed as part of the **Samsung Innovation Campus (SIC)** competition. It is designed to improve a user's sleep quality (sleep-tech) while optimizing in-room energy efficiency.

## 🏅 Achievement & Appreciation

* **Samsung Innovation Campus:** This project successfully passed the selection phase, placing it in the **Top 1000 out of 10,000 participants** (Top 10%) nationally.
* **Mentor Appreciation:** The prototype also received positive feedback and appreciation from our Samsung Innovation Campus mentor, validating its functionality and design.

![Proof of Mentor Appreciation](/ASSET/Samsung-Mentor.png)

---

## 👥 The Team

* **Wisely Janson Halim**
* **Ethan Christian Theron**
* **Dominicius Francis Ang Gunadi**
* **Finley Septhian Dharma**

---

## 🎯 Purpose & Key Features

The system has two primary goals: helping the user sleep better and saving electricity. This is achieved through three main features governed by time (NTP) and sensors.

### 💤 1. Sleep Mode (22:00 - 06:00)
* **Red Light ON:** The red lamp turns on automatically.
* **Purpose:** Red light is scientifically shown to help the body continue producing melatonin, creating a relaxing atmosphere and improving sleep quality.

### ⏰ 2. Smart Alarm (06:00 Sharp)
* **Buzzer ON:** A buzzer sounds as an alarm for 15 minutes.
* **Snooze Feature:** The buzzer can be manually disabled by pressing and holding the physical button for 5-7 seconds.

### ☀️ 3. Energy Saver Mode (06:01 - 21:59)
* **Yellow Light (Smart Logic):** The yellow lamp will only turn on if **both** of these conditions are met:
    1.  The **LDR** sensor detects the room is **Dark**.
    2.  The **PIR** sensor detects **Motion**.
* **Efficiency Logic:** If the room is dark but empty (no motion), the light stays off to save energy. If the room is already bright, the light also stays off.

---

## 📊 System Architecture & Data Flow

This project is split into two main components: the hardware (ESP32) and a backend API (Flask), with a dual data-sending architecture.

1.  **Device (ESP32 / `main.py`):**
    * Connects to Wi-Fi.
    * Syncs global time via **NTP** (for local timezone, UTC+7).
    * Reads the **LDR** (darkness) and **PIR** (motion) sensors.
    * Executes the control logic for the lamps (Red/Yellow) and alarm (Buzzer).
    * Displays all real-time status to the **OLED Display**.
    * Sends **numeric data** (LDR, lamp status) to **Ubidots** for dashboards & graphs.
    * Sends **descriptive text data** (e.g., "Room Dark | Motion: YES") to the local Flask API.

2.  **Backend (Flask / `app.py`):**
    * Provides a local network API endpoint (`/sensor1`).
    * Receives JSON status data from the ESP32.
    * Stores these text-based status logs in a **MongoDB Atlas** (cloud) database.
    * Provides a `GET` endpoint to view all historical logs.

---

## 🛠️ Tech Stack

### 1. Hardware
* ESP32
* Light Dependent Resistor (LDR)
* PIR Motion Sensor
* OLED Display (SSD1306 I2C)
* Red & Yellow LEDs
* Buzzer
* Push Button

### 2. Software & Cloud
* **Device:** MicroPython
    * `urequests` (for HTTP POST)
    * `ntptime` (for time synchronization)
    * `ssd1306` (for OLED)
* **Backend:** Python
    * `Flask` (for the API server)
    * `PyMongo` & `Certifi` (for database connection)
* **Database:** MongoDB Atlas (Cloud)
* **IoT Platform:** Ubidots (Cloud, for dashboarding)