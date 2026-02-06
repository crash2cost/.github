# Crash2Cost

AI-powered vehicle damage assessment and repair cost estimation system.

## Overview

Crash2Cost analyzes vehicle damage photos using a three-stage ML pipeline — detection, classification, and cost estimation — to automatically identify damaged areas, classify severity, and predict repair costs.

## Architecture

The system uses a microservices architecture:

```
Frontend (React) → API Gateway (Spring Cloud) → Auth Service / Report Service
                                               → ML Server (FastAPI)
                                                    ↓
                                          Detection (YOLOv8)
                                          Classification (ResNet)
                                          Cost Estimation (Random Forest)
```

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React 19, TypeScript, Vite, Tailwind CSS |
| API Gateway | Spring Cloud Gateway (Port 8080) |
| Auth Service | Spring Boot, Spring Security, JWT (Port 8002) |
| Report Service | Spring Boot, MongoDB (Port 8003) |
| ML Server | FastAPI, PyTorch, scikit-learn (Port 8004) |
| Database | MongoDB (Port 27017) |

## Project Structure

```
crash2cost/
├── frontend/client/   # React web application
├── backend/
│   ├── api-gateway/   # Spring Cloud Gateway
│   ├── auth-service/  # JWT authentication service
│   └── report-service/# Assessment report service
├── machines/          # ML pipeline & models
│   ├── detection-model/   # YOLOv8 damage detection
│   ├── severity-model/    # ResNet damage classification
│   ├── cost-model/        # Repair cost estimation
│   └── ml-server/         # FastAPI inference server
└── Scripts/           # Service automation scripts
```

## ML Pipeline

1. **Detection** — YOLOv8 identifies damaged regions via bounding boxes
2. **Classification** — ResNet classifies 7 damage types (bumper dent/scratch, door dent/scratch, glass shatter, head lamp, tail lamp) with severity scores (1–5)
3. **Cost Estimation** — Predicts repair cost in ILS based on part type, severity, and car segment

## Getting Started

### Prerequisites

- Java 25
- Node.js & npm
- Python 3 with virtual environment
- Docker (for MongoDB)

### Running Services

Use the automation scripts in `Scripts/` to start all services:

```bash
./Scripts/start-all.sh
```

Or start services individually:

```bash
# MongoDB
docker compose up -d

# Backend services
cd backend/api-gateway && ./mvnw spring-boot:run
cd backend/auth-service && ./mvnw spring-boot:run
cd backend/report-service && ./mvnw spring-boot:run

# ML server
cd machines/ml-server && python main.py

# Frontend
cd frontend/client && npm run dev
```
