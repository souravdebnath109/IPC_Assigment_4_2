# Person Detection Using YOLO with IPC (C & Python)


## 1. Assignment Description

This project implements a **person detection system** using the **YOLO (You Only Look Once)** algorithm.  
The detection is performed in a **C program**, and the detection output (bounding box coordinates + confidence score) is shared using **POSIX Shared Memory (IPC)**.

A separate **Python program** reads the shared memory data and visualizes the detected persons by drawing **bounding boxes** on the image using **OpenCV**.

This assignment demonstrates:
- YOLO inference using C
- Inter-Process Communication (IPC) using POSIX shared memory
- Data sharing between C and Python programs
- Separation of detection and visualization modules

---


## 2. System Overview

### Components

- **C Program (`yolo_ipc.c`)**  
  Runs YOLO person detection and writes results to shared memory.

- **Python Program (`visualize.py`)**  
  Reads detection results from shared memory and visualizes them on the input image.

- **IPC Mechanism (POSIX Shared Memory)**  
  Shared memory is used for fast communication between processes.

---

## 3. Environment Setup

### 3.1 Install WSL (Windows Subsystem for Linux)

1. Open **PowerShell as Administrator**
2. Install Ubuntu:

   ```bash
   wsl --install -d Ubuntu

### 3.2 Install Required Packages in WSL

```bash
sudo apt update
sudo apt install build-essential python3 python3-opencv -y
```

---

### 3.3 Darknet Setup

1. Clone Darknet:

   ```bash
   git clone https://github.com/AlexeyAB/darknet.git
   cd darknet
   ```

2. Edit `Makefile` and set:

   ```
   GPU=0
   OPENCV=0
   LIBSO=1
   ```

3. Build Darknet as a shared library:

   ```bash
   make clean
   make
   ```

4. Download YOLOv4-tiny model files:

   ```bash
   wget https://github.com/AlexeyAB/darknet/releases/download/yolov4/yolov4-tiny.weights
   wget https://raw.githubusercontent.com/AlexeyAB/darknet/master/cfg/yolov4-tiny.cfg
   wget https://raw.githubusercontent.com/AlexeyAB/darknet/master/data/coco.names
   ```

---

## 4. Files in This Assignment

```
darknet/
│
├── yolo_ipc.c        # C program for YOLO person detection + IPC
├── visualize.py      # Python program for IPC reading and visualization
├── yolov4-tiny.cfg
├── yolov4-tiny.weights
├── person.jpg        # Input image
```

---

## 5. How to Run the Assignment

### Step 1: Compile the C Program

From inside the `darknet` directory:

```bash
gcc yolo_ipc.c -o yolo_ipc \
    -Iinclude \
    -L. -ldarknet \
    -lm -pthread
```

Set the library path:

```bash
export LD_LIBRARY_PATH=.
```

---

### Step 2: Run the C Program (YOLO + IPC Writer)

```bash
./yolo_ipc person.jpg
```

Expected output:

```
Detections written to shared memory. Count = X
```

---

### Step 3: Run the Python Program (IPC Reader + Visualization)

```bash
python3 visualize.py
```

Expected output:

```
Detections read from shared memory: X
Output saved as ipc_output.jpg
Shared memory cleaned
```

The output image `ipc_output.jpg` will be generated with bounding boxes around detected persons.

---

## 6. Output

- `ipc_output.jpg`  
  Image showing detected person(s) with bounding boxes and confidence values.

---

  ### Input Image
  ![Input Image](/person.jpg)

  ### Output After YOLO Detection
  ![Detected Persons](/ipc_output.jpg)

## 7. Adding Images to the Report

For documentation or report purposes, the following images can be included:

- Input image (`person.jpg`)
- Output image (`ipc_output.jpg`)
- System architecture or block diagram
- Shared memory workflow diagram

Images can be stored in a separate folder (e.g., `images/`) and referenced in the report.

---

## 8. Conclusion

This assignment successfully demonstrates person detection using YOLO in C and efficient communication using POSIX shared memory (IPC).
The detection and visualization are separated into two programs, making the system modular, clear, and effective.
