# WASTE-MANAGEMENT
PROJECT 1 : WASTE MANAGEMENT
Project 1: Waste Management System

Overview

This project is an AI-powered Waste Segregation System that classifies waste into different categories such as biodegradable, non-biodegradable, recyclable, plastic, electronic, and more. The system integrates sensor-based data collection, a waste segregation mechanism, and cleaning automation for real-time waste sorting.

Features

Image & Sensor-Based Waste Classification

Moisture sensors for wet/dry waste detection.

Infrared sensors to differentiate plastic, metal, organic, and glass.

RFID sensors to detect electronic waste.

Weight sensors to categorize waste based on mass.

Three-Level Waste Segregation

Level 1: Central Core

Houses all machinery, wiring, and sensors.

Waste is rotated using a DC motor.

Initial segregation based on sensor data.

Level 2: Side Bins (Roulette Mechanism)

Waste is redirected into respective bins through a window-like opening.

Higher-quality sensors refine segregation.

Incorrectly sorted waste is reanalyzed.

Level 3: Final Storage Bins (Vertical Segregation)

Fully classified waste is stored for disposal.

Compost generation for biodegradable waste.

Automated Cleaning System

Air Jet Blowers: Remove dry waste.

Spray Nozzles: Clean wet/biodegradable residue.

Rotating Wiper Blades: Maintain sensor accuracy.

Flask API Integration

Real-time sensor data retrieval.

Motor control for Level 1 rotation.

Cleaning mechanism activation.

Data storage & retrieval via SQLite.

API Endpoints

Endpoint

Method

Function

/get_sensors

GET

Fetch real-time sensor data

/control_motor

POST

Start/stop Level 1 motor

/clean_system

POST

Activate cleaning system

/get_stored_data

GET

Retrieve past sensor data

Installation & Setup

Prerequisites

Python 3.x

Install dependencies:

pip install flask sqlite3

Running the API

Clone the repository:

git clone https://github.com/your-username/Project-1-Waste-Management.git
cd Project-1-Waste-Management

Run the Flask application:

python waste_segregation_api.py

Access the API at:

http://127.0.0.1:5000/

Deployment Options

Local Machine: For development and testing.

Raspberry Pi: For sensor-based real-world testing.

Cloud (AWS/GCP/Render): To make the API accessible globally.

Future Enhancements

Deep Learning Integration: Use AI models for image-based waste detection.

Mobile App Interface: To monitor and control segregation remotely.

IoT Connectivity: Real-time waste tracking via smart devices.

License

This project is open-source under the MIT License.
from flask import Flask, request, jsonify
import random  # Simulating sensor inputs
import sqlite3  # Database integration
import time
app = Flask(name)
Database setup
def init_db():
conn = sqlite3.connect("waste_system.db")
cursor = conn.cursor()
cursor.execute("""
CREATE TABLE IF NOT EXISTS sensor_data (
id INTEGER PRIMARY KEY AUTOINCREMENT,
moisture REAL,
infrared TEXT,
weight REAL,
rfid TEXT,
timestamp DATETIME DEFAULT CURRENT_TIMESTAMP
)
""")
conn.commit()
conn.close()

init_db()

Simulated sensor readings

def get_sensor_data():
data = {
"moisture": random.uniform(0, 100),  # Percentage
"infrared": random.choice(["plastic", "metal", "organic", "glass"]),
"weight": random.uniform(0, 500),  # Grams
"rfid": random.choice(["electronic", "non-electronic"])
}
save_sensor_data(data)
return data

Save sensor data to database

def save_sensor_data(data):
conn = sqlite3.connect("waste_system.db")
cursor = conn.cursor()
cursor.execute("""
INSERT INTO sensor_data (moisture, infrared, weight, rfid)
VALUES (?, ?, ?, ?)""", (data["moisture"], data["infrared"], data["weight"], data["rfid"]))
conn.commit()
conn.close()

API to get real-time sensor data

@app.route('/get_sensors', methods=['GET'])
def get_sensors():
return jsonify(get_sensor_data())

API to control DC motor (rotation of Level 1)

@app.route('/control_motor', methods=['POST'])
def control_motor():
data = request.json
if data.get("action") == "start":
return jsonify({"message": "Motor started"})
elif data.get("action") == "stop":
return jsonify({"message": "Motor stopped"})
return jsonify({"error": "Invalid action"}), 400

API to activate air jets / cleaning mechanisms

@app.route('/clean_system', methods=['POST'])
def clean_system():
data = request.json
mechanism = data.get("mechanism")
if mechanism in ["air_jet", "spray_nozzle", "wiper_blade"]:
return jsonify({"message": f"{mechanism} activated"})
return jsonify({"error": "Invalid cleaning mechanism"}), 400

API to retrieve stored sensor data

@app.route('/get_stored_data', methods=['GET'])
def get_stored_data():
conn = sqlite3.connect("waste_system.db")
cursor = conn.cursor()
cursor.execute("SELECT * FROM sensor_data ORDER BY timestamp DESC LIMIT 10")
data = cursor.fetchall()
conn.close()
return jsonify(data)

API for final waste segregation process

@app.route('/segregate_waste', methods=['POST'])
def segregate_waste():
data = get_sensor_data()
category = classify_waste(data)
return jsonify({"waste_category": category})

Waste classification logic

def classify_waste(data):
if data["moisture"] > 50 and data["infrared"] == "organic":
return "Biodegradable"
elif data["infrared"] == "plastic":
return "Plastic Waste"
elif data["infrared"] == "metal":
return "Metal Waste"
elif data["infrared"] == "glass":
return "Glass Waste"
elif data["rfid"] == "electronic":
return "Electronic Waste"
elif data["weight"] > 300:
return "Heavy Non-Biodegradable Waste"
else:
return "General Waste"

if name == 'main':
app.run(debug=True, host='0.0.0.0', port=5000)  # Running locally
