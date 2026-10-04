# 🤖 Autonomous Box Detection and Stacking Robot

An autonomous robotic system that uses **OpenCV** for real-time box detection, **MQTT** for communication, and **CRI commands** for robot control.

The system detects four randomly placed boxes, determines their positions, selects them in **nearest-first order**, and autonomously picks and stacks them at a storage location received through MQTT.

## 🚀 Features

- 📦 Detects **four randomly placed boxes** using OpenCV
- 🎯 Determines box coordinates from the camera feed
- 📏 Calculates the distance to each detected box
- 🔄 Selects boxes automatically in **nearest-first order**
- 🦾 Controls robotic pick-and-place operations using **CRI commands**
- 📡 Receives the storage/target position through **MQTT**
- 📤 Publishes robot status and pick/place coordinates through MQTT
- 👁️ Provides **live OpenCV visualization**
- 🏷️ Highlights detected, currently selected, and target boxes
- 📚 Automatically stacks all detected boxes at the target storage position

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| Python | Main programming language |
| OpenCV | Object detection and live visualization |
| MQTT | Communication and coordinate/status exchange |
| CRI Commands | Robot control and movement |
| Computer Vision | Box localization and coordinate detection |
| Robotics | Autonomous pick-and-place operations |

## ⚙️ System Workflow

```text
Camera Feed
     ↓
OpenCV Box Detection
     ↓
Detect Four Boxes
     ↓
Extract Box Coordinates
     ↓
Calculate Distance
     ↓
Select Nearest Box
     ↓
Generate Pick Coordinates
     ↓
Send CRI Commands to Robot
     ↓
Pick Box
     ↓
Receive Storage Position via MQTT
     ↓
Move Robot to Target Position
     ↓
Place / Stack Box
     ↓
Publish Status & Coordinates via MQTT
     ↓
Repeat Until All Boxes Are Stacked
```

## 🧠 How It Works

### 1. Box Detection

The camera continuously captures the workspace.

OpenCV processes the live video feed to identify the four randomly positioned boxes and determine their locations.

```text
Camera → Frame Capture → Image Processing → Box Detection
```

### 2. Nearest-First Selection

After detecting all boxes, the system calculates their relative distances.

The nearest available box is selected first.

Example:

```text
Detected Boxes:

Box 1 → Distance: 120
Box 2 → Distance: 75
Box 3 → Distance: 160
Box 4 → Distance: 95

Picking Order:

Box 2 → Box 4 → Box 1 → Box 3
```

This creates an efficient autonomous pick sequence.

### 3. MQTT Communication

MQTT is used for communication between different parts of the system.

The robot receives the **storage/target position** through MQTT.

The system also publishes information such as:

```text
Robot Status
Selected Box
Pick Coordinates
Place Coordinates
Current Operation
```

Example messages:

```text
Status: BOX_DETECTED
Pick Position: (x, y, z)

Status: PICKING
Selected Box: Box_2

Status: MOVING_TO_STORAGE

Place Position: (x, y, z)

Status: PLACED
```

### 4. Robot Control

After selecting a box, the system generates the required robot movement instructions.

The robot is controlled using **CRI commands**.

The basic sequence is:

```text
Move to Box
     ↓
Position End Effector
     ↓
Pick Box
     ↓
Lift Box
     ↓
Move to Storage Position
     ↓
Place Box
     ↓
Return for Next Box
```

### 5. Box Stacking

The storage position is received over MQTT.

Each detected box is transported to the target location and stacked sequentially.

```text
Storage Position

      ┌─────────┐
      │  Box 4  │
      ├─────────┤
      │  Box 3  │
      ├─────────┤
      │  Box 2  │
      ├─────────┤
      │  Box 1  │
      └─────────┘
```

The stacking position can be adjusted after every successful placement based on the box height.

## 👁️ Live OpenCV Visualization

The system displays the robot workspace in real time.

The visualization can show:

- Detected boxes
- Bounding boxes
- Box coordinates
- Currently selected box
- Target/storage position
- Pick position
- Place position
- Robot operation status

This makes it easier to monitor and debug the autonomous operation.

## 📡 System Architecture

```text
                    ┌──────────────────┐
                    │      Camera      │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │     OpenCV       │
                    │  Box Detection   │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ Box Coordinates  │
                    │ + Distance Calc. │
                    └────────┬─────────┘
                             │
                             ▼
                    ┌──────────────────┐
                    │ Nearest Box      │
                    │ Selection        │
                    └────────┬─────────┘
                             │
                ┌────────────┴────────────┐
                ▼                         ▼
        ┌───────────────┐         ┌───────────────┐
        │     MQTT      │         │ CRI Commands  │
        │ Communication │         │ Robot Control │
        └───────┬───────┘         └───────┬───────┘
                │                         │
                ▼                         ▼
        Storage Position            Robotic Arm
                │                         │
                └────────────┬────────────┘
                             ▼
                    ┌──────────────────┐
                    │ Pick & Stack Box │
                    └──────────────────┘
```

## 📁 Suggested Project Structure

```text
autonomous-box-stacking/
│
├── src/
│   ├── box_detection.py
│   ├── robot_control.py
│   ├── mqtt_client.py
│   ├── coordinate_utils.py
│   └── main.py
│
├── config/
│   └── config.yaml
│
├── images/
│   └── demo.png
│
├── requirements.txt
├── .gitignore
├── LICENSE
└── README.md
```

## 📦 Installation

Clone the repository:

```bash
git clone <your-repository-url>
cd autonomous-box-stacking
```

Create a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install the required dependencies:

```bash
pip install -r requirements.txt
```

Example dependencies:

```text
opencv-python
numpy
paho-mqtt
```

## ▶️ Running the Project

Start the system using:

```bash
python src/main.py
```

Make sure that:

- The camera is connected and accessible
- The MQTT broker is running
- The robot controller is connected
- CRI communication is configured correctly
- The storage position is being published through MQTT

## 🔄 Complete Operation

```text
START
  ↓
Connect Camera
  ↓
Connect MQTT
  ↓
Connect Robot
  ↓
Detect 4 Boxes
  ↓
Calculate Coordinates
  ↓
Find Nearest Box
  ↓
Pick Box
  ↓
Receive Storage Position
  ↓
Move to Storage
  ↓
Place / Stack Box
  ↓
Publish Status via MQTT
  ↓
More Boxes?
  │
  ├── YES → Select Next Nearest Box
  │
  └── NO → Task Complete
```

## 🎯 Project Objective

The objective of this project is to demonstrate an autonomous robotic pick-and-place system that combines **computer vision, robotics, and IoT communication**.

The system minimizes manual intervention by automatically detecting objects, deciding the picking sequence, controlling robot movements, and communicating operation status in real time.

## 🔮 Future Improvements

Possible improvements include:

- Deep-learning-based object detection using YOLO
- Automatic camera-to-robot coordinate calibration
- Collision and obstacle detection
- Dynamic path planning
- Multiple box classes and sizes
- Improved stacking optimization
- Web-based monitoring dashboard
- MQTT-based remote robot control
- Logging and analytics for robot operations

## 📌 Key Concepts

`Computer Vision` • `OpenCV` • `Robotics` • `MQTT` • `CRI` • `Python` • `Pick and Place` • `Object Detection` • `Autonomous Systems` • `IoT`
