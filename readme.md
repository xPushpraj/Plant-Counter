# 🌱 Plant Counter System with Real-Time Detection

A smart, edge-AI powered plant counting solution built on Raspberry Pi 5 using YOLOv8 for real-time detection of standing and fallen plants. The system exposes a live API feed accessible across your local network for remote monitoring via mobile or desktop browsers.

---

## 📋 Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Hardware Requirements](#hardware-requirements)
- [System Architecture](#system-architecture)
- [Installation](#installation)
- [Usage](#usage)
- [API Endpoints](#api-endpoints)
- [Live Demo](#live-demo)
- [Mobile Application](#mobile-application)
- [Circuit Diagram](#circuit-diagram)
- [Demo Videos](#demo-videos)
- [Gallery](#gallery)
- [Troubleshooting](#troubleshooting)
- [Contact](#contact)

---

## 🎯 Overview

This project leverages the power of **YOLOv8** object detection running natively on a **Raspberry Pi 5** to identify and count plants in real-time. The system distinguishes between **standing (healthy)** and **fallen (damaged/lodged)** plants, making it ideal for agricultural monitoring, crop health assessment, and automated field analytics.

The Raspberry Pi serves as both the inference engine and a web server, broadcasting the processed video feed with detection overlays to any device on the same network.

---

## ✨ Features

| Feature | Description |
|---------|-------------|
| 🔍 **Real-Time Detection** | YOLOv8 model optimized for edge deployment |
| 📊 **Dual Classification** | Detects both **standing** and **fallen** plants |
| 🌐 **Network Streaming** | Live feed accessible via local network API |
| 📱 **Cross-Platform** | View on mobile, tablet, or laptop wirelessly |
| ⚡ **Edge Computing** | No cloud dependency; fully offline capable |
| 🔄 **REST API** | Easy integration with external systems |
| 📸 **High-Quality Imaging** | Powered by Raspberry Pi Camera Module 3 |

---

## 🛠 Hardware Requirements

### Core Components

| Component | Specification | Purpose |
|-----------|-------------|---------|
| **Raspberry Pi 5** | 4GB/8GB RAM | Main processing unit & inference engine |
| **Camera Module 3** | 12MP, HDR, Auto-focus | High-quality image capture |
| **Power Supply** | 5V 5A USB-C (Official RPi 5 PSU) | Stable power for sustained operation |
| **MicroSD Card** | 64GB+ (Class 10/UHS-I) | OS and model storage |
| **Ethernet/WiFi** | Built-in WiFi 6 / Gigabit Ethernet | Network connectivity |

### Optional Accessories
- **Cooling Fan / Heatsink** – Recommended for sustained inference workloads
- **Enclosure/Case** – Weatherproof housing for field deployment
- **Tripod/Mount** – For stable camera positioning

---

## 🏗 System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Raspberry Pi 5                           │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐  │
│  │  Camera      │───▶│  YOLOv8      │───▶│  Flask/Fast  │  │
│  │  Module 3    │    │  Inference   │    │  API Server  │  │
│  └──────────────┘    └──────────────┘    └──────────────┘  │
│                                      │                      │
│                                      ▼                      │
│                           ┌──────────────────┐              │
│                           │  Local Network   │              │
│                           │  (WiFi/Ethernet) │              │
│                           └────────┬─────────┘              │
└────────────────────────────────────┼────────────────────────┘
                                     │
                    ┌────────────────┼────────────────┐
                    ▼                ▼                ▼
              ┌─────────┐      ┌─────────┐      ┌─────────┐
              │ Mobile  │      │ Laptop  │      │ Tablet  │
              │ Browser │      │ Browser │      │ Browser │
              └─────────┘      └─────────┘      └─────────┘
```

---

## 🚀 Installation

### 1. Operating System Setup
```bash
# Download and flash Raspberry Pi OS (64-bit) to SD card
# Enable camera and SSH via raspi-config
sudo raspi-config
# Interface Options → Camera → Enable
# Interface Options → SSH → Enable
```

### 2. System Dependencies
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y python3-pip python3-venv libcamera-dev
```

### 3. Python Environment
```bash
python3 -m venv plantcounter-env
source plantcounter-env/bin/activate

pip install ultralytics flask opencv-python picamera2 numpy
```

### 4. Model Setup
Place your trained YOLOv8 model (`best.pt`) in the project directory:
```bash
mkdir -p ~/plant-counter/models
# Copy your trained model (standing vs fallen plants)
cp best.pt ~/plant-counter/models/
```

### 5. Clone & Configure
```bash
cd ~
git clone https://github.com/yourusername/plant-counter-system.git
cd plant-counter-system
```

---

## 💻 Usage

### Start the Detection Server
```bash
source ~/plantcounter-env/bin/activate
cd ~/plant-counter-system
python3 app.py
```

### Access the Live Feed
Once running, access the stream from any device on the same network:

| Endpoint | URL Format | Description |
|----------|-----------|-------------|
| **Live Video Feed** | `http://<raspberry-pi-ip>:5000/video_feed` | MJPEG stream with detection overlays |
| **API Status** | `http://<raspberry-pi-ip>:5000/api/status` | JSON response with count data |
| **Web Dashboard** | `http://<raspberry-pi-ip>:5000/` | Browser-based monitoring interface |

> **Find your Pi's IP:** Run `hostname -I` on the Raspberry Pi terminal.

---

## 🔌 API Endpoints

### GET `/api/status`
Returns current detection statistics in JSON format.

```json
{
  "status": "active",
  "timestamp": "2024-01-15T10:30:00Z",
  "counts": {
    "standing": 142,
    "fallen": 8,
    "total": 150
  },
  "confidence_threshold": 0.5,
  "fps": 15.3,
  "model": "yolov8n.pt"
}
```

### GET `/video_feed`
Real-time MJPEG stream endpoint. Compatible with:
- HTML `<img>` tags
- OpenCV `VideoCapture()`
- VLC Media Player
- Mobile browsers

### POST `/api/config`
Update detection parameters dynamically.

```bash
curl -X POST http://<pi-ip>:5000/api/config \
  -H "Content-Type: application/json" \
  -d '{"confidence": 0.7, "save_frames": true}'
```

---

## 🌐 Live Demo

Experience the system in action at our official portal:

### 🌍 **[https://coaetianinnovators.in/](https://coaetianinnovators.in/)**

The website provides:
- Live demonstration of the plant counting interface
- Real-time feed visualization
- System analytics dashboard
- Project documentation and updates

---

## 📱 Mobile Application

Download the official companion app for Android to access plant counting feeds on the go:

### ⬇️ **[Download APK](https://drive.google.com/uc?export=download&id=16_5lmzncWiqYwIoqXwCJmsU5Y1LPlxUA)**

**App Features:**
- 🔗 Connect to any Pi on your network
- 📊 Real-time count display
- 🎥 Live video stream with bounding boxes
- 📈 Historical data logging
- 🔔 Alert notifications for anomalies

---

## 🔧 Circuit Diagram

<!-- IMAGE PLACEHOLDER: Circuit Diagram -->
<!-- Add your circuit wiring image here -->
![Circuit Diagram](https://github.com/xPushpraj/Plant-Counter/blob/main/images/g_hub.png)


### Wiring Overview

| Component | Pi Pin | Connection |
|-----------|--------|------------|
| Camera Module 3 | CSI Port | Ribbon cable to Camera Serial Interface |
| Power LED | GPIO 14 (Pin 8) | 220Ω resistor to GND |
| Status LED | GPIO 15 (Pin 10) | 220Ω resistor to GND |
| Cooling Fan | 5V Pin (Pin 4) | Direct to 5V and GND |

```
┌─────────────────────────────────────────┐
│         Raspberry Pi 5 GPIO           │
│                                         │
│  [CSI]──────Camera Module 3             │
│                                         │
│  5V  (Pin 2/4)────┬──Power Supply      │
│  GND (Pin 6/9)────┘   (USB-C)          │
│                                         │
│  [Optional Peripherals]                 │
│  GPIO14 ──[R=220Ω]── LED+ ── LED- ──GND │
│  GPIO15 ──[R=220Ω]── LED+ ── LED- ──GND │
│                                         │
└─────────────────────────────────────────┘
```

---

## 🎥 Demo Videos

<!-- VIDEO PLACEHOLDER: Live Working Demo -->
### Live System in Action
> Add your demonstration video here showing the real-time detection working

```markdown
![Live Demo Video](assets/videos/live-demo.mp4)
```

**Video Contents Should Include:**
- 📷 Camera positioning and field of view
- 🖥 Terminal showing inference FPS and counts
- 📱 Mobile device accessing the feed wirelessly
- 📊 Real-time count updates as plants are detected
- 🔄 Transition between standing and fallen detection

---

## 📸 Gallery

<!-- IMAGE PLACEHOLDER: Machine Attachment -->
### System Mounted on Equipment
> Add images showing the Pi + Camera mounted on your agricultural machine/vehicle

```markdown
![Mounted System](images/https://github.com/xPushpraj/Plant-Counter/blob/main/images/g_hub.jpeg)
```

<!-- IMAGE PLACEHOLDER: Close-up Hardware -->
### Hardware Close-up
> Add close-up images of the assembled Raspberry Pi, Camera Module 3, and power connections

```markdown
![Hardware Close-up](assets/images/hardware-closeup.jpg)
```

<!-- IMAGE PLACEHOLDER: Detection Output -->
### Detection Visualization
> Add screenshots of the YOLOv8 bounding boxes on standing vs fallen plants

```markdown
![Detection Output](assets/images/detection-output.jpg)
```

---

## ⚙️ Configuration

Create a `config.yaml` file for easy parameter tuning:

```yaml
# config.yaml
camera:
  resolution: [1280, 720]
  fps: 30
  autofocus: true

model:
  path: "models/best.pt"
  confidence: 0.5
  iou_threshold: 0.45
  classes: ["standing", "fallen"]

server:
  host: "0.0.0.0"
  port: 5000
  debug: false

detection:
  save_frames: false
  output_dir: "captures/"
  alert_threshold: 10  # Alert if fallen plants > 10
```

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| Camera not detected | Run `libcamera-hello` to test; check ribbon cable orientation |
| Low FPS / lag | Reduce resolution; use YOLOv8n (nano) model; enable GPU acceleration |
| Network unreachable | Ensure Pi and client are on same subnet; check firewall rules |
| Model not loading | Verify `best.pt` path; ensure Ultralytics version compatibility |
| Power warnings | Use official 5V 5A PSU; avoid underpowered USB chargers |

---

## 📂 Project Structure

```
plant-counter-system/
├── app.py                  # Main Flask/FastAPI server
├── detector.py             # YOLOv8 inference wrapper
├── camera.py               # Pi Camera Module 3 interface
├── config.yaml             # System configuration
├── models/
│   └── best.pt             # Trained YOLOv8 model
├── static/
│   ├── css/
│   └── js/
├── templates/
│   └── index.html          # Web dashboard
├── assets/
│   ├── images/             # Screenshots & hardware photos
│   └── videos/             # Demo recordings
├── captures/               # Saved detection frames
├── requirements.txt
└── README.md
```

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- [Ultralytics YOLOv8](https://github.com/ultralytics/ultralytics) for the state-of-the-art detection framework
- [Raspberry Pi Foundation](https://www.raspberrypi.org/) for the incredible hardware ecosystem
- [Flask](https://flask.palletsprojects.com/) for the lightweight web server

---

## 📞 Contact

For questions, support, or collaboration inquiries:

**🌐 Website:** [https://coaetianinnovators.in/](https://coaetianinnovators.in/)

**💬 Contact Portal:** [https://xpushpz.me](https://xpushpz.me)

**📧 Email:** [Add your email here]

---

<p align="center">
  <b>Built with ❤️ for smarter agriculture</b><br>
  <i>Coaetian Innovators</i>
</p>
