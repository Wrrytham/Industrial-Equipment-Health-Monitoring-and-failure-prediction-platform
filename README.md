# Industrial Equipment Health Monitoring & Failure Prediction Platform

A full-stack web platform for real-time monitoring of industrial equipment health and AI-driven prediction of potential equipment failures. The system ingests sensor data, analyzes equipment performance trends, and surfaces actionable insights to help maintenance teams shift from reactive to proactive maintenance strategies.
---

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [Backend Setup](#backend-setup)
  - [Frontend Setup](#frontend-setup)
  - [Running with Docker](#running-with-docker)
- [Environment Variables](#environment-variables)
- [API Endpoints](#api-endpoints)
- [Architecture Overview](#architecture-overview)
- [Contributing](#contributing)
- [License](#license)

---

## Features

- **Real-Time Equipment Monitoring** — Continuously tracks sensor data (temperature, vibration, pressure, RPM, etc.) from industrial equipment and displays live status on a centralized dashboard.
- **Failure Prediction** — Uses machine learning models to analyze historical and live sensor data to predict the probability of equipment failure before it occurs.
- **Remaining Useful Life (RUL) Estimation** — Estimates how long a piece of equipment can continue operating safely before scheduled maintenance is needed.
- **Anomaly Detection** — Automatically flags unusual readings or behavioral patterns that deviate from normal equipment operation.
- **Alert & Notification System** — Triggers alerts when equipment health drops below critical thresholds or when a predicted failure event is imminent.
- **Interactive Dashboard** — A responsive frontend interface for visualizing equipment health, trends, predictions, and maintenance history.
- **Maintenance History Logging** — Keeps a structured record of past maintenance events, sensor readings, and prediction outcomes for auditing and model retraining.
- **Docker Support** — Fully containerized deployment for both backend and frontend services.

---

## Tech Stack

| Layer         | Technology                                  |
|---------------|---------------------------------------------|
| Frontend      | JavaScript, React, HTML5, CSS3              |
| Backend       | Python, Flask / FastAPI                     |
| ML / Data     | scikit-learn, Pandas, NumPy                 |
| Database      | PostgreSQL / MongoDB (configurable)         |
| Containerization | Docker, Docker Compose                   |

---

## Project Structure

```
Industrial-Equipment-Health-Monitoring-and-failure-prediction-platform/
│
├── Backend/                    # Python backend application
│   ├── app/                    # Core application logic
│   │   ├── __init__.py
│   │   ├── models/             # ML models & database schemas
│   │   ├── routes/             # API route definitions
│   │   ├── services/           # Business logic & prediction engine
│   │   └── utils/              # Utility helpers
│   ├── requirements.txt        # Python dependencies
│   ├── Dockerfile              # Backend container image
│   ├── config.py               # App configuration
│   └── main.py                 # Application entry point
│
├── Frontend/                   # React frontend application
│   ├── public/                 # Static public assets
│   ├── src/                    # React source code
│   │   ├── components/         # Reusable UI components
│   │   ├── pages/              # Page-level views (Dashboard, Equipment, etc.)
│   │   ├── services/           # API call handlers
│   │   ├── App.js              # Root React component
│   │   └── index.js            # App entry point
│   ├── package.json            # Node.js dependencies
│   ├── Dockerfile              # Frontend container image
│   └── .env                    # Frontend environment variables
│
├── docker-compose.yml          # Multi-service container orchestration
└── README.md                   # This file
```

---

## Prerequisites

Make sure you have the following installed on your machine:

- **Python** 3.8 or higher
- **Node.js** 16 or higher & **npm**
- **Docker** & **Docker Compose** (for containerized deployment)
- A running instance of **PostgreSQL** or **MongoDB** (if not using Docker)

---

## Getting Started

### Backend Setup

```bash
# 1. Navigate to the Backend directory
cd Backend

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # Linux/macOS
# venv\Scripts\activate         # Windows

# 3. Install Python dependencies
pip install -r requirements.txt

# 4. Set your environment variables (see .env section below)
cp .env.example .env

# 5. Start the backend server
python main.py
```

The backend API will be available at `http://localhost:8000` by default.

---

### Frontend Setup

```bash
# 1. Navigate to the Frontend directory
cd Frontend

# 2. Install Node.js dependencies
npm install

# 3. Configure the API base URL in .env
# Set REACT_APP_API_URL=http://localhost:8000

# 4. Start the development server
npm start
```

The frontend dashboard will be available at `http://localhost:3000` by default.

---

### Running with Docker

The simplest way to get the entire platform running is with Docker Compose:

```bash
# From the project root directory
docker-compose up --build
```

This will spin up the backend, frontend, and database services simultaneously. Once healthy:

| Service  | URL                       |
|----------|---------------------------|
| Frontend | http://localhost:3000     |
| Backend  | http://localhost:8000     |

To stop and remove containers:

```bash
docker-compose down
```

---

## Environment Variables

Create a `.env` file in the `Backend/` directory with the following variables:

```env
# Database
DB_HOST=localhost
DB_PORT=5432
DB_NAME=equipment_health_db
DB_USER=your_db_user
DB_PASSWORD=your_db_password

# Application
APP_HOST=0.0.0.0
APP_PORT=8000
SECRET_KEY=your_secret_key_here

# ML Model
MODEL_PATH=./models/trained_model.pkl
PREDICTION_THRESHOLD=0.75

# Alerts
ALERT_EMAIL=maintenance-team@company.com
SMTP_HOST=smtp.yourprovider.com
SMTP_PORT=587
SMTP_USER=your_email@company.com
SMTP_PASSWORD=your_smtp_password
```

For the `Frontend/`, set:

```env
REACT_APP_API_URL=http://localhost:8000
```

---

## API Endpoints

| Method | Endpoint                          | Description                                    |
|--------|-----------------------------------|------------------------------------------------|
| GET    | `/api/equipment`                  | List all monitored equipment                   |
| GET    | `/api/equipment/:id`              | Get details of a specific equipment unit       |
| GET    | `/api/equipment/:id/sensor-data`  | Fetch latest sensor readings for equipment     |
| GET    | `/api/equipment/:id/health`       | Get current health score and status            |
| GET    | `/api/equipment/:id/predict`      | Run failure prediction for equipment           |
| GET    | `/api/equipment/:id/rul`          | Get estimated Remaining Useful Life            |
| GET    | `/api/alerts`                     | Retrieve all active alerts                     |
| POST   | `/api/alerts/acknowledge/:id`     | Acknowledge a specific alert                   |
| GET    | `/api/maintenance/history`        | Fetch maintenance history logs                 |
| POST   | `/api/maintenance/log`            | Log a new maintenance event                    |
| GET    | `/api/dashboard/summary`          | Get aggregated health summary for dashboard    |

---

## Architecture Overview

```
┌──────────────────────────────────────────────────────┐
│                     Client Browser                   │
│              (React Dashboard — Port 3000)           │
└─────────────────────────┬────────────────────────────┘
                          │ HTTP / REST API
┌─────────────────────────▼────────────────────────────┐
│                   Backend API Server                  │
│         (Python / Flask or FastAPI — Port 8000)       │
│                                                      │
│   ┌──────────────┐   ┌───────────────────────┐       │
│   │   Routes /   │   │   ML Prediction Engine│       │
│   │  Controllers │──▶│  (scikit-learn models) │       │
│   └──────────────┘   └───────────────────────┘       │
│          │                                           │
│   ┌──────▼──────────────────┐                        │
│   │   Database Layer        │                        │
│   │ (PostgreSQL / MongoDB)  │                        │
│   └─────────────────────────┘                        │
└──────────────────────────────────────────────────────┘
```

The platform follows a clean separation of concerns: the React frontend handles all UI rendering and user interaction, communicates exclusively through REST APIs with the Python backend, which in turn manages data persistence, business logic, and ML-based predictions.

---

## Contributing

Contributions are welcome! Here's how to get involved:

1. **Fork** the repository.
2. **Clone** your fork locally.
3. Create a new branch for your feature or bug fix:
   ```bash
   git checkout -b feature/your-feature-name
   ```
4. Make your changes and commit them with a clear message:
   ```bash
   git commit -m "Add: description of your changes"
   ```
5. Push your branch to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```
6. Open a **Pull Request** against the `main` branch of the original repository.

Please ensure your code follows the  existing style and conventions used in the project.

---

## License

This project is licensed under the **MIT License**. You are free to use, modify, and distribute this software for any purpose, provided the original notice is retained.

---

*Built to bring predictive intelligence to industrial maintenance operations.*
