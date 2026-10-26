# 🚨 SafeCrowd

## **A Real-Time Crowd Management & Safety System**

A comprehensive full-stack application for detecting overcrowding, managing emergencies, and providing real-time monitoring of public spaces. Built by the **Strange Coders** team for MEGAHACK 2026.

---

## 📋 Project Overview

**SafeCrowd** is an intelligent crowd management system that leverages AI-powered computer vision to detect, analyze, and respond to overcrowding situations in real-time. The system provides emergency response coordination, real-time analytics, and administrative controls for managing public safety.

### Key Capabilities
- 🎥 **Real-time crowd detection** using YOLOv8 AI models
- 📊 **Live density analysis** and heatmaps
- 🚨 **Automatic alert generation** for overcrowding
- 🆘 **SOS emergency requests** with location tracking
- 📢 **Public announcements** and messaging
- 💬 **AI-powered chatbot** for inquiries
- 🗺️ **Interactive map visualization** with Leaflet
- 📈 **Advanced analytics** and historical data tracking
- 🎨 **3D visualization** with Three.js
- 🌓 **Dark/Light theme support** with responsive design


---

## 💻 Technology Stack

### **Frontend** (77.8% of codebase)
| Technology | Purpose |
|-----------|---------|
| **React 19.2.0** | UI framework |
| **Vite 7.3.1** | Build tool & dev server |
| **Tailwind CSS 4.2.1** | Styling & responsive design |
| **React Router 7.13.1** | Client-side routing |
| **Framer Motion 12.35.2** | Animations & motion effects |
| **Three.js 0.183.2** | 3D visualization |
| **Leaflet 1.9.4** | Interactive maps |
| **Recharts 3.8.0** | Data visualization charts |
| **Socket.IO Client 4.8.3** | Real-time communication |
| **TensorFlow.js 4.22.0** | ML in browser (optional) |
| **Lucide React 0.577.0** | Icon library |
| **React Three Fiber 9.5.0** | React wrapper for Three.js |

### **Backend** (12.9% of codebase)
| Technology | Purpose |
|-----------|---------|
| **Flask 3.1.0** | Web framework |
| **Flask-SocketIO 5.4.1** | WebSocket support |
| **OpenCV 4.10.0.84** | Computer vision operations |
| **Ultralytics YOLOv8 8.3.58** | Object detection |
| **OpenAI 1.58.1** | Chatbot AI integration |
| **Eventlet 0.38.0** | WSGI server |
| **Python-dotenv 1.0.1** | Environment variables |
| **SQLite3** | Database (built-in) |

### **ML/AI Components**
- **YOLOv8 Nano** (yolov8n.pt) - Lightweight object detection model (6.5MB)
- **OpenAI GPT** - Conversational AI for chatbot responses
- **TensorFlow.js** - Browser-based ML inference (optional)

### **Languages**
- **JavaScript** - 107,409 bytes (77.8%)
- **Python** - 17,849 bytes (12.9%)
- **CSS** - 11,555 bytes (8.4%)
- **Batchfile** - 913 bytes (0.7%)
- **HTML** - 372 bytes (0.3%)

---

## 🎨 Frontend Features

### Main Application Structure
```javascript
// App.jsx - Core application with routing
Routes:
���── / (Dashboard) - Main analytics dashboard
├── /monitoring (Live Monitoring) - Real-time crowd detection feed
├── /three-d (3D Visualization) - 3D crowd visualization
├── /sos-alerts (SOS Alerts) - Emergency requests management
├── /admin (Admin Panel) - System administration
└── /settings (Settings) - User preferences & configuration
```

## Key Frontend Components
- Dashboard - Real-time statistics and analytics
- Live Monitoring - Video feed with bounding boxes for detected people
- 3D Visualization - Three.js 3D scene with crowd representation
- SOS Alerts Management - Emergency response coordination
- Admin Panel - System administration and announcements
- Settings Page - User preferences, theme, notifications, etc.
- SOSButton - Emergency alert trigger component
