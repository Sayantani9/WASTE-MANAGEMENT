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
