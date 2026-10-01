# UNIT 4: INTERNET OF THINGS (IoT)
## Complete Comprehensive Student Study Guide & Lab Manual
**Target Audience:** BCA / BSc (Computer Science / IT) / B.Tech Students  
**Subject:** Internet of Things (Unit 4: Physical Devices, Raspberry Pi, Python & Projects)

---

# CHAPTER 1: IoT PHYSICAL DEVICES AND ENDPOINTS

### 1.1 What is the Internet of Things (IoT)?
The **Internet of Things (IoT)** refers to an interconnected mesh of physical objects ("things") embedded with sensors, software, processing units, and communication hardware that enable them to sense the environment, process data, and exchange information with other systems over the internet or local networks without continuous human intervention.

### 1.2 IoT Physical Devices
An **IoT Physical Device** is an actual electronic hardware unit that interfaces with the physical environment. Unlike traditional computers that interact primarily with humans through keyboards and displays, an IoT device interacts directly with physical phenomena:
* Measures physical variables (e.g., temperature, humidity, vibration, distance, sound).
* Converts physical states into digital signals.
* Performs physical actions (e.g., triggering relays, blinking LEDs, rotating motors).
* Has communication capabilities (Wi-Fi, Bluetooth, Ethernet, LoRa, Cellular).

### 1.3 IoT Endpoints
An **IoT Endpoint** is the terminal node located at the outermost boundary ("the edge") of an IoT network architecture:
* **Producer Endpoints:** Devices that generate and transmit telemetry data (e.g., weather station sensor node, soil moisture probe, seismic monitor).
* **Consumer Endpoints:** Devices that receive network commands and execute physical actions (e.g., smart water valve, irrigation pump, digital indicator).
* **Bi-directional Endpoints:** Devices that sense, process locally, and also receive commands to actuate (e.g., smart thermostat, industrial robot controller, smart edge gateway).

### 1.4 The 4-Stage IoT Data Pipeline (SPCA Model)
Every IoT implementation follows this fundamental pipeline:
1. **SENSE (Physical Layer):** Sensors detect physical stimuli and convert them into electrical voltages.
2. **PROCESS (Edge Compute):** A microprocessor (Raspberry Pi) or microcontroller samples, filters, and analyzes the data.
3. **COMMUNICATE (Network Layer):** Built-in wireless radios (Wi-Fi 802.11) packetize and route the data.
4. **ACT / STORE (Actuation & Cloud):** Actuators respond (LEDs blink, alarms sound) and data is permanently stored in a database (MySQL).

---

# CHAPTER 2: BUILDING BLOCKS OF AN IoT DEVICE

An IoT device consists of **seven fundamental building blocks**:

1. **Power Supply Unit (PSU):**
   * Supplies regulated DC power (3.3V or 5.0V). Raspberry Pi 4 requires a stable 5V / 3A (15W) USB-C supply.
   * Insufficient power causes brownouts, erratic sensor readings, and filesystem corruption on the MicroSD card.
2. **Processing Unit (Brain):**
   * **Microcontrollers (MCUs):** Arduino (ATmega328P), ESP32. Self-contained on a single chip, low clock speed (16-240 MHz), executes bare-metal C code without an OS.
   * **Microprocessors / SBCs:** Raspberry Pi (ARM Cortex-A72 @ 1.5 GHz). Requires external RAM, storage, and runs a complete Linux operating system.
3. **Memory & Storage:**
   * **RAM (Volatile):** 2GB/4GB/8GB LPDDR4 on Raspberry Pi 4 for running the OS kernel, Python scripts, and database buffers.
   * **Storage (Non-Volatile):** MicroSD card or USB SSD hosting the root filesystem, code scripts, and MySQL database files.
4. **Sensing Layer:**
   * **Digital Sensors:** Produce binary high/low pulses or serial packets (DHT11, HC-SR04, SW-420, PIR). Directly readable by GPIO.
   * **Analog Sensors:** Produce continuous variable voltage (LDR, Potentiometer, Thermistor, MQ-2 Gas). Require an external ADC (e.g., MCP3008 via SPI or ADS1115 via I2C) to connect to Raspberry Pi.
5. **Actuation Layer:**
   * Converts software output to physical actions. Because Raspberry Pi GPIO pins can only deliver 16mA at 3.3V, high-current actuators require driver circuits (transistors, optocoupled relays, motor driver H-bridges).
6. **Communication Layer:**
   * Connects the device to LAN or WAN: Wi-Fi, Ethernet, Bluetooth LE, Zigbee, LoRa, or Cellular NB-IoT.
7. **Software / Application Layer:**
   * Operating system (Raspberry Pi OS Linux), hardware access libraries (`RPi.GPIO`, `gpiozero`), database engines (MariaDB/MySQL), application scripts (Python), and communication protocols (HTTP, MQTT, SMTP).

---

# CHAPTER 3: RASPBERRY PI HARDWARE OVERVIEW

### 3.1 What is Raspberry Pi?
Raspberry Pi is a series of low-cost, credit-card-sized Single Board Computers (SBC) developed by the Raspberry Pi Foundation (UK). It functions as a complete desktop and server computer with a 64-bit ARM processor, multi-gigabyte RAM, display outputs, USB ports, networking, and a 40-pin hardware expansion header.

### 3.2 Raspberry Pi 4 Model B Specifications
* **SoC:** Broadcom BCM2711, Quad-core Cortex-A72 (ARM v8) 64-bit SoC @ 1.5 GHz.
* **RAM:** 2GB, 4GB, or 8GB LPDDR4-3200 SDRAM.
* **Wireless:** 2.4 GHz and 5.0 GHz IEEE 802.11ac Wi-Fi, Bluetooth 5.0, BLE.
* **Ethernet:** True Gigabit Ethernet (1000 Mbps).
* **USB:** 2 x USB 3.0 (up to 5 Gbps), 2 x USB 2.0.
* **Video Output:** 2 x Micro-HDMI ports (supporting dual displays up to 4Kp60).
* **Camera/Display:** 2-lane MIPI CSI camera port, 2-lane MIPI DSI display port.
* **GPIO:** Standard 40-pin header.
* **Power:** USB-C (5V / 3.0A).

### 3.3 Microcontroller vs Single Board Computer (Summary Table)
* **Arduino / ESP32:** Real-time deterministic execution, sub-microsecond pin response, native analog inputs, ultra-low power (< 50mA), no operating system.
* **Raspberry Pi:** Full Linux OS, multi-threading, network server, database host, complex Python libraries, requires 500-1500mA, lacks native analog pins.

---

# CHAPTER 4: LINUX ON RASPBERRY PI & COMMAND-LINE ESSENTIALS

### 4.1 Raspberry Pi OS
Raspberry Pi runs **Raspberry Pi OS** (a tailored version of Debian Linux).
* **Headless Setup:** Operating without monitor/keyboard via SSH (`ssh pi@raspberrypi.local`).
* Pre-configured using the Raspberry Pi Imager gear icon (setting Wi-Fi credentials and enabling SSH).

### 4.2 Key Linux Commands for IoT Students
* **File Operations:** `pwd` (current path), `ls -la` (list files with details), `cd <dir>` (change directory), `mkdir <folder>` (create directory), `cp src dest` (copy), `mv src dest` (move/rename), `rm file` (delete), `nano file` (text editor), `cat file` (view file contents).
* **System Monitoring:** `htop` (live CPU/RAM process monitor), `free -h` (memory usage), `df -h` (disk space), `vcgencmd measure_temp` (CPU temperature).
* **Package Management:**
  ```bash
  sudo apt update && sudo apt upgrade -y
  sudo apt install -y python3-pip git
  pip3 install RPi.GPIO gpiozero mysql-connector-python Adafruit-DHT
  ```
* **Service Management:** `sudo systemctl start/stop/status/enable <service>`
* **Automation with Cron:** `crontab -e` allows scheduling automated scripts at boot (`@reboot`) or periodic intervals (`*/5 * * * *`).

---

# CHAPTER 5: RASPBERRY PI HARDWARE INTERFACES

### 5.1 The 40-Pin GPIO Header
* **Power Pins:** Two 5V pins (Pins 2, 4), two 3.3V pins (Pins 1, 17), and eight Ground pins (Pins 6, 9, 14, 20, 25, 30, 34, 39).
* **Logic Pins:** 26 General Purpose I/O pins running strictly at **3.3V logic level**.

> **CRITICAL HARDWARE WARNING:** Never supply 5V to any Raspberry Pi GPIO pin directly. Doing so destroys the SoC. Always use a voltage divider or logic level converter.

### 5.2 Interface Protocols
1. **Digital GPIO:** Basic binary HIGH (3.3V) or LOW (0V). Pins can be configured as INPUT (with optional internal Pull-Up or Pull-Down resistors) or OUTPUT.
2. **PWM (Pulse Width Modulation):** Simulates variable analog voltage by rapidly switching a digital pin between HIGH and LOW.
   Duty Cycle (%) = (Ton / Tperiod) * 100
   Used for dimming LEDs, controlling DC motor speed via H-bridge drivers, and setting servo motor angles (50 Hz, 1-2ms pulse width).
3. **I2C (Inter-Integrated Circuit):**
   * 2 wires: **SDA** (GPIO 2, Pin 3) and **SCL** (GPIO 3, Pin 5).
   * Supports up to 127 devices on the same 2 wires using unique 7-bit addresses.
   * Devices: BME280, MPU6050, SSD1306 OLED, ADS1115 ADC.
   * Enabled via `sudo raspi-config` -> Interface Options -> I2C.
4. **SPI (Serial Peripheral Interface):**
   * 4 wires: **MOSI** (Pin 19), **MISO** (Pin 21), **SCLK** (Pin 23), **CE0/CE1** (Pins 24/26).
   * High-speed synchronous full-duplex communication (> 10 MHz).
   * Devices: MCP3008 ADC, RC522 RFID, TFT color displays.
5. **UART (Serial):**
   * 2 wires: **TXD** (GPIO 14, Pin 8) and **RXD** (GPIO 15, Pin 10).
   * Asynchronous point-to-point communication.
   * Devices: GPS modules (NEO-6M), GSM modems (SIM800L).

---

# CHAPTER 6: PYTHON PROGRAMMING ON RASPBERRY PI

### 6.1 Pin Numbering Conventions
* **`GPIO.BOARD`:** Physical pin positions 1 to 40 on the connector.
* **`GPIO.BCM`:** Broadcom channel numbers (GPIO 4, GPIO 17, etc.). Recommended standard.

### 6.2 Digital Output (LED Control)
```python
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BCM)
GPIO.setwarnings(False)
LED_PIN = 17
GPIO.setup(LED_PIN, GPIO.OUT)

# Turn ON and OFF
GPIO.output(LED_PIN, GPIO.HIGH) # 3.3V
time.sleep(1)
GPIO.output(LED_PIN, GPIO.LOW)  # 0V
GPIO.cleanup()
```

### 6.3 Digital Input (Pull-Up / Pull-Down Resistors)
When reading switches or sensors, floating pins pick up electromagnetic noise. Use internal resistors:
```python
# Internal Pull-Up: Pin is HIGH by default, goes LOW when pressed to GND
GPIO.setup(18, GPIO.IN, pull_up_down=GPIO.PUD_UP)

# Internal Pull-Down: Pin is LOW by default, goes HIGH when connected to 3.3V
GPIO.setup(18, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)
```

### 6.4 Interrupts (Event Detection) vs Polling
Polling wastes 100% of CPU in an infinite loop. Interrupt-driven event detection sleeps the CPU and executes a callback function only when a voltage transition occurs:
```python
def button_callback(channel):
    print("Button event detected!")

# Trigger on falling edge (HIGH -> LOW) with 200ms software debouncing
GPIO.add_event_detect(18, GPIO.FALLING, callback=button_callback, bouncetime=200)
```

---

# CHAPTER 7: PROJECT 1 — MEASURE DISTANCE USING ULTRASONIC SENSOR & LED BLINK

### 7.1 Principle of Operation (HC-SR04)
The HC-SR04 emits an 8-cycle ultrasonic sound burst at 40 kHz when triggered by a 10us pulse on the **TRIG** pin. The sound wave reflects off an obstacle and returns to the receiver. The **ECHO** pin remains HIGH for the exact duration of the round trip:
Speed of sound in air = 343 m/s = 34,300 cm/s
Distance (cm) = (Echo Duration (seconds) * 34,300) / 2

### 7.2 Safe Circuit Wiring (Voltage Divider)
Because the HC-SR04 ECHO pin outputs 5V, a voltage divider (R1 = 1k, R2 = 2k) is mandatory to protect the Raspberry Pi GPIO:
Vout = 5.0V * (2000 / (1000 + 2000)) = 3.33V

* **Connections:**
  * HC-SR04 VCC -> Pin 2 (5V)
  * HC-SR04 GND -> Pin 6 (GND)
  * HC-SR04 TRIG -> GPIO 23 (Pin 16)
  * HC-SR04 ECHO -> 1k resistor -> GPIO 24 (Pin 18) -> 2k resistor -> GND
  * LED Anode -> 330 Ohm resistor -> GPIO 17 (Pin 11); LED Cathode -> GND

### 7.3 Python Code
```python
import RPi.GPIO as GPIO
import time

TRIG = 23
ECHO = 24
LED  = 17

GPIO.setmode(GPIO.BCM)
GPIO.setup(TRIG, GPIO.OUT)
GPIO.setup(ECHO, GPIO.IN)
GPIO.setup(LED,  GPIO.OUT)

def measure():
    GPIO.output(TRIG, True)
    time.sleep(0.00001) # 10us
    GPIO.output(TRIG, False)
    
    start = time.time()
    stop = time.time()
    
    timeout = start + 0.05
    while GPIO.input(ECHO) == 0:
        start = time.time()
        if start > timeout: return -1
        
    timeout = stop + 0.05
    while GPIO.input(ECHO) == 1:
        stop = time.time()
        if stop > timeout: return -1
        
    duration = stop - start
    return round((duration * 34300) / 2, 2)

try:
    while True:
        d = measure()
        if d > 0:
            print(f"Distance: {d} cm")
            # Blink faster when closer
            if d <= 10:
                GPIO.output(LED, True); time.sleep(0.05)
                GPIO.output(LED, False); time.sleep(0.05)
            elif d <= 30:
                GPIO.output(LED, True); time.sleep(0.2)
                GPIO.output(LED, False); time.sleep(0.2)
            else:
                GPIO.output(LED, False); time.sleep(0.5)
        time.sleep(0.1)
except KeyboardInterrupt:
    GPIO.cleanup()
```

---

# CHAPTER 8: PROJECT 2 — DETECT THE VIBRATION OF AN OBJECT

### 8.1 Principle of Operation (SW-420)
The SW-420 contains a tiny conductive spring resting against a contact inside an insulated metal tube (Normally Closed). When stationary, the circuit is closed and the onboard LM393 comparator outputs LOW (0V). When mechanical vibration, shock, or tampering occurs, the spring momentarily disconnects, driving the digital output HIGH (3.3V).

### 8.2 Wiring
* SW-420 VCC -> Pin 1 (3.3V)
* SW-420 GND -> Pin 6 (GND)
* SW-420 DO -> GPIO 4 (Pin 7)
* Buzzer/Alarm LED -> GPIO 27 (Pin 13)

### 8.3 Python Code (Interrupt Driven)
```python
import RPi.GPIO as GPIO
import time
from datetime import datetime

VIB_PIN = 4
ALARM_PIN = 27

GPIO.setmode(GPIO.BCM)
GPIO.setup(VIB_PIN, GPIO.IN, pull_up_down=GPIO.PUD_DOWN)
GPIO.setup(ALARM_PIN, GPIO.OUT)

last_time = 0
count = 0

def vibration_detected(channel):
    global last_time, count
    now = time.time()
    if now - last_time > 1.5: # 1.5s cooldown debounce
        count += 1
        last_time = now
        ts = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        print(f"[ALERT #{count}] Vibration detected at {ts}!")
        GPIO.output(ALARM_PIN, True)
        time.sleep(0.2)
        GPIO.output(ALARM_PIN, False)

GPIO.add_event_detect(VIB_PIN, GPIO.RISING, callback=vibration_detected, bouncetime=100)

try:
    print("Vibration monitor active. Idling CPU...")
    while True:
        time.sleep(1)
except KeyboardInterrupt:
    GPIO.cleanup()
```

---

# CHAPTER 9: PROJECT 3 — CONNECT WITH AVAILABLE WI-FI

### 9.1 Wi-Fi Scanning & Configuration
* **Scanning:**
  ```bash
  sudo iwlist wlan0 scan | grep ESSID
  ```
* **Configuration via `wpa_supplicant.conf`:**
  ```bash
  sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
  ```
  ```ini
  country=IN
  ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
  update_config=1

  network={
      ssid="HomeWiFi_SSID"
      psk="SecretPassword"
      key_mgmt=WPA-PSK
  }
  ```
  Apply changes: `sudo wpa_cli -i wlan0 reconfigure`

### 9.2 Python Network Verification Script
```python
import subprocess
import urllib.request

def check_wifi():
    try:
        out = subprocess.check_output(["iwconfig", "wlan0"]).decode()
        ssid = out.split('ESSID:"')[1].split('"')[0] if 'ESSID:"' in out else "Unknown"
        return ssid
    except:
        return "None"

def check_internet():
    try:
        urllib.request.urlopen("https://www.google.com", timeout=3)
        return True
    except:
        return False

print("Connected SSID:", check_wifi())
print("Internet Online:", check_internet())
```

---

# CHAPTER 10: PROJECT 4 — TEMPERATURE NOTIFICATION SYSTEM

### 10.1 DHT11 Sensor Overview
The DHT11 measures temperature (0-50 C +- 2 C) and relative humidity (20-80% +- 5%) via a single-wire digital interface. It transmits a 40-bit packet containing integer and decimal bytes for humidity, temperature, and a verification checksum.

### 10.2 Wiring
* DHT11 VCC -> Pin 1 (3.3V)
* DHT11 GND -> Pin 9 (GND)
* DHT11 DATA -> GPIO 4 (Pin 7) with a 10k pull-up resistor to 3.3V

### 10.3 Python Script with Automated Email Alerts (SMTP)
```python
import Adafruit_DHT
import smtplib
from email.mime.text import MIMEText
import time

SENSOR = Adafruit_DHT.DHT11
PIN = 4
ALERT_TEMP = 32.0 # Celsius

def send_alert(t, h):
    msg = MIMEText(f"High Temperature Alert: {t} C, Humidity: {h}%")
    msg["Subject"] = "[IoT ALERT] Overheating Detected!"
    msg["From"] = "pi_alert@example.com"
    msg["To"] = "admin@example.com"
    
    try:
        server = smtplib.SMTP("smtp.gmail.com", 587)
        server.starttls()
        server.login("pi_alert@example.com", "app_password_here")
        server.send_message(msg)
        server.quit()
        print("Email alert sent successfully!")
    except Exception as e:
        print("Email failed:", e)

try:
    while True:
        humidity, temperature = Adafruit_DHT.read_retry(SENSOR, PIN)
        if temperature is not None:
            print(f"Temp: {temperature} C | Humidity: {humidity}%")
            if temperature >= ALERT_TEMP:
                send_alert(temperature, humidity)
        time.sleep(30)
except KeyboardInterrupt:
    pass
```

---

# CHAPTER 11: MySQL DATABASE INSTALLATION & PYTHON DATA LOGGING

### 11.1 Installation and Security Setup
```bash
# 1. Install MariaDB Server and Client
sudo apt update
sudo apt install -y mariadb-server mariadb-client

# 2. Enable and start service
sudo systemctl enable mariadb
sudo systemctl start mariadb

# 3. Secure installation
sudo mysql_secure_installation
```

### 11.2 Database & Table Creation (SQL)
```sql
CREATE DATABASE iot_telemetry_db;
CREATE USER 'iot_user'@'localhost' IDENTIFIED BY 'IoTPassword2025!';
GRANT ALL PRIVILEGES ON iot_telemetry_db.* TO 'iot_user'@'localhost';
FLUSH PRIVILEGES;

USE iot_telemetry_db;
CREATE TABLE sensor_readings (
    id INT AUTO_INCREMENT PRIMARY KEY,
    temperature DECIMAL(5,2) NOT NULL,
    humidity DECIMAL(5,2) NOT NULL,
    is_alert BOOLEAN DEFAULT 0,
    recorded_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 11.3 Python Integration (`mysql-connector-python`)
```python
import mysql.connector
import Adafruit_DHT
import time

conn = mysql.connector.connect(
    host="localhost",
    user="iot_user",
    password="IoTPassword2025!",
    database="iot_telemetry_db"
)
cursor = conn.cursor()

def log_data(t, h):
    alert = 1 if t >= 32.0 else 0
    sql = "INSERT INTO sensor_readings (temperature, humidity, is_alert) VALUES (%s, %s, %s)"
    cursor.execute(sql, (t, h, alert))
    conn.commit()
    print(f"Logged to MySQL: Temp={t} C, Hum={h}%, Alert={alert}")

try:
    while True:
        h, t = Adafruit_DHT.read_retry(Adafruit_DHT.DHT11, 4)
        if t is not None:
            log_data(t, h)
        time.sleep(10)
except KeyboardInterrupt:
    cursor.close()
    conn.close()
```

---

# CHAPTER 12: VIVA VOCE & EXAMINATION PREPARATION QUESTIONS

1. **What is an IoT physical device vs an endpoint?**  
   *An IoT physical device is any hardware unit containing sensors, computation, and communication. An endpoint is a boundary node at the network edge where digital computation directly meets physical reality by producing or consuming telemetry.*
2. **Why must we never connect 5V directly to a Raspberry Pi GPIO pin?**  
   *The Broadcom SoC silicon gates operate strictly at 3.3V logic levels. Connecting 5V exceeds the silicon dielectric breakdown limit, causing catastrophic permanent damage to the GPIO pin and SoC.*
3. **What is the difference between BCM and BOARD numbering?**  
   *BOARD refers to physical sequential pin positions (1-40) on the physical header. BCM refers to Broadcom silicon channel numbers (GPIO 4, GPIO 17). BCM is preferred in industry.*
4. **Why is interrupt-driven I/O better than polling?**  
   *Polling burns 100% of a CPU core in a continuous loop and risks missing rapid events. Interrupt-driven event detection sleeps the CPU at 0% usage and executes instantly via kernel callbacks upon voltage transitions.*
5. **How does an ultrasonic sensor calculate distance?**  
   *It emits a 40 kHz acoustic burst and measures the time of flight t until the echo returns. Distance = (t * 34,300 cm/s) / 2.*
6. **What is the role of pull-up/pull-down resistors?**  
   *They prevent input pins from entering a high-impedance floating state, holding the pin at a known voltage (3.3V or GND) when an external switch is open.*
7. **What is the difference between I2C and SPI?**  
   *I2C uses 2 wires (SDA, SCL), is half-duplex, uses 7-bit device addressing, and operates at 100-400 kbps. SPI uses 4 wires (MOSI, MISO, SCLK, CS), is full-duplex, uses dedicated chip-select lines, and operates at > 10 MHz.*
8. **What is PWM and what is duty cycle?**  
   *PWM simulates analog voltages using rapid digital pulses. Duty cycle is the percentage of time the signal is HIGH during one period (Ton / Ttotal * 100).*
9. **Why is MySQL used for IoT data instead of CSV?**  
   *MySQL provides transaction integrity (ACID), structured relational schemas, indexed high-speed queries, concurrent read/write access without file-locking conflicts, and aggregate analytics (AVG, MAX, MIN).*
10. **How do you make a Python IoT script run automatically on Raspberry Pi boot?**  
    *Using Linux cron (`crontab -e`) with the `@reboot` directive: `@reboot python3 /home/pi/iot/logger.py &` or by creating a `systemd` background service.*