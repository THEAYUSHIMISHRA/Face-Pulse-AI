# Face-Pulse-AI
🧠 FacePulse: Face Recognition System for Server Room
🚆 RDSO (Research Designs and Standards Organisation), Ministry of Railways, Government of India
📘 Project Overview

FacePulse is an intelligent face recognition–based logging and monitoring system developed during a Summer Internship at RDSO (Lucknow).
It automates in-time / out-time logging of authorized personnel entering secure server rooms, replacing traditional manual or RFID systems with an AI-driven, contactless authentication solution.

Developed using Django, DeepFace (Facenet), and PostgreSQL, the system integrates machine learning, computer vision, and web technologies for real-time identity verification, time tracking, and secure data management.

🎯 Problem Statement

Manual or card-based access systems often suffer from:
⚙️ Human errors and impersonation risks

🕒 Time-consuming registration and logging

🔒 Lack of centralized monitoring

📉 Limited scalability and auditability

💡 Proposed Solution

FacePulse offers a web-based facial recognition system that:

Detects and verifies user identity using DeepFace + Facenet

Automatically records entry and exit timestamps

Stores logs in a PostgreSQL database

Provides a simple, browser-accessible dashboard for real-time tracking

Supports future integration with mobile and analytics systems

⚙️ Tech Stack
Component	Technology Used
Backend Framework	Django (Python)
Face Recognition	DeepFace (Facenet model)
Database	PostgreSQL
Frontend	HTML5, CSS3, JavaScript (MediaStream API)
Libraries	OpenCV, NumPy, Pandas, TensorFlow/Keras, python-docx
Tools	Visual Studio Code, Git, GitHub
Version Control	Git
