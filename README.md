## Virtual Mouse with Hand Gestures
Welcome to the Virtual Mouse with Hand Gestures project! 🎉 This innovative program leverages computer vision and hand tracking to control your computer mouse using simple hand gestures. Perfect for adding a futuristic touch to your daily computer interactions!

## Features
Real-Time Hand Tracking: Utilizes MediaPipe to track hand movements through your webcam.
Mouse Control: Move the cursor and perform clicks by adjusting your hand position and gestures.
Customizable Sensitivity: Easily adjust the click sensitivity and gesture detection thresholds.
## Requirements
Before running this project, ensure you have the following installed:

Python 3.x
OpenCV
MediaPipe
PyAutoGUI
You can install the required packages using pip:

bash
Copy code
pip install opencv-python mediapipe pyautogui
## Getting Started
Clone the Repository

bash
Copy code
git clone https://github.com/yourusername/virtual-mouse.git
cd virtual-mouse
Run the Code

Ensure your webcam is connected and run the Python script:

bash
Copy code
python virtual_mouse.py
Use the Virtual Mouse

Move the Cursor: Move your index finger to control the cursor.
Click: Bring your index finger close to your thumb to simulate a mouse click.
Adjust Sensitivity: Modify the click sensitivity and gesture detection in the script if needed.
## How It Works
Video Capture: Captures video feed from your webcam.
Hand Detection: Uses MediaPipe to detect hand landmarks in the video feed.
Mouse Actions: Maps hand positions to screen coordinates and performs mouse actions based on the distance between your index finger and thumb.
## Customization
Feel free to tweak the sensitivity values and other parameters in the code to better suit your needs. The variables index_y and thumb_y control how sensitive the clicking action is.

## Code Overview
python
Copy code
import cv2
import mediapipe as mp
import pyautogui

# Initialize video capture and hand detector
cap = cv2.VideoCapture(0)
hand_detector = mp.solutions.hands.Hands()
drawing_utils = mp.solutions.drawing_utils
screen_width, screen_height = pyautogui.size()
index_y = 0

while True:
    # Capture frame from webcam
    ret, frame = cap.read()
    if not ret:
        break

    frame = cv2.flip(frame, 1)
    frame_height, frame_width, _ = frame.shape

    # Convert frame to RGB
    rgb_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
    output = hand_detector.process(rgb_frame)
    hands = output.multi_hand_landmarks

    if hands:
        for hand in hands:
            # Draw landmarks and perform actions
            drawing_utils.draw_landmarks(frame, hand)
            landmarks = hand.landmark
            index_x, index_y = 0, 0
            thumb_x, thumb_y = 0, 0

            for id, landmark in enumerate(landmarks):
                x = int(landmark.x * frame_width)
                y = int(landmark.y * frame_height)

                if id == 8:
                    cv2.circle(img=frame, center=(x, y), radius=10, color=(0, 255, 255), thickness=-1)
                    index_x = screen_width / frame_width * x
                    index_y = screen_height / frame_height * y

                if id == 4:
                    cv2.circle(img=frame, center=(x, y), radius=10, color=(0, 255, 255), thickness=-1)
                    thumb_x = screen_width / frame_width * x
                    thumb_y = screen_height / frame_height * y

            if abs(index_y - thumb_y) < 20:
                pyautogui.click()
                pyautogui.sleep(1)
            elif abs(index_y - thumb_y) < 100:
                pyautogui.moveTo(index_x, index_y)

    cv2.imshow('Virtual Mouse', frame)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
## Contribute
We welcome contributions to this project! If you have suggestions or improvements, feel free to submit a pull request or open an issue.

## *License
This project is licensed under the MIT License. See the LICENSE file for details.

