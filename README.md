<div align="center">
  <!-- <img src="src/assets/logo.png" alt="AttendX Logo" width="180"/> -->
  <h1 align="center">AttendX - For Smarter Classrooms</h1>
  <p align="center"><strong></strong></p>
  <p align="center">AI-Powered Dual-Modal Attendance System — Face Recognition + Voice Recognition</p>



  <br/>

  <p align="center">
    <img src="https://img.shields.io/badge/Python-3.14-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python"/>
    <img src="https://img.shields.io/badge/Streamlit-1.56-FF4B4B?style=flat-square&logo=streamlit&logoColor=white" alt="Streamlit"/>
    <img src="https://img.shields.io/badge/Supabase-PostgreSQL-3ECF8E?style=flat-square&logo=supabase&logoColor=white" alt="Supabase"/>
    <img src="https://img.shields.io/badge/dlib-Face_Rec-008080?style=flat-square&logo=dlib&logoColor=white" alt="dlib"/>
    <img src="https://img.shields.io/badge/scikit--learn-SVM-F7931E?style=flat-square&logo=scikit-learn&logoColor=white" alt="scikit-learn"/>
    <img src="https://img.shields.io/badge/Resemblyzer-Voice-FF6F00?style=flat-square&logo=libreofficeaudio&logoColor=white" alt="Resemblyzer"/>
    <img src="https://img.shields.io/badge/bcrypt-Auth-004880?style=flat-square&logo=auth0&logoColor=white" alt="bcrypt"/>
    <img src="https://img.shields.io/badge/license-MIT-999999?style=flat-square" alt="License"/>
  </p>
</div>

---

## Overview

**AttendX** is a modern, AI-driven classroom attendance system that eliminates manual roll calls. Teachers simply **upload a classroom photo** or **record classroom audio**, and AttendX's AI automatically identifies present students using **facial recognition** and **speaker recognition**. Students log in via their face — no passwords needed.

Built entirely on free-tier infrastructure (Streamlit Community Cloud + Supabase), AttendX delivers a production-ready experience at **zero infrastructure cost**.

---

## Key Features

### 👨‍🏫 Teacher Portal

| Feature | Description |
|---------|-------------|
| **Face Attendance** | Upload or capture a classroom photo — AI detects and identifies every student in frame |
| **Voice Attendance** | Record the classroom audio — AI splits the audio into individual speakers and identifies each student by voice |
| **Subject Management** | Create, view, and manage subjects with section details |
| **QR Code Sharing** | Generate a scannable QR code for any subject — students scan to auto-enroll |
| **Attendance Records** | View complete attendance history grouped by session with present/total summaries |

### 🧑‍🎓 Student Portal

| Feature | Description |
|---------|-------------|
| **Face Login** | Log in instantly via webcam — no username, no password |
| **Smart Enrollment** | Enroll in subjects by code or via a shareable `?join-code=` link |
| **Attendance Dashboard** | View per-subject attendance stats — total classes vs. attended |
| **Voice Enrollment** | Optionally enroll a voice profile during registration for voice-only attendance |

---

## How It Works

### Face Recognition Pipeline
```
Classroom Photo → dlib Face Detection → 128-d Embedding → SVM Classifier → Attendance Log
```
- **Detection**: dlib frontal face detector
- **Embedding**: Pre-trained face recognition models (128-dimensional vectors)
- **Classification**: Linear SVM trained on registered student embeddings with cosine distance threshold of **0.6**

### Voice Recognition Pipeline
```
Classroom Audio → Voice Activity Detection → Segment Splitting → Resemblyzer Embedding → Cosine Similarity → Attendance Log
```
- **VAD**: WebRTC Voice Activity Detection to isolate non-silent segments (>0.5s)
- **Embedding**: Resemblyzer VoiceEncoder maps each segment to an embedding vector
- **Matching**: Cosine similarity against stored voice profiles with threshold of **0.65**

---

## Tech Stack

<p align="center">
  <img src="https://skillicons.dev/icons?i=python,supabase,scikitlearn,postgresql,git" />
</p>

| Layer | Technology |
|-------|-----------|
| **Framework** | [Streamlit](https://streamlit.io) (Python web app framework) |
| **Database** | [Supabase](https://supabase.com) (PostgreSQL + REST API) |
| **Face Recognition** | dlib + [face_recognition_models](https://github.com/ageitgey/face_recognition_models) + scikit-learn SVM |
| **Voice Recognition** | [Resemblyzer](https://github.com/resemble-ai/Resemblyzer) + librosa + WebRTC VAD |
| **Auth** | bcrypt (teacher) / Face Recognition (student) |
| **QR Codes** | segno |
| **Deployment** | Streamlit Community Cloud |

---

## Project Structure

```
attendx/
├── app.py                          # Entry point
├── requirements.txt                # Dependencies
├── schema.sql                      # Database schema (reference)
│
├── src/
│   ├── assets/                     # Logos & branding
│   ├── ui/
│   │   └── base_layout.py          # Custom CSS (dark theme, fonts, buttons)
│   ├── screens/
│   │   ├── home_screen.py          # Landing page (Student/Teacher selection)
│   │   ├── teacher_screen.py       # Teacher login, dashboard, attendance flow
│   │   └── student_screen.py       # Student face login, registration, dashboard
│   ├── components/
│   │   ├── header.py               # Navigation header
│   │   ├── footer.py               # App footer
│   │   ├── subject_card.py         # Subject display card
│   │   ├── dialog_create_subject.py
│   │   ├── dialog_add_photo.py
│   │   ├── dialog_attendence_record.py
│   │   ├── dialog_auto_enroll.py
│   │   ├── dialog_enroll_subject.py
│   │   ├── dialog_share_subject.py
│   │   └── dialog_voice_attendence.py
│   ├── database/
│   │   ├── config.py               # Supabase client init
│   │   └── db.py                   # All CRUD operations
│   └── pipelines/
│       ├── face_pipeline.py        # Face detection, embedding, classification
│       └── voice_pipeline.py       # Voice encoding, speaker identification
└── .streamlit/
    └── secrets.toml                # Supabase credentials
```

---

## Quick Start

### Prerequisites
- Python 3.14+
- A [Supabase](https://supabase.com) account (free tier)

### Setup

```bash
# Clone the repo
git clone https://github.com/yourusername/attendx.git
cd attendx

# Create virtual environment
python -m venv venv
.\venv\Scripts\Activate  # Windows
# source venv/bin/activate  # macOS/Linux

# Install dependencies
pip install -r requirements.txt

# Configure Supabase
# 1. Create a Supabase project
# 2. Run schema.sql to create tables
# 3. Create .streamlit/secrets.toml:
#    [supabase]
#    supabase_url = "your-project-url"
#    supabase_key = "your-anon-key"

# Run the app
streamlit run app.py
```

---

## Database Schema

| Table | Purpose | Key Columns |
|-------|---------|-------------|
| `teachers` | Teacher accounts | `teacher_id`, `username`, `password` (bcrypt), `name` |
| `students` | Student profiles with AI embeddings | `student_id`, `name`, `face_embedding` (JSONB), `voice_embedding` (JSONB) |
| `subjects` | Class subjects | `subject_id`, `subject_code`, `name`, `section`, `teacher_id` (FK) |
| `subject_students` | Many-to-many enrollment | `subject_id` (FK), `student_id` (FK) |
| `attendence_logs` | Attendance records | `id`, `timestamp` (timestamptz), `subject_id` (FK), `student_id` (FK), `is_present` |

---

## Deployment

AttendX deploys effortlessly on **Streamlit Community Cloud**:

1. Push the repo to GitHub
2. Log in to [Streamlit Community Cloud](https://streamlit.io/cloud)
3. Click **New app** → select your repo → set main file to `app.py`
4. Add Supabase secrets in the Streamlit Cloud dashboard
5. Deploy — done 🚀

---

## Roadmap

- [ ] Real-time attendance via live webcam feed
- [ ] Export attendance reports to CSV/PDF
- [ ] Email/SMS notifications for absent students
- [ ] Parent/guardian portal
- [ ] Multi-classroom support for larger institutions

---

## Contributing

Contributions are welcome! Feel free to open an issue or submit a pull request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

<div align="center">
  <sub>Built with 💚 for smarter classrooms</sub>
  <br/>
  <sub>© 2026 AttendX — <a href="https://attendx.streamlit.app">attendx.streamlit.app</a></sub>
</div>
