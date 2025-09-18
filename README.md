# FENCE-SENTINEL
---
# 🛡️ FenceSentinel: Smart Fence Monitoring System

**FenceSentinel** is an **ESP32-based IoT project** designed to **detect and prevent unauthorized use of electric fences**.  
It monitors **current and voltage** on the fence line, detects **tampering**, and sends **real-time alerts** via **LoRa communication** with optional **GPS location tagging**.

---

##  Features
- **Fence Tap Detection** – Detects unauthorized load connected to fence using **ACS712 current sensor** and a **voltage divider**.  
- **Tamper Detection** – Push-button tamper switch to detect device enclosure opening.  
- **Buzzer Alert** – Local audible alert when unauthorized activity is detected.  
- **LoRa Transmission** – Sends alerts to a central receiver up to **5–10 km** in rural areas.  
- **GPS Module (NEO-6M)** – Adds location & timestamp to alerts for multi-node deployments.  
- **Manual Reset Button** – Reset alarm state after tampering/tapping events.  

---

##  Hardware Used
- **ESP32 DevKitC (ESP-32S)** – Main controller  
- **ACS712 Current Sensor (20A module)** – Current monitoring  
- **Voltage Divider (33kΩ + 10kΩ)** – Fence voltage measurement  
- **LoRa SX1278 Module (Ra-02)** – Long-range wireless alerts  
- **NEO-6M GPS Module** – Location and time tagging  
- **Buzzer + 2N2222 Transistor** – Local alert mechanism  
- **Tamper Button (push switch)** – Unauthorized box opening detection  
- **Manual Reset Button** – Alarm reset control  
- **LM2596 Buck Converter (12V → 3.3V)** – Power regulation  
- **12V Li-ion Battery (3S, 11.1V nominal)** – Power 

##  Working Principle
1. The **battery (12V)** powers the fence wire.  
2. **ACS712** measures current flow through the fence.  
3. A **voltage divider** scales fence voltage to ESP32 ADC range.  
4. If **tapping is detected** (current/voltage abnormality) or **tampering occurs**, ESP32:  
   - Activates the **buzzer**  
   - Sends an **alert packet over LoRa**  
   - Includes **GPS coordinates** if available  
5. A **manual reset button** clears the alarm state
   


##  Applications
- Rural **electric fence security**  
- **Farm boundary monitoring**  
- **Theft/tamper prevention** in remote areas  
- Scalable to **multi-node fence networks**  

---

##  Future Scope
- 🔄 Add **cloud backend** for data logging & alerts (MQTT/HTTP).  
- 📱 Mobile app integration for **real-time farmer notifications**.  
- 🔋 Solar panel integration for **self-sustaining power**.  
- 📍 Multi-node GPS coordination to **pinpoint tamper location**.  

---

# 👨‍💻 Contributors
-  ## 👨‍💻 Contributors

- Chirugudu Jayanth Kumar –Hardware Deveploment & system integration
- Chittiboina Venkata Naga Poojitha – Circuit Design & Testing  
- Chinthala Muni Jaswanth Kumar – Embedded Programming & Debugging
- Skandan C- circuit schematic and Research
- Harsha Vardhan-PPT design, presentation & project pitching  
- Rajesh S – Documentation
