# 📡 MQTT Reliability, QoS & Retained Messages (Exercise 5)

## 📖 Overview
This lab demonstrates how MQTT ensures reliable communication using different Quality of Service (QoS) levels and the Retain flag. Postman is used as the publisher, Node-RED as the subscriber, and Wireshark is used to analyze the network packets and observe protocol behavior.

---

## 🎯 Objectives
- Configure Node-RED as an MQTT subscriber  
- Publish messages using Postman  
- Analyze MQTT packets using Wireshark  
- Understand QoS 0, QoS 1, and QoS 2  
- Explore retained messages for state persistence  

---

## 🛠️ Tools Used
- Postman (MQTT Publisher)  
- Node-RED (Subscriber)  
- MQTT Broker (EMQX / Mosquitto)  
- Wireshark (Packet Analyzer)  

---

## 🔁 Architecture
Postman → MQTT Broker → Node-RED

---

## ⚙️ Setup

### 1. Node-RED Configuration
- Add mqtt in node  
- Set Broker: localhost:1883  
- Topic: campus/qos/test  
- QoS: 2  
- Connect to debug node  
- Click Deploy  

---

### 2. Wireshark Setup
- Select Loopback adapter  
- Apply filter:
  tcp.port == 1883  
  or  
  mqtt  

---

### 3. Postman MQTT Setup
- Connect to:
  mqtt://localhost:1883  
- Go to Publish tab  

---

## 📊 Results & Analysis

### 🔹 QoS 0 (At Most Once)
Payload:
{"test": "QoS 0 execution"}

Packets observed:
- 1 MQTT Publish  

Explanation:
No acknowledgment is sent. This is a fast "fire and forget" transmission with the lowest overhead.

---

### 🔹 QoS 1 (At Least Once)
Payload:
{"test": "QoS 1 execution"}

Packets observed:
- MQTT Publish  
- MQTT PUBACK  

Explanation:
The broker sends a PUBACK to confirm receipt. This ensures the message is delivered at least once but may cause duplicates.

---

### 🔹 QoS 2 (Exactly Once)
Payload:
{"test": "QoS 2 execution"}

Packets observed:
- MQTT Publish  
- MQTT PUBREC  
- MQTT PUBREL  
- MQTT PUBCOMP  

Explanation:
A four-step handshake ensures the message is delivered exactly once with no duplication, but it introduces the highest overhead.

---

## 📌 Retained Message

Payload:
{"status": "OPEN"}  
Topic:
campus/state/door  

Retain: ON  

Explanation:
When the Retain flag is enabled, the broker stores the last message. Any new subscriber will instantly receive this message upon connection.

Wireshark observation:
- MQTT Publish Message  
- Fixed Header → Retain: 1  

---

## 🧠 Key Learnings
- QoS controls reliability vs network overhead  
- QoS 0 = fastest, QoS 2 = most reliable  
- TCP ensures packet delivery, MQTT QoS ensures message delivery  
- Retained messages allow late subscribers to receive the latest data instantly  

---

## 📌 Conclusion
This experiment shows how MQTT balances performance and reliability using QoS levels. It also demonstrates how retained messages improve system design by maintaining the latest system state for new subscribers.

---
