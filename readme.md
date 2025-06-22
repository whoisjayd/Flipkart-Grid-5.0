# Flipkart GRID 5.0 - Robotics Challenge

![visitor badge](https://visitor-badge.laobi.icu/badge?page_id=whoisjayd.Flipkart-Grid-5.0)
[![Python 3.7+](https://img.shields.io/badge/Python-3.7+-blue.svg)](https://www.python.org/downloads/release/python-370/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![OpenCV](https://img.shields.io/badge/OpenCV-4.x-green.svg)
![YOLO](https://img.shields.io/badge/YOLO-v5-blueviolet.svg)

This repository contains the complete codebase developed by our team for the **Flipkart Grid 5.0 Robotics Challenge Finals**, held at IIT Madras. The project demonstrates a sophisticated robotic arm system capable of identifying, picking, and placing objects using advanced computer vision and robotics techniques.

## Table of Contents

- [Project Overview](#project-overview)
- [System Architecture](#system-architecture)
- [Features](#features)
- [Hardware & Software Stack](#hardware--software-stack)
- [Installation](#installation)
- [Usage](#usage)
- [State Machine](#state-machine)
- [License and Permissions](#license-and-permissions)
- [Meet the Team](#meet-the-team)
- [Acknowledgments](#acknowledgments)
- [Contact](#contact)

## Project Overview

The core of this project is a fully autonomous robotic arm that performs complex pick-and-place operations. The system uses an Intel RealSense camera to perceive the environment in 3D, detects target objects using a custom-trained YOLO model, and calculates the precise movements required to grasp the object using inverse kinematics. Once an object is secured, the robot uses feeds from multiple ESP32 cameras to identify the correct drop-off zone, completing the task with high precision and efficiency.

## System Architecture

The robot's workflow is orchestrated as a finite state machine, ensuring robust and sequential execution of tasks.

1.  **Object Detection**: The primary RealSense camera scans the workspace. A YOLOv5 model running on the video feed detects and localizes potential objects ("boxes").
2.  **3D Coordinate Mapping**: For each detected object, the system uses the camera's depth sensor to calculate its real-world 3D coordinates (X, Y, Z).
3.  **Inverse Kinematics**: The 3D coordinates of the closest object are fed into the inverse kinematics algorithm, which calculates the required rotation angles for each of the arm's joints.
4.  **Motor Control**: The calculated joint angles are sent to an Arduino via serial communication, which drives the motors to move the arm and pick up the object.
5.  **QR Code Verification**: After picking up an object, the arm moves to a designated scanning position. Two ESP32 cameras are used to detect a QR code that determines the final drop zone. This is handled with multithreading to process both camera feeds simultaneously.
6.  **Placing the Object**: Based on the detected QR code, the arm moves to the corresponding drop zone and releases the object. The system then returns to its initial state to look for new objects.

## Features

-   **Real-time 3D Object Detection**: Utilizes a custom-trained YOLOv5 model with an Intel RealSense depth camera to detect and locate objects in 3D space.
-   **Precise Robotic Arm Control**: Implements a robust inverse kinematics model to translate 3D world coordinates into accurate joint angles for the robotic arm.
-   **Multi-threaded Camera Handling**: Concurrently processes video streams from two ESP32 cameras to efficiently detect QR codes for sorting tasks.
-   **Arduino-based Motor Control**: Seamless serial communication with an Arduino board for low-level control of the arm's servo motors and gripper.
-   **State-Machine Logic**: A clear and robust state-driven architecture ensures reliable and sequential operation of the robotic tasks.

## Hardware & Software Stack

### Hardware
* Custom-built Robotic Arm
* Intel® RealSense™ Depth Camera
* ESP32-CAM Modules (x2)
* Arduino Mega/Uno for motor control
* Servo Motors & Gripper Mechanism

### Key Software/Libraries
* **Python 3.7+**
* **OpenCV**: For image processing and camera handling.
* **Ultralytics YOLOv5**: For deep learning-based object detection.
* **pyrealsense2**: Official SDK for Intel RealSense cameras.
* **pyserial**: For communication with the Arduino.
* **numpy**: For numerical operations and calculations.

## Installation

1.  **Clone the Repository**
    ```bash
    git clone [https://github.com/whoisjayd/Flipkart-Grid-5.0.git](https://github.com/whoisjayd/Flipkart-Grid-5.0.git)
    cd Flipkart-Grid-5.0
    ```

2.  **Set up Python Environment**
    It is recommended to use a virtual environment.
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    ```

3.  **Install Dependencies**
    Install all the required packages from the `requirements.txt` file.
    ```bash
    pip install -r requirements.txt
    ```

4.  **Hardware Setup**
    -   Connect the Intel RealSense camera to a USB 3.0 port.
    -   Connect the Arduino to the computer via USB. Ensure the correct `COM` port is specified in `arduino_communication.py`.
    -   Power on the ESP32 cameras and ensure they are connected to the same network. Update their IP addresses in `simulation.py`.

## Usage

To run the main robotic operation script, execute the `main.py` file.

```bash
python main.py
````

The script will initialize the cameras and the robotic arm, and then enter the main state-driven loop to begin the pick-and-place task.

## State Machine

The robot operates based on the following states defined in `simulation.py`:

  - `INITIALIZING`: Sets the robot to its default home position.
  - `PICKING_UP`: Moves the arm to a predefined observation pose to scan for objects.
  - `PROCESSING_PAUSED`: Pauses the arm while the camera detects objects and identifies the closest one.
  - `PICKED_UP`: Calculates the inverse kinematics for the target and moves to pick it up.
  - `PICK_UP_PICKED`: Confirms the object is picked and moves to a safe height.
  - `DROP_ZONE_PICKED`: Moves the arm to a general drop area.
  - `ESP_POSE_PICKED`: Moves the object in front of the ESP32 cameras to scan for a QR code, which determines the final placement.

## License and Permissions

This project is licensed under the MIT License. See the `LICENSE` file for more details.

The code in this repository is made publicly available for demonstration and reference purposes. You are free to review and learn from it. However, you may not use, copy, or modify the code for other competitions or commercial purposes without explicit prior permission from the author.

## Meet the Team

We are a group of passionate engineering students from the Institute of Technology, Nirma University, Ahmedabad.

  - **[Jaydeep Solanki](https://www.linkedin.com/in/solanki-jaydeep)** (EC)
  - **[Vivek Samani](https://www.linkedin.com/in/vivek-samani-0957a127a/)** (EC)
  - **[Satyam Rana](https://www.linkedin.com/in/satyam-rana-690692256/)** (EC)
  - **[Sneh Shah](https://www.linkedin.com/in/sneh-shah-b8177828a/)** (EC)
  - **[Ameya Kale](https://www.linkedin.com/in/ameya-kale-5228a8257/)** (EI)
  - **[Keshav Vyas](https://www.linkedin.com/in/keshav-vyas-b194b4259/)** (EE)

## Acknowledgments

We extend our sincere gratitude to **Flipkart** for organizing the Flipkart GRID 5.0 competition. It was an incredible platform that allowed us to apply our skills to a real-world robotics challenge and learn immensely throughout the process.

## Contact

For any inquiries, permissions, or collaborations, please feel free to reach out:

  - **Name**: Jaydeep Solanki
  - **Email**: [jaydeep.solankee@yahoo.com](mailto:contactjaydeepsolanki@gmail.com)
  - **LinkedIn**: [linkedin.com/in/jaydeep-solanki-79ab61253](https://www.linkedin.com/in/solanki-jaydeep)
