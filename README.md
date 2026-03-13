 # EXPERIMENT-04-INTERFACING-DIGITAL-SENSOR-DHT-11-Soil-Moisture-sensor--WITH-EDGE-DEVELOPMENT-BOARD-

---

### **NAME:KANNAN R**  
### **DEPARTMENT: AI&ML**  
### **ROLL NO: 212224230306**  
### **DATE OF EXPERIMENT: 25.02.2026**  

---

## **AIM:**  
To interface an **Temperature and humidity sensor (DHT 11) Soil Moisture Sensor (REES52)** with the **Raspberry Pi 4** and display the sensor readings using HiveMQ cloud.

---

## **APPARATUS REQUIRED:**  
1. Raspberry Pi 4  
2. Temperature and Humidity sensor (DHT-11) and Soil Moisture Sensor (REES52)  
3. Jumper Wires  
4. Breadboard  
5. USB Cable  
6. Computer with Thonny IDE  

---

## **THEORY:**  
<img width="1293" height="744" alt="image" src="https://github.com/user-attachments/assets/3c04afa6-1517-45d2-88f1-e671d9ed1ffb" />

 ### FIGURE-01 RASPI PI 4 PINOUT DIAGRAM: 

The Raspberry Pi 4 Model B is built around a Broadcom BCM2711 system-on-chip that integrates a quad-core ARM Cortex-A72 (64-bit) CPU, VideoCore VI GPU, memory controller, and peripheral interfaces, forming a compact yet complete computer architecture where the SoC connects internally to RAM, USB 3.0 controller, Gigabit Ethernet, HDMI display, and wireless modules. Its 40-pin GPIO header provides a flexible pin configuration consisting of power pins (5 V and 3.3 V), multiple ground pins, and general-purpose input/output pins that operate at 3.3 V logic and can be programmed for digital I/O or alternate functions. Key alternate functions include I²C (SDA, SCL) for sensor communication, SPI (MOSI, MISO, SCLK, CS) for high-speed peripheral interfacing, UART (TX, RX) for serial communication, and PWM for control applications.  For communication, I2C (SDA, SCL), SPI (MOSI, MISO, SCK), and UART (TX, RX) interfaces are mapped across different GPIO pins, allowing seamless connectivity with sensors and peripherals. All GPIO pins support PWM (Pulse Width Modulation), making it useful for motor control, LED brightness adjustment, and sound applications. The BOOTSEL button enables USB mass storage mode for firmware flashing, while the DEBUG pins (SWD interface) provide debugging capabilities. With its low power consumption, flexible GPIO options, and rich interface support, the Raspberry Pi Pico is widely used for IoT, embedded systems, robotics, and automation projects.This architecture and pin multiplexing allow the Raspberry Pi 4 to act as both a general-purpose computing platform and an embedded controller, supporting rapid prototyping, hardware interfacing, and IoT applications.
## Temperature and Humidity Sensor (DHT-11):
The DHT11 is a low-cost digital sensor used to measure ambient temperature and relative humidity in embedded and IoT applications. It integrates a thermistor for temperature sensing and a capacitive humidity sensor for detecting moisture levels in the air, along with an internal 8-bit microcontroller that processes the signals and provides calibrated digital output through a single-wire communication interface. The sensor operates typically at 3.3 V to 5 V, measures temperature in the range of 0 °C to 50 °C with ±2 °C accuracy, and humidity from 20% to 80% with ±5% accuracy. Due to its simple interface, low power consumption, and reliable performance, it is widely used in weather monitoring systems, home automation, agricultural monitoring, and basic environmental data acquisition projects.

<img width="1000" height="1000" alt="image" src="https://github.com/user-attachments/assets/5c8d35b5-4381-434f-8617-db4b8fe19154" />

### FIGURE-02 Temperature and Humidity Sensor (DHT-11)

## Soil Moisture Sensor (REES52):
The Soil Moisture Sensor (REES52) is a low-cost resistive-type sensor used to measure the volumetric water content present in soil. It operates on the principle that the electrical conductivity of soil changes with moisture level—wet soil conducts electricity better than dry soil due to the presence of water acting as a conductor between the probe electrodes. The module typically consists of two exposed metal probes and a control board with a comparator (often based on LM393), providing both analog output (for precise moisture level measurement) and digital output (for threshold-based detection). It operates at 3.3V–5V, making it compatible with microcontrollers such as Arduino and Raspberry Pi, and is widely used in irrigation control systems, smart agriculture, and automated plant watering applications.
<img width="800" height="800" alt="image" src="https://github.com/user-attachments/assets/32b40be6-1781-4459-9035-464b6064ec0f" />

 ### FIGURE-03 Soil Moisture Sensor (REES52) 

## Working Principle:
Experiment 4A
The Temperature and Humidity Sensor (DHT-11) OUT is connected to one of the GPIO pins of the Raspberry Pi 4.
The Python script sets the measure the real time temperature and Humidity output and shown in HiveMQcloud with current status and Console.
CIRCUIT DIAGRAM
Connect the Vcc of the Temperature and Humidity Sensor (DHT-11) is connected to +5V in Raspberrry Pi4.
Connect the Gnd of the Temperature and Humidity Sensor (DHT-11) is connected to Gnd in Raspberrry Pi4.
Connect the OUT to any one GPIO.


Experiment 4B
The Soil Moisture Sensor (REES52) D0 is connected one of the GPIO pins in Raspberry Pi 4.
The Soil Moisture Sensor (REES52) A0 is connected one of the GPIO pins in Raspberry Pi 4.
The Python script sets the Soil Moisture Sensor (REES52) value based on the variation in the temparature and humdity (Dry or Wet) and shown in HiveMQ Cloud and console.
CIRCUIT DIAGRAM
Connect the Soil Moisture Sensor (REES52) Vcc to any +5V.
Connect the Soil Moisture Sensor (REES52) GND to any GND.
Connect the Soil Moisture Sensor (REES52) D0 to any one GPIO. 
Connect the Soil Moisture Sensor (REES52) A0 to any one GPIO. 

Experiment 4A
## PROGRAM (Python)
```


 import Adafruit_DHT
import paho.mqtt.client as mqtt
import ssl
import time

# ---------------- DHT11 Setup ----------------
DHT_SENSOR = Adafruit_DHT.DHT11
DHT_PIN = 18   # GPIO4

# ---------------- HiveMQ Cloud Credentials ----------------
MQTT_BROKER = "804bc14ead4b4ebe8f5bbb8d93874976.s1.eu.hivemq.cloud"
MQTT_PORT = 8883
MQTT_USER = "hivemq.webclient.1772095046804"
MQTT_PASSWORD = "0ut3P%f#jSZW,Lk1Y9!a"

TEMP_TOPIC = "raspberrypi/dht/temperature"
HUM_TOPIC = "raspberrypi/dht/humidity"

# ---------------- MQTT Setup ----------------
client = mqtt.Client()

client.username_pw_set(MQTT_USER, MQTT_PASSWORD)
client.tls_set(tls_version=ssl.PROTOCOL_TLS)
client.connect(MQTT_BROKER, MQTT_PORT)

print("Connected to HiveMQ Cloud")
print("Reading DHT11 Sensor...\n")

# ---------------- Main Loop ----------------
while True:
    humidity, temperature = Adafruit_DHT.read(DHT_SENSOR, DHT_PIN)

    if humidity is not None and temperature is not None:
        print(f"Temperature = {temperature} °C")
        print(f"Humidity    = {humidity} %")
        print("---------------------------")

        # Publish to HiveMQ
        client.publish(TEMP_TOPIC, temperature)
        client.publish(HUM_TOPIC, humidity)

        print("Data sent to HiveMQ\n")

    else:
        print("Sensor failure. Check wiring.")

    time.sleep(10)



 
````

### OUPUT  
## Experiment 4A
<img width="1200" height="1600" alt="image" src="https://github.com/user-attachments/assets/4bae88e5-20dc-420a-8172-d41871c3aec8" />
<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/3be02909-9a89-42d2-a8eb-225c61046f7c" />

<img width="1919" height="910" alt="image" src="https://github.com/user-attachments/assets/76f3c06c-200c-4295-a958-6a28607964db" />

# Experiment 4B
## PROGRAM (Python)
```
import RPi.GPIO as GPIO
import time
import paho.mqtt.client as mqtt
import json

# ---------------- GPIO Setup ----------------
DIGITAL_PIN = 24   # D0 connected to GPIO 23

GPIO.setmode(GPIO.BCM)
GPIO.setup(DIGITAL_PIN, GPIO.IN)

# ---------------- MQTT Setup ----------------
broker = "804bc14ead4b4ebe8f5bbb8d93874976.s1.eu.hivemq.cloud"
port = 8883
topic = "Soil Moisture"

username = "hivemq.webclient.1772180523263"
password = "3H2y:POu!hja*EG9>r4V"

client = mqtt.Client()
client.username_pw_set(username, password)
client.tls_set()  # Required for HiveMQ Cloud (TLS)

client.connect(broker, port)
client.loop_start()

print("Connected to HiveMQ Cloud")

# ---------------- Main Loop ----------------
try:
    while True:
        digital_value = GPIO.input(DIGITAL_PIN)

        # Convert Digital Value
        if digital_value == 0:
            moisture_status = "WET"
            humidity_value = 100
        else:
            moisture_status = "DRY"
            humidity_value = 0

        payload = {
            "temperature": humidity_value,  # shown like temperature
            "humidity": moisture_status     # shown like humidity
        }

        client.publish(topic, json.dumps(payload))
        print("Published:", payload)

        time.sleep(2)

except KeyboardInterrupt:
    GPIO.cleanup()
    print("Program Stopped")

 



 
````

### OUPUT  
## Experiment 4B
<img width="1200" height="1600" alt="image" src="https://github.com/user-attachments/assets/3bcaa78a-b71e-43a0-8eac-882605f268cb" />



<img width="1600" height="1200" alt="image" src="https://github.com/user-attachments/assets/e7641391-8d97-48e7-ae85-da4fee04c5ae" />

<img width="1910" height="917" alt="image" src="https://github.com/user-attachments/assets/5b0f0d15-783a-45aa-bbac-7a79314968d6" />


## **RESULT:**  
The **Temperature and humidity sensor (DHT 11) Soil Moisture Sensor (REES52)** was successfully interfaced with the **Raspberry Pi 4**, and real-time **Temperature, Humidity and Soil Moisture level** were read and displayed in Console and HiveMq Cloud. 

---

