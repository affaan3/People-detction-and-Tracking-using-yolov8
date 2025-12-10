# **📌 People Counter Using YOLOv8 and OpenCV**

A real-time **people counting system** built with **YOLOv8**, **OpenCV**, and **NumPy**.
The application detects people through a live webcam feed, tracks them across frames, and counts entries based on virtual zones drawn in the scene. It also logs timestamped counts and stores the processed video output.

---

## **✨ Features**

* ✔️ Real-time detection using **YOLOv8n**
* ✔️ Object tracking enabled with `model.track()`
* ✔️ Counts people entering a designated polygonal area
* ✔️ Saves video output (`.avi`) automatically
* ✔️ Writes count logs with timestamps to `counter.txt`
* ✔️ Displays live system time overlay
* ✔️ Mouse callback to get pixel coordinates
* ✔️ Customizable polygon zones for entry/exit detection

---

## **📁 Project Structure**

```
├── people_counter.py      # Main program
├── coco.txt               # COCO class labels
├── counter.txt            # Auto-generated log file
├── YYYY-MM-DD.avi         # Auto-generated output video
└── README.md
```

---

## **🛠️ Requirements**

Install dependencies before running the script:

```bash
pip install opencv-python-headless ultralytics cvzone pandas numpy
```

Or for GPU systems:

```bash
pip install ultralytics
```

Ensure you download the YOLO model:

* `yolov8n.pt` (automatically downloaded on first run)

---

## **🚀 How It Works**

### **1. Live Camera Input**

The script reads frames from the webcam (`cv2.VideoCapture(0)`).

### **2. YOLOv8 Detection & Tracking**

```python
results = model.track(frame, persist=True)
```

Each detection provides:
`x1, y1, x2, y2, id, class_id`.

### **3. Polygon Zone Logic**

Two polygonal areas define a "path" of movement:

```python
area1 = [(198,499),(807,499),(822,440),(198,440)]
area2 = [(174,404),(174,435),(822,435),(806,404)]
```

A person is counted as **entered** when tracked through both zones sequentially.

### **4. Counter Logic**

IDs are stored in sets:

* `entry` → IDs detected in zone 1
* `entry1` → IDs confirmed through zone 2

The count is:

```python
total_people = len(entry1) + 1402   # 1402 = initial baseline
```

### **5. Video Recording & Logging**

* Frames saved to a `.avi` file using OpenCV’s `VideoWriter`
* Each count is logged:

```
COUNT: 1532 at 2025-12-10 19:25:31.482929
```

---

## **▶️ How to Run**

Simply execute:

```bash
python people_counter.py
```

Press **ESC** to stop the program.

---

## **📊 Output**

### **Real-Time Display Shows:**

* Bounding box for confirmed entries
* Live count
* Timestamp
* Processed frame preview

### **Generated Files:**

| File             | Purpose                         |
| ---------------- | ------------------------------- |
| `YYYY-MM-DD.avi` | Saved output video              |
| `counter.txt`    | Logs every count with timestamp |

---

## **🧩 Customization**

### **Change the entry zone**

Use your mouse to get coordinates (printed on console):

```python
cv2.setMouseCallback('Frame', RGB)
```

Update `area1` and `area2` as needed.

### **Change YOLO model**

Replace:

```python
model = YOLO('yolov8n.pt')
```

with:

```
yolov8s.pt  # more accurate, slower
yolov8l.pt  # high accuracy
```

---

## **📌 Notes**

* Ensure good lighting for more consistent detection.
* Adjust polygon coordinates based on your camera angle.
* The `+1402` baseline is optional; remove if not required.
