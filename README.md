# License-Plate-Recognition-System
# 🚗 License Plate Recognition System

An AI-powered real-time license plate detection and recognition tool using OpenCV and OCR libraries. Designed to enhance vehicle logging systems in parking lots, toll booths, and security checkpoints.

---

## 📌 Project Overview

Traditional vehicle check-in systems are slow, manual, and prone to errors. This project automates the process by:

* Capturing plates via webcam or IP camera
* Extracting text using OCR
* Logging plate numbers with timestamps in CSV

---

## 🛠️ Tech Stack

* **Python** – Core logic
* **OpenCV** – Plate detection using edge/contour
* **EasyOCR / pytesseract** – Text recognition
* **Pandas** – Logging to CSV
* **Tkinter (optional)** – Admin UI

---

## 📁 Folder Layout

```
LicensePlateReader/
├── plates/
├── logs/
├── detect_and_log.py
├── gui.py (optional)
├── requirements.txt
└── README.md
```

---

## 💻 Key Features

* 📸 Live webcam capture
* 🔍 Plate detection using contours
* 🧾 OCR for text extraction
* 🕒 Logs number + timestamp
* 📤 Export vehicle logs daily

---

## 🧠 Theory

* Uses edge detection and contour filtering to isolate rectangular license plates
* OCR engine scans extracted image
* Text is parsed and written into `logs/plates_log.csv`

---

## 🔣 Sample Snippet

```python
import cv2, easyocr, pandas as pd
from datetime import datetime

cap = cv2.VideoCapture(0)
reader = easyocr.Reader(['en'])

log_file = 'logs/plates_log.csv'
while True:
    ret, frame = cap.read()
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    edges = cv2.Canny(gray, 50, 200)
    cnts, _ = cv2.findContours(edges, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
    for c in cnts:
        approx = cv2.approxPolyDP(c, 0.03 * cv2.arcLength(c, True), True)
        if len(approx) == 4 and cv2.contourArea(c) > 1000:
            x, y, w, h = cv2.boundingRect(c)
            plate = frame[y:y+h, x:x+w]
            text = reader.readtext(plate)
            if text:
                plate_text = text[0][1]
                with open(log_file, 'a') as f:
                    f.write(f"{plate_text},{datetime.now()}\n")
    cv2.imshow("Plate Reader", frame)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
cap.release()
cv2.destroyAllWindows()
```

---

## 📈 Applications

* 🅿️ Parking access control
* 🚧 Toll booths or gated societies
* 🚓 Police or RTO records automation

---

## 👨‍💻 Author Info

**Pranjal Gurjar**
AI/Computer Vision Project – Final Year MCA
📧 [gurjarpranjal.work@gmail.com](mailto:gurjarpranjal.work@gmail.com)
