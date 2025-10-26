```markdown
# Face-Pulse-AI

🧠🚆 FacePulse — Face Recognition System for Server Room  
A web-based, contactless in-time / out-time logging and monitoring system using DeepFace (Facenet), Django and PostgreSQL.

Project report submitted as a part of Summer Internship/Training at RDSO (Research Designs and Standards Organisation), Ministry of Railways, Government of India.

---

## Project overview 📘

FacePulse is a prototype biometric logging system that:

- 📸 Allows user registration via facial image upload or webcam capture  
- 🔗 Generates and stores facial embeddings (Facenet via DeepFace)  
- 🌐 Performs real-time identity verification through a browser webcam (MediaStream API)  
- 🕒 Automatically records entry (in-time) and exit (out-time) timestamps and duration  
- 💾 Stores logs in a relational database (PostgreSQL recommended; SQLite used for development)

The system is intended for secure areas such as server rooms where contactless, auditable entry/exit logging is required.

---

## Problem statement / Challenges faced ❗

Organizations relying on traditional logging or access-control methods (manual registers, RFID cards, or legacy scanners) face several practical problems:

- ✍️ Manual dependency — paper registers are error-prone, hard to audit and enable impersonation.  
- ⌛ Time inefficiency — card swipes / manual entries add delay and create queues for high-traffic areas.  
- 🔒 Tamper & security risk — physical tokens/cards can be lost, shared, or cloned; paper records can be altered.  
- 📉 Poor scalability & integration — legacy systems rarely integrate with centralized analytics or cloud services.  
- 🌙 Environmental & hygiene constraints — fingerprint scanners require contact; in pandemic contexts contactless is preferred.  
- 🧩 Deployment complexity — installing and maintaining dedicated hardware across many locations increases cost and operational overhead.

These issues motivated the design of a contactless, centralized, and auditable logging solution that reduces human error and operational friction.

---

## Proposed solution 💡

FacePulse — a web-accessible, FaceNet-based recognition and logging system — addresses the problems above with the following approach:

- Contactless authentication: users register once (image or webcam) and can authenticate via browser webcam (MediaStream API), eliminating physical tokens.  
- Embedding-based identity: FaceNet embeddings (via DeepFace) represent users as compact vectors stored server-side; matching uses cosine/Euclidean distance for reliable verification.  
- Automated logging: Entry and exit timestamps are recorded automatically; durations are computed and stored for analytics.  
- Centralized datastore: PostgreSQL (recommended for production) stores user metadata and logs, enabling reporting and audits.  
- Minimal hardware footprint: runs on standard web browsers with webcam support — avoids expensive fingerprint or card infrastructure.  
- Security-first design: server-side matching, HTTPS transport, RBAC-ready admin views, and support for encryption/retention policies.

Benefits:
- Faster check-ins and check-outs, reduced queues  
- Better auditability and tamper resistance  
- Easier scalability and integration with dashboards and analytics  
- Hygiene-friendly, contactless operation

---

## Key features ✅

| Feature | Description |
|---|---|
| 🆔 Face registration | Register users with an image or webcam snapshot; embeddings stored server-side |
| 📷 Real-time recognition | Browser webcam capture via MediaStream → server-side DeepFace matching |
| 📏 Matching metrics | Cosine similarity / Euclidean distance thresholding for identity verification |
| 📝 Automated logging | Automatic entry/exit timestamps and duration calculation per session |
| 🖥️ Web UI | Lightweight HTML5/JS templates for registration, recognition and admin |
| 🧩 Modular backend | Django models/views/helpers for easy extension and reporting |

---

## Tech stack 🧩

| Component | Technology | Notes |
|---:|---|---|
| Backend | Django (Python) | MVT architecture, ORM, admin |
| Face recognition | DeepFace (Facenet) | Embedding generation & verification |
| Image processing | OpenCV, NumPy, Pillow | Preprocessing, decoding and resizing |
| ML Backend | TensorFlow / Keras | DeepFace model runtime |
| DB (dev) | SQLite | Lightweight local testing |
| DB (prod) | PostgreSQL | Recommended for production concurrency |
| Frontend | HTML5, CSS3, JavaScript | MediaStream API for webcam |
| Reporting | python-docx | Document generation helpers present |

---

## Quick setup (development) 🛠️

### Prerequisites

| Tool | Purpose | Version / Notes |
|---|---|---|
| Python | Runtime | 3.8+ (match TF/DeepFace compatibility) |
| pip | Package installer | Latest |
| virtualenv / venv | Isolate deps | Recommended |
| Git | Version control | — |
| (Prod) PostgreSQL | Production DB | Optional for dev |
| (Prod) TLS | Camera access over HTTPS | Required in production |

### Basic steps (summary)

| Step | Command / Action |
|---:|---|
| Clone repo | git clone <repo-url> |
| Create venv | python -m venv venv |
| Activate venv | source venv/bin/activate (Windows: venv\Scripts\activate) |
| Install deps | pip install -r requirements.txt |
| Configure env | Copy `.env.example` → `.env`, set DB and MEDIA paths |
| Migrate DB | python manage.py migrate |
| Create admin | python manage.py createsuperuser |
| Start server | python manage.py runserver |

Important notes:
- 🔒 Browsers require secure origins for camera access — serve over HTTPS in production. Localhost often allowed for testing.
- ⚙️ dlib/TensorFlow/OpenCV installs can be OS-sensitive — consult OS-specific guides or prebuilt wheels.

---

## Usage ▶️

| Page | Endpoint | Action |
|---|---:|
| Registration | `/register/` | Upload image or capture webcam → embedding generated & saved |
| Recognition | `/recognize/` | Capture snapshot → compare embeddings → log entry/exit |
| Admin | `/admin/` | Manage users, logs, export reports |

Behavior:
- If no active session → record entry_time ⏱️  
- If active session exists → record exit_time, calculate duration, mark session complete ✅

---

## Project structure (high level) 📁

| Path | Purpose |
|---|---|
| manage.py | Django project management |
| myproject/ | Django settings, URL routing, WSGI/ASGI |
| my_app/ | App logic: models.py, views.py, forms, templates, word_helpers.py |
| my_app/templates/ | data_entry.html, download_doc.html UI templates |
| my_app/word_helpers.py | Utilities for .docx generation |
| media/ | Uploaded images and generated documents |
| static/ | Static assets: logos, css, js |
| requirements.txt | Pinned Python packages |

---

## Data & security notes 🔐

| Concern | Recommendation |
|---|---|
| Storage | Store embeddings (vectors) instead of raw images where possible; keep images in protected media with restricted filesystem permissions |
| Transmission | Always use HTTPS in production |
| Encryption | Use DB/disk encryption or encrypt embeddings (Fernet/AES) for sensitive deployments |
| Access control | RBAC for admin and reporting features |
| Privacy / Retention | Implement deletion/retention policies and consent flows as required by regulations |

---

## Results & evaluation (summary) 📈

| Metric | Observed / Recommendation |
|---|---|
| Recognition accuracy | High with Facenet in controlled tests; real-world varies with lighting/camera |
| Avg inference time | ≈ 180 ms/frame on tested dev hardware |
| FAR / FRR | Depends on dataset; tune thresholds per deployment |
| Deployment notes | Cache/load model on startup; preprocess images for low-light improvements |

---

## Known issues & limitations ⚠️

| Issue | Impact | Mitigation |
|---|---|---|
| Dependency complexity | Installation failures on some OS/envs | Use pinned wheels, document OS-specific steps, provide requirements.txt |
| Low-light / occlusion | Degraded recognition | Improve preprocessing, use better cameras or IR illumination |
| Spoofing risk | Photos/videos may be used to spoof | Add liveness detection in future |
| UI is minimal | Admin/analytics UX limited | Implement dashboard & charts in future iterations |

---

## Future enhancements 🚀

| Feature | Benefit | Priority |
|---|---|---:|
| Mobile app (Flutter/React Native) | Field / on-the-go check-ins | Medium |
| Liveness detection | Prevent spoof attacks | High |
| Analytics dashboard | Visualize peaks, durations, export reports | Medium |
| Multi-org & RBAC | Serve multiple departments / orgs | Medium |
| Biometric encryption | Stronger data protection | High |


---

## License 📝

This repository does not include a license file by default. Add an appropriate license (e.g., MIT) if you wish to allow reuse. If needed, include a `LICENSE` file.
```
