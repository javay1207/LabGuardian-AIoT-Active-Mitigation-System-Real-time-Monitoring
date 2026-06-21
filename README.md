# LabGuardian AIoT Active Mitigation System: Real-time Monitoring

## 專案簡介 (Introduction)
The LabGuardian project is designed to address the safety monitoring needs of school laboratories. Using the Raspberry Pi 5 as a high-performance central gateway, the system tracks ambient temperature and humidity levels in real-time to prevent equipment overheating or fire hazards. 

While traditional systems rely on audible alarms or simple notifications that may be ignored when a laboratory is unattended, this design utilizes the high computational power of Raspberry Pi 5 to bridge edge computing with high-torque mechatronics. The primary contribution is the development of an automated, soft-hard integrated mechanism capable of physically 'yanking' the power plug from the socket during a critical overheat event. 

## 目標使用者與應用場景 (Target Users & Usage Scenario)
* **Campus Lab Managers:** Who need to oversee multiple lab rooms.
* **Engineering Students:** Who leave experimental setups running for long periods.

**情境範例：** A student is running a stress test on a server rack inside the Smart Campus lab. 
1. The room's air conditioning fails during the weekend.
2. The server rack temperature begins to rise rapidly, reaching a dangerous threshold.
3. The Raspberry Pi 5 detects the temperature spike through its sensors.
4. It immediately publishes an alert message via MQTT. The Node-RED dashboard turns red and triggers an on-screen notification, allowing the duty manager to intervene before the hardware is damaged.

## 核心功能特色 (Key Features)
* **即時數據感測 (Real-time Sensing):** A Python script on the Raspberry Pi 5 reads sensor data every few seconds.
* **主動式物理防禦 (Active Physical Mitigation):** If temperature exceeds 40°C, it automatically triggers a "Warning" logic node. 系統會觸發伺服馬達 (Servo Motor) 轉動，強制拔除電源插頭 (physically 'yanking' the power plug)，並點亮 LED 警示燈.
* **視覺化儀表板 (Visualization):** 透過 MQTT 通訊協定，Node-RED subscribes to the MQTT data. It processes the values and displays them on a Gauges/Charts dashboard.
* **歷史數據分析 (Data Analysis):** Our goal is to use the SQLite database to calculate average temperature and humidity levels over specific periods. 程式內建移動平均 (Moving Average) 演算法，讀取最近 20 筆資料計算均值，以消弭感測器偶發波動。

## 系統架構與環境需求 (System Architecture & Requirements)

### 硬體設備 (Hardware Composition)
* **Primary Platform:** Raspberry Pi 5
* **Sensors:** DHT11/DHT22 (Temperature & Humidity Sensor) (接腳：GPIO 23)
* **Indicators:** Active Buzzer or LED for local visual/audio alerts (LED 接腳：GPIO 16)
* **Actuator:** High-Torque Actuator (Servo/Motor) (Servo 接腳：GPIO 18)

### 軟體技術 (Software Workflow)
* **作業系統：** Raspberry Pi OS
* **程式語言：** Python 3 (使用 `gpiozero`, `adafruit_dht`, `paho-mqtt`, `sqlite3` 等套件)
* **通訊協定：** MQTT (Mosquitto Broker 架設於 Pi 本機)
* **資料庫：** SQLite3 (儲存欄位包含：id, timestamp, temperature, humidity, status)
* **前端視覺化：** Node-RED

## 快速開始 (Quick Start)

**1. 系統更新與環境準備**
確保你的 Raspberry Pi 已經安裝 Python 3 並且啟動了 Mosquitto 與 Node-RED 服務：
```bash
sudo apt-get update
sudo apt-get install -y mosquitto mosquitto-clients
```
