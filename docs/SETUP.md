# Setup and Deployment Guide

This project can be run in two ways:

- Docker Compose for the full stack (recommended)
- Manual local development for debugging and iteration

## 1. Prerequisites

- Docker Desktop or Docker Engine
- Node.js 20+
- Python 3.11+
- Git
- A local copy of the trained model weights at `ml_service/weights/best.pt`

## 2. Clone and configure

```bash
git clone https://github.com/hamza01055/smartcity-ai.git
cd smartcity-ai
cp .env.example .env
```

Review the values in `.env` and keep the defaults unless you need to change ports or model settings.

## 3. Run with Docker Compose (recommended)

```bash
docker compose up --build
```

Once the services are running, open:

- Frontend: http://localhost:5173
- Backend API: http://localhost:3333
- ML docs: http://localhost:8000/docs

Useful commands:

```bash
docker compose logs -f backend
docker compose logs -f worker
docker compose down
```

## 4. Run manually without Docker

### Terminal 1 — Frontend

```bash
cd frontend
npm install
npm run dev
```

### Terminal 2 — Backend

```bash
cd backend
npm install
npm run dev
```

### Terminal 3 — ML service

```bash
cd ml_service
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### Terminal 4 — Worker

```bash
cd worker
npm install
npm run dev
```

You will also need local PostgreSQL with PostGIS and Redis running, or a Dockerized version of those services.

## 5. Environment variables

The main variables are:

```env
POSTGRES_USER=smartcity
POSTGRES_PASSWORD=smartcity
POSTGRES_DB=smartcity

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

## 6. Demo accounts

| Username | Password | Role |
|----------|----------|------|
| admin | admin123 | Admin |
| hamza | hamza123 | Admin |
| ahmed | ahmed123 | Field Worker |
| sara | sara123 | Field Worker |
| usman | usman123 | Field Worker |

## 7. Troubleshooting

- If the frontend cannot reach the backend, confirm `VITE_API_URL`.
- If the ML service fails, verify that `ml_service/weights/best.pt` exists.
- If the worker cannot connect, make sure Redis and Postgres are healthy.
- If Docker reports a port conflict, stop the existing service using that port.
