<div align="center">




# SmartCity — AI-Powered Urban Issue Detection & Reporting System

[![React](https://img.shields.io/badge/React-18-61DAFB?style=flat-square&logo=react)](https://reactjs.org/)
[![Node.js](https://img.shields.io/badge/Node.js-20-339933?style=flat-square&logo=node.js)](https://nodejs.org/)
[![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python)](https://python.org/)
[![YOLOv8](https://img.shields.io/badge/YOLOv8-Ultralytics-FF6B35?style=flat-square)](https://ultralytics.com/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=flat-square&logo=docker)](https://docker.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-336791?style=flat-square&logo=postgresql)](https://postgresql.org/)

**Final Year Project · BSCS · Hamza Shahzad**

*SmartCity is an AI-powered urban issue reporting platform that helps citizens submit reports with photos and GPS, while city teams manage, classify, and resolve them through a unified dashboard.*

[Overview](#overview) · [Demo](#demo) · [Screenshots](#screenshots) · [Features](#features) · [Architecture](#architecture) · [Tech Stack](#tech-stack) · [ML Model](#machine-learning-model) · [Getting Started](#getting-started) · [Demo Accounts](#demo-accounts)

</div>

---

## Quick Start

```bash
git clone https://github.com/hamza01055/smartcity-ai.git
cd smartcity-ai
cp .env.example .env
docker compose up --build
```

Open the app once the containers are running:
- Frontend: http://localhost:5173
- Backend API: http://localhost:3333
- ML docs: http://localhost:8000/docs

For full setup steps, environment variables, and troubleshooting, see [docs/SETUP.md](docs/SETUP.md).

## Overview

SmartCity replaces the slow, manual complaint workflow found in most municipal systems. A citizen photographs a pothole, a broken traffic light, or an overflowing garbage bin — the system detects and categorizes the issue with computer vision and routes it to the appropriate city department within seconds.

### The Problem

Traditional municipal complaint systems are slow: a citizen files a complaint, someone reads it, someone categorizes it, and someone routes it. Each hand-off adds delay and opportunity for error.

### The Solution

SmartCity automates the detection and categorization step, so every report is classified the moment it is submitted:

1. **Report** — a citizen uploads a photo with GPS location
2. **Classify** — YOLOv8 instantly identifies and categorizes the issue
3. **Dispatch** — an administrator sees it on a live map dashboard and assigns a worker
4. **Resolve** — the field worker completes the task and updates the status in real time

---

## Demo

![Demo](assets/demo.gif)

**Full video walkthrough:** [Watch on YouTube](https://www.youtube.com/watch?v=qiU9LLmWEfM)

---

## Screenshots

### Citizen Report Page
> Citizens upload a photo and share their GPS location. YOLOv8 automatically detects and classifies the urban issue — no written description required.

<img width="960" height="540"
<img width="960" height="540" alt="Citizen report page" src="https://github.com/user-attachments/assets/ec34dc15-780a-4c1d-aa28-ce552afd7272" />

### Track Report — Search
> Citizens track their submitted report using a unique tracking ID.

<img width="960" height="540" alt="Track report search" src="https://github.com/user-attachments/assets/2e870599-bf8a-4c39-83a5-acf3372ecd1d" />

### Track Report — Status View
> A real-time 5-step progress tracker showing AI confidence score, category, location, and assigned worker.

<img width="960" height="540" alt="Report status view" src="https://github.com/user-attachments/assets/4d03fa3a-cef7-4055-8054-40751e3bcbb4" />

### Field Worker Portal
> Field workers see only their assigned tasks and can advance each task's status through to resolution.

<img width="960" height="540" alt="Field worker portal" src="https://github.com/user-attachments/assets/c6bfd78c-a88c-4ee4-81cb-8fb0ad32a9ae" />

### Admin Dashboard
> Live map with color-coded report markers, KPI cards, and a report management table with CSV export.

<img width="960" height="540" alt="Admin dashboard" src="https://github.com/user-attachments/assets/ca805feb-390f-4609-870f-078f8c1c3432" />

### Analytics Page
> Trend charts, category breakdown, AI confidence scores, an issue heatmap, and a worker leaderboard.

<img width="960" height="540" alt="Analytics page" src="https://github.com/user-attachments/assets/c6c56857-f9c3-4f73-b468-164277a5e6ea" />

<details>
<summary><b>More screenshots</b></summary>
<br/>

<img width="960" height="540" alt="Additional screenshot 1" src="https://github.com/user-attachments/assets/b96486f1-75ac-4011-9a06-2b3d16567192" />
<img width="960" height="540" alt="Additional screenshot 2" src="https://github.com/user-attachments/assets/38412b03-8e7d-4f54-9646-fcb8bc0cd7a1" />
<img width="960" height="540" alt="Additional screenshot 3" src="https://github.com/user-attachments/assets/b27f8821-bd8a-4d64-b264-5021968a7b2c" />

</details>

---

## Features

### Citizen Portal
| Feature | Description |
|---------|-------------|
| Photo Upload | Drag-and-drop or camera capture (HEIC/JPG/PNG/WEBP, max 10 MB) |
| GPS Auto-detect | One-click browser geolocation |
| AI Classification | No description needed — YOLOv8 reads the photo automatically |
| Status Tracking | Real-time 5-step progress tracker by report ID |

### Admin Dashboard
| Feature | Description |
|---------|-------------|
| Live Map | Leaflet map with reports color-coded by category |
| Assign & Dispatch | Set worker, department, and priority from a side panel |
| Filter & Export | Filter by category/status; one-click CSV export |
| KPI Cards | Total / Pending / Reviewed / Assigned / Resolved at a glance |

### Analytics Page
| Feature | Description |
|---------|-------------|
| Trend Chart | Reports per day for the last 7 days |
| Category Breakdown | Bar chart by issue type with AI confidence scores |
| Heatmap | Leaflet heatmap of report hot-spots |
| Worker Leaderboard | Completed vs. active tasks per field worker |
| Avg. Resolution Time | Mean hours from submission to resolution |

### Field Worker Portal
| Feature | Description |
|---------|-------------|
| Personal Task List | Shows only tasks assigned to the logged-in worker |
| Status Advancement | Advance through Assigned → In Progress → Resolved |
| Completion Panel | Submit notes and an optional completion photo |

### Authentication
| Feature | Description |
|---------|-------------|
| Login Page | Professional layout with demo-credential quick-fill |
| Registration | Full name, username, password-strength meter, role selection |
| Offline Fallback | Works without the backend using a localStorage credential store |
| Session Persistence | Auth state survives page refresh |

---

## Architecture

```
  Citizen / Field Worker
    (React + Tailwind)
          │
          │  POST /api/report  (photo + GPS)
          ▼
  ┌─────────────────────┐
  │  Backend            │──── PostgreSQL + PostGIS  (reports, geo-queries)
  │  (Node.js/Express)  │──── Redis + BullMQ        (inference job queue)
  └─────────────────────┘
          │  enqueue job
          ▼
  ┌─────────────────────┐
  │  ML Worker          │
  │  (Node.js/BullMQ)   │
  └─────────────────────┘
          │  POST /predict
          ▼
  ┌─────────────────────┐
  │  ML Service         │──── category + confidence + bounding box
  │  (FastAPI/YOLOv8)   │
  └─────────────────────┘
          │
          ▼
  Report Updated → Admin Dashboard
  (Live map · Analytics · Dispatch)
```

**Flow:** the backend accepts a report and enqueues an inference job in Redis. The worker consumes the job, calls the ML service's `/predict` endpoint, and writes the classification (category, confidence, bounding box) back to PostgreSQL. The admin dashboard reads the updated report and displays it on the live map.

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | React 18, Vite, TypeScript, Tailwind CSS v3 |
| **UI Style** | Dark cyberpunk / glassmorphism with custom Tailwind color tokens |
| **Maps** | React-Leaflet, OpenStreetMap tiles, CircleMarker heat overlays |
| **Forms** | Formik + Yup validation |
| **Backend** | Node.js 20, Express 4, Multer (file uploads) |
| **Queue** | Redis + BullMQ (producer in backend, consumer in worker) |
| **Database** | PostgreSQL 15 + PostGIS extension |
| **ML Service** | Python 3.11, FastAPI, Ultralytics YOLOv8 |
| **Model** | YOLOv8m, custom-trained on 3 urban classes |
| **Deployment** | Docker Compose (6 containers) |

---

## Machine Learning Model

The detection model is a **YOLOv8m** network trained on a custom dataset of roughly 1,500 labeled images across three urban issue classes:

| Class | Description |
|-------|-------------|
| `Pothole` | Damaged road surfaces |
| `Traffic_Light` | Traffic signal infrastructure |
| `Waste_Container` | Garbage bins and waste accumulation |

**Training:** 100 epochs on a Google Colab T4 GPU.

### Validation Results

| Metric | Score |
|--------|-------|
| mAP@50 | **0.73** |
| mAP@50–95 | 0.41 |
| Precision | **0.82** |
| Recall | 0.70 |

> To retrain the model or add new classes, use `notebooks/yolov8_training.ipynb`, replace `best.pt` in `ml_service/weights/`, and restart the ML container.

---

## Project Structure

```
smart-city-project/
├── frontend/                        React + TypeScript app
│   └── src/
│       ├── pages/
│       │   ├── LoginPage.tsx        Sign-in with demo-credential quick-fill
│       │   ├── RegisterPage.tsx     Account creation with password-strength meter
│       │   ├── ReportPage.tsx       Citizen photo + GPS submission form
│       │   ├── StatusPage.tsx       5-step progress tracker by report ID
│       │   ├── DashboardPage.tsx    Admin map, filters, dispatch panel, CSV export
│       │   ├── AnalyticsPage.tsx    KPIs, trend chart, heatmap, worker leaderboard
│       │   └── FieldWorkerPage.tsx  Task list, status advancement, completion panel
│       ├── components/
│       │   └── SmartCityHero.tsx    Canvas 3D animated city (HTML5, mouse-reactive)
│       ├── contexts/
│       │   └── AuthContext.tsx      Auth state, localStorage persistence
│       └── lib/
│           └── api.ts               Axios client, Report type, color maps
│
├── backend/                         Express API + BullMQ producer
├── ml_service/                      FastAPI + YOLOv8 inference
│   ├── app/main.py                  POST /predict endpoint
│   └── weights/best.pt              Trained YOLOv8m weights
├── worker/                          BullMQ consumer → calls ML → updates DB
├── notebooks/
│   └── yolov8_training.ipynb        Colab training notebook
├── prepare_dataset.py               Dataset cleaning + YOLO train/val split
├── docker-compose.yml
└── .env.example                     Environment configuration template
```

---

## Getting Started

For complete setup and deployment instructions, see [docs/SETUP.md](docs/SETUP.md).

### Quick start with Docker

```bash
git clone https://github.com/hamza01055/smartcity-ai.git
cd smartcity-ai
cp .env.example .env
docker compose up --build
```

Open the app at:

- http://localhost:5173 — frontend
- http://localhost:3333 — backend API
- http://localhost:8000/docs — ML service docs

### Manual setup

If you prefer to run the services locally, follow the detailed steps in [docs/SETUP.md](docs/SETUP.md).

### Environment configuration

```env
DB_HOST=postgres
DB_PORT=5432
DB_USER=smartcity
DB_PASSWORD=smartcity
DB_DATABASE=smartcity

REDIS_HOST=redis
REDIS_PORT=6379

ML_SERVICE_URL=http://ml_service:8000
USE_REAL_MODEL=true
MODEL_PATH=/app/weights/best.pt

VITE_API_URL=http://localhost:3333
```

> ⚠️ Never commit your `.env` file. Use `.env.example` as a template.

---

## Demo Accounts

| Username | Password | Role |
|----------|----------|------|
| `admin` | `admin123` | Admin |
| `hamza` | `hamza123` | Admin |
| `ahmed` | `ahmed123` | Field Worker |
| `sara` | `sara123` | Field Worker |
| `usman` | `usman123` | Field Worker |

---

## Roadmap

- [ ] Real-time dashboard updates via WebSockets
- [ ] JWT-based stateless authentication
- [ ] Cloud deployment (AWS ECS / Railway / Render)
- [ ] Push notifications to field workers
- [ ] Extended model classes: flooding, fire, illegal dumping
- [ ] Mobile-native app (React Native) for field workers

---

## Author

<div align="center">

**Hamza Shahzad**
Final Year Project · BSAI

[![GitHub](https://img.shields.io/badge/GitHub-hamza01055-181717?style=flat-square&logo=github)](https://github.com/hamza01055)
[![Email](https://img.shields.io/badge/Email-hamzashahzad78374@gmail.com-EA4335?style=flat-square&logo=gmail)](mailto:hamzashahzad454545@gmail.com)

</div>
