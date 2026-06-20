# Facial Recognition Attendance System

A contactless attendance system built with Python, OpenCV (Haar Cascade + LBPH face recognizer), and Tkinter. It verifies that a device is on an allowed Wi-Fi network, lets a teacher open a timed attendance window with a password, and lets students mark attendance with a face scan from a webcam. Records are saved to a daily CSV file along with a face snapshot.

Originally built as a B.Tech (CSE - Data Science) mini project at Meerut Institute of Engineering and Technology, affiliated to Dr. A.P.J. Abdul Kalam Technical University, Lucknow.

## Features

- Teacher-controlled, password-protected, time-limited attendance window
- Wi-Fi based geo-fencing (only allows marking attendance on the configured network)
- Face training from a simple `pictures/` folder of `ID_Name.jpg` photos
- Live webcam face recognition using Haar Cascade detection + LBPH recognition
- Daily CSV attendance logs with timestamps, plus a saved face snapshot per entry
- Simple Tkinter GUI with live Wi-Fi status indicator

## Project structure

```
facial-attendance-system/
├── attendance_system_pictures.py   # main application
├── requirements.txt
├── pictures/                       # put registered student photos here (ID_Name.jpg)
├── haarcascade_frontalface_default.xml   # you need to add this (see Setup)
└── attendance/                     # created automatically at runtime (CSVs + snapshots)
```

## Setup

1. Clone the repo and create a virtual environment (optional but recommended):
   ```bash
   git clone <your-repo-url>
   cd facial-attendance-system
   python -m venv venv
   source venv/bin/activate   # Windows: venv\Scripts\activate
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
   `opencv-contrib-python` is required (not plain `opencv-python`) because the LBPH recognizer lives in `cv2.face`, which is only included in the contrib build.

3. Add the Haar Cascade file. Either download `haarcascade_frontalface_default.xml` from the [OpenCV GitHub repo](https://github.com/opencv/opencv/tree/master/data/haarcascades) and place it in the project root, or copy it from your OpenCV install:
   ```bash
   python -c "import cv2, shutil, os; shutil.copy(os.path.join(cv2.data.haarcascades, 'haarcascade_frontalface_default.xml'), '.')"
   ```

4. Update the configuration at the top of `attendance_system_pictures.py` for your setup:
   ```python
   TEACHER_PASSWORD = "teacher123"
   ALLOWED_WIFI = "Chaudhary"
   ATTENDANCE_WINDOW_SECONDS = 120
   ```

5. Add registered student photos to `pictures/`, named `ID_Name.jpg` (e.g. `101_Anuj.jpg`).

## Usage

```bash
python attendance_system_pictures.py
```

1. Click **Train from pictures** once after adding/updating photos in `pictures/`.
2. Teacher enters the password and clicks **Start Attendance Window** to open a timed window.
3. Each student clicks **Mark Attendance** and looks at the webcam; a match writes a row to `attendance/attendance_YYYYMMDD.csv` and saves a face snapshot to `attendance/snapshots/`.

## Notes / limitations

- This is a mini-project/teaching prototype, not a production security system. The Wi-Fi check and password protection are basic deterrents against proxy attendance, not strong security controls.
- Recognition uses LBPH on Haar-detected face crops, so accuracy depends a lot on lighting and photo quality in `pictures/`.
- Wi-Fi SSID detection logic differs by OS (Windows via `netsh`, Linux via `iwgetid`/`nmcli`, macOS via the `airport` utility) and may need adjustment depending on your system/driver.

## References

See the project report for literature references on face detection (Haar Cascades), LBPH face recognition, and the OpenCV/Pandas documentation used while building this.
