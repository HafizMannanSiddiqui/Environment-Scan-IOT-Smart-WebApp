# 🌍 EnviroScan: Air Quality Monitoring System

An IoT-enabled smart air quality monitoring solution using ESP8266, real-time sensor data, AI decision-making, and a React-based dashboard.

---

## 📌 Table of Contents
1. [Project Aims](#project-aims)
2. [Block Diagram](#block-diagram)
3. [MQTT Topics & Broker Details](#mqtt-topics--broker-details)
4. [Cloud AI Model](#cloud-ai-model)
5. [Execution Instructions](#execution-instructions)
6. [Web Dashboard User Guide](#web-dashboard-user-guide)

---

## 1️⃣ Project Aims

The primary goal of **EnviroScan** is to design and implement an IoT-based air quality monitoring system that:

- Collects data from temperature, humidity, PM2.5, and CO2 sensors via an ESP8266 board.
- Transmits sensor data to the cloud using MQTT over WiFi.
- Uses a pretrained AI model to detect dangerous levels and anomalies.
- Automatically triggers actuators like fans or purifiers.
- Offers a user-friendly dashboard for real-time monitoring and manual override.
- Maintains modular, cost-effective architecture for easy deployment in homes or small industries.

---

## 2️⃣ Block Diagram

```

\[Insert Block Diagram Image Here: e.g., images/block\_diagram.png]

````

> 📌 **Figure 1**: Architecture of the EnviroScan Sensor Board.

---

## 3️⃣ MQTT Topics & Broker Details

### 🔐 Broker Configuration

- **Broker URL**: `4547d51320d54e74be02bb04e3c1b342.s1.eu.hivemq.cloud`
- **Port**: `8883 (TLS)`
- **Cluster ID**: `4547d51320d54e74be02bb04e3c1b342`
- **Plan**: Serverless
- **Current Usage**: 5/100 sessions, 74.03 KB/10 GB

### 👤 User Credentials

#### `envscan` (ESP8266 Sensor Board)
- **Username**: `envscan`
- **Password**: `Abcd1234`
- **Role**: Publish sensor data, subscribe to actuator commands.

```text
Publish: enviroscan/temp, enviroscan/humidity, enviroscan/pm25, enviroscan/co2  
Subscribe: enviroscan/fan
````

#### `ai user` (AI Script)

* **Role**: Analyze sensor data and publish predictions.
* **Permissions**:

```text
Subscribe: enviroscan/temp, enviroscan/humidity, enviroscan/pm25, enviroscan/co2  
Publish: enviroscan/fan, enviroscan/accuracy
```

#### `web user` (Web Dashboard)

* **Role**: Monitor data and manually control fan.
* **Permissions**:

```text
Subscribe: enviroscan/temp, enviroscan/humidity, enviroscan/pm25, enviroscan/co2, enviroscan/fan, enviroscan/accuracy  
Publish: enviroscan/fan (manual override)
```

### 📡 MQTT Topics Overview

| Topic                 | Description                    |
| --------------------- | ------------------------------ |
| `enviroscan/temp`     | Temperature (°C) from DHT22    |
| `enviroscan/humidity` | Humidity (%) from DHT22        |
| `enviroscan/pm25`     | PM2.5 (μg/m³) from dust sensor |
| `enviroscan/co2`      | CO2 level (ppm) from MQ135     |
| `enviroscan/fan`      | Fan control (ON/OFF)           |
| `enviroscan/accuracy` | AI model accuracy              |

---

## 4️⃣ Cloud AI Model

### 🧠 Model Info

* **Type**: Decision Tree Classifier (scikit-learn)
* **Features**: Temperature, Humidity, PM2.5, CO2
* **Output**: Fan ON if PM2.5 > 100 or CO2 > 700, else OFF

### 🧪 Training Data Summary

| Feature     | Distribution                      |
| ----------- | --------------------------------- |
| Temperature | Normal (mean 25°C, std 5°C)       |
| Humidity    | Normal (mean 50%, std 10%)        |
| PM2.5       | Exponential (mean 50 μg/m³)       |
| CO2         | Normal (mean 60 ppm, std 200 ppm) |

* **Preprocessing**: Mean Imputation + StandardScaler
* **Accuracy**: `98%`, Precision/Recall > `0.97`

### ⚙️ Deployment

* **Platform**: Windows 10 + Miniconda
* **Script**: `decision_tree_model.py`
* **Broker**: HiveMQ Cloud (same as above)
* **Interval**: Every 10s

---

## 5️⃣ Execution Instructions

### Install Required Libraries:

```bash
pip install paho-mqtt scikit-learn numpy
```

### Run AI Model Script:

```bash
python decision_tree_model.py
# or full path if needed:
C:\Users\usman\IoT-Project\decision_tree_model.py
```

---

## 6️⃣ Web Dashboard User Guide

### 🖥️ Features

* Real-time sensor display
* AI-based fan control
* Manual override button
* Responsive UI (Tailwind CSS)

### 🚀 Setup Instructions

1. Ensure Node.js (v16+) is installed.
2. Navigate to the project:

```bash
cd C:\Users\usman\IoT-Project\enviroscan-web
```

3. Install dependencies:

```bash
npm install
```

4. Start the web app:

```bash
npm start
```

5. Open in browser:

```
http://localhost:3000
```

### 📡 Web App MQTT Config

| Parameter         | Value                                                 |
| ----------------- | ----------------------------------------------------- |
| Broker            | `4547d51320d54e74be02bb04e3c1b342.s1.eu.hivemq.cloud` |
| Port              | `8883`                                                |
| Username          | `web user`                                            |
| Password          | \[Set in HiveMQ]                                      |
| Subscribed Topics | temp, humidity, pm25, co2, fan, accuracy              |
| Published Topic   | `enviroscan/fan` (manual override)                    |

---

### 🧩 Usage

* Monitor temperature, humidity, PM2.5, and CO2 in real-time.
* Check fan status and AI prediction accuracy.
* Toggle fan manually with the ON/OFF button.

---

### ❗ Troubleshooting

* **No Data?** Check MQTT credentials in `App.js`
* **Stuck Connections?** Review HiveMQ Cloud session limit (5/100)
* **AI/ESP Not Working?** Ensure both are running and connected to MQTT.

---

## 👤 Authors

* Hafiz Mannan Siddiqui ([@HafizMannanSiddiqui](https://github.com/HafizMannanSiddiqui))
* Usman Amjad
