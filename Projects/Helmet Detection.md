
# 🪖 Helmet Detection using YOLOv8n

A real-time Helmet Detection System built using **YOLOv8n** and **PyTorch** for detecting whether motorcycle riders are wearing helmets or not.

This project uses Computer Vision and Deep Learning techniques to improve road safety monitoring and automate helmet compliance detection from images, videos, and live webcam feeds.

---

# 🚀 Features

✅ Real-time helmet detection  
✅ YOLOv8n lightweight model for fast inference  
✅ PyTorch-based implementation  
✅ Supports images, videos, and webcam streams  
✅ High-speed object detection  
✅ Easy to train on custom datasets  
✅ OpenCV integration for visualization  

---

# 🛠️ Tech Stack

- **Python**
- **PyTorch**
- **YOLOv8n (Ultralytics)**
- **OpenCV**
- **Pandas**
- **Roboflow**

---

# ⚙️ Installation

## Clone the Repository

```bash
git clone https://github.com/lakshyajain1508/Helmet-Detection.git
cd Helmet-Detection
```

---

# 📦 Requirements

Example dependencies:

```txt
ultralytics
torch
torchvision
opencv-python
numpy
matplotlib
```

---

# 🧠 Model Used

This project uses:

## YOLOv8n

* Lightweight and fast object detection model
* Optimized for real-time applications
* Built using PyTorch
* Developed by Ultralytics

---

# 🏋️ Training the Model

```bash
yolo detect train data=data.yaml model=yolov8n.pt epochs=50 imgsz=640
```

---

# ▶️ Run Detection

## Detect on Image

```bash
python detect.py --source image.jpg
```

## Detect using Webcam

```bash
python detect.py --source 0
```

---

# 📊 Classes

The model is trained to detect:

* Helmet
* No Helmet

---

# 📸 Sample Results

| Input            | Detection Output |
| ---------------- | ---------------- |
| Motorcycle Rider | Helmet Detected  |
| Traffic CCTV     | No Helmet Alert  |

> Add screenshots or GIFs here for better presentation.

---

# 📈 Future Improvements

* Number plate recognition
* Traffic violation automation
* Multi-rider tracking
* Cloud deployment
* Android/Web application integration

---


