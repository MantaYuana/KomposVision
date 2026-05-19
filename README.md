<div align="center">

# 🌿 KomposVision

### Smart Composting & Waste Classification Assistant

![KomposVision](KomposVision.gif)

[![Expo](https://img.shields.io/badge/Expo-SDK%2054-000020?logo=expo&logoColor=white)](https://expo.dev/)
[![React Native](https://img.shields.io/badge/React%20Native-0.81-61DAFB?logo=react&logoColor=black)](https://reactnative.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![TensorFlow Lite](https://img.shields.io/badge/TensorFlow%20Lite-On--device-FF6F00?logo=tensorflow&logoColor=white)](https://www.tensorflow.org/lite)
[![FastAPI](https://img.shields.io/badge/FastAPI-Optional-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

KomposVision is an AI-assisted composting app that helps users sort waste correctly, track compost progress, and learn sustainable practices. The final mobile app lives in `komposvision_final/` and runs fully on-device with optional backend integrations.

**🏆 1st Place — IYREF 2026 (Integrated Youth Renewable Energy Festival)**

[Features](#-features) · [Architecture](#-architecture) · [Quick Start](#-quick-start) · [API Docs](#-api-documentation) · [Contributing](#-contributing)

</div>

---

## 📖 Table of Contents

- [About the Project](#-about-the-project)
- [Features](#-features)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Quick Start](#-quick-start)
- [Environment Variables](#-environment-variables)
- [API Documentation](#-api-documentation)
- [Running Tests](#-running-tests)
- [Deployment](#-deployment)
- [Contributing](#-contributing)
- [Team](#-team)
- [License](#-license)

---

## 🎯 About the Project

KomposVision empowers households and communities to compost smarter by combining:

- **On-device computer vision** to classify organic vs inorganic waste
- **Contaminant detection** to flag items that should not enter compost
- **Local progress tracking** for compost batches and materials
- **Guided learning** via a materials guide and composting tips
- **Optional online services** for chat assistance and shared progress

---

## ✨ Features

### 📷 Live Scan & Waste Classification

Point your camera at waste items and get instant results:

- Organic vs inorganic classification
- Segmentation-aware overlays for clearer feedback
- Fast on-device inference via TensorFlow Lite

### 🧪 Contamination Detection

Detect common compost contaminants early:

- Flags plastics, metals, and non-compostables
- Helps keep compost clean and odor-free

### 📈 Compost Progress Tracking

Track your compost batch over time:

- Record materials added and daily progress
- View composition summaries and history

### 📚 Materials Guide

Practical tips on what to compost and how:

- Do/Don’t recommendations
- Best practices and troubleshooting

### 🗂️ Offline-first Data

Works without a network connection:

- Local storage with WatermelonDB
- Fast retrieval for logs and batch history

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                   Expo React Native App (Final)                     │
│  Tabs: Scan · Progress · Materials · Chat                           │
└──────────────────────────────┬──────────────────────────────────────┘
                               │ On-device inference
┌──────────────────────────────▼──────────────────────────────────────┐
│                 Vision Pipeline (TensorFlow Lite)                   │
│  - Organic/Inorganic Classifier                                     │
│  - Segmentation (CN)                                                │
│  - Contaminant Detection                                            │
└──────────────────────────────┬──────────────────────────────────────┘
                               │ Results + metadata
┌──────────────────────────────▼──────────────────────────────────────┐
│                 Local Storage (WatermelonDB)                        │
│  Batches · Materials · Scans · Profiles                             │
└──────────────────────────────┬──────────────────────────────────────┘
                               │ Optional sync / AI guidance
┌──────────────────────────────▼──────────────────────────────────────┐
│          KomposVision Online Backend (FastAPI, optional)            │
│  /scan · /chat · /progress · /materials                             │
└──────────────────────────────┬──────────────────────────────────────┘
                               │
                ┌──────────────▼───────────────┐
                │  Supabase + Gemini API       │
                └──────────────────────────────┘
```

### Vision Pipeline

```
Camera Frame
    │
    ▼
┌───────────────────────────────────────────┐
│ Preprocess (resize + normalize)           │
└──────────────────┬────────────────────────┘
                   │
                   ▼
┌───────────────────────────────────────────┐
│ On-device Models (TFLite)                 │
│  - Organic/Inorganic Classifier           │
│  - Segmentation (CN ratio estimation)     │
│  - Contaminant Detector (YOLO)            │
└──────────────────┬────────────────────────┘
                   │
                   ▼
┌───────────────────────────────────────────┐
│ Compost Advisor                            │
│  - Score + recommendations                 │
└──────────────────┬────────────────────────┘
                   │
                   ▼
              UI Feedback
```

---

## 🛠️ Tech Stack

| Layer                  | Technology                                                                |
| ---------------------- | ------------------------------------------------------------------------- |
| **Mobile**             | Expo SDK 54, React Native 0.81, React 19, TypeScript 5.9                  |
| **UI**                 | NativeWind (Tailwind), Expo Router, React Navigation                      |
| **Vision**             | TensorFlow Lite, `react-native-fast-tflite`, `react-native-vision-camera` |
| **Storage**            | WatermelonDB                                                              |
| **AI**                 | YOLOv11m, YOLOv11m-seg, Gemini API                                        |
| **Backend (optional)** | FastAPI + Supabase                                                        |

---

## 📂 Project Structure

```
KomposVision/
├── komposvision_final/               # Final mobile app (Expo)
│   ├── app/                          # Screens & routes (Expo Router)
│   ├── assets/                       # Images + TFLite models
│   ├── components/                   # Reusable UI components
│   ├── database/                     # WatermelonDB schema & models
│   ├── services/                     # Vision + composting services
│   └── utils/                        # Helpers & advisors
├── KomposVision_Online_Backend/       # Optional FastAPI backend
└── komposvision-enterprise-frontend/  # Optional web dashboard
```

---

## 🚀 Quick Start

### Prerequisites

- [Node.js](https://nodejs.org/) v18+
- [Expo CLI](https://docs.expo.dev/get-started/installation/)
- Android Studio or Xcode (for native builds)

---

### 1. Install Dependencies

```bash
cd komposvision_final
npm install
```

### 2. Run the App

```bash
npm run start
```

For camera + TFLite modules, use a development build:

```bash
npm run android
```

---

## 🔧 Environment Variables

The mobile app runs fully offline and does **not** require environment variables.

If you use the optional backend, create `KomposVision_Online_Backend/backend/.env` with:

| Variable         | Description               |
| ---------------- | ------------------------- |
| `SUPABASE_URL`   | Supabase project URL      |
| `SUPABASE_KEY`   | Supabase service role key |
| `GEMINI_API_KEY` | Gemini API key            |

---

## 📚 API Documentation

When the optional backend is running, FastAPI docs are available at:

```
http://localhost:8000/docs
```

### Key Endpoints

| Method | Endpoint     | Description              |
| ------ | ------------ | ------------------------ |
| `GET`  | `/health`    | Service health check     |
| `POST` | `/scan`      | Analyze a waste image    |
| `POST` | `/chat`      | Composting Q&A assistant |
| `GET`  | `/progress`  | Compost progress summary |
| `GET`  | `/materials` | Materials guide          |

---

## 🧪 Running Tests

No automated tests are configured yet. Linting is available via:

```bash
cd komposvision_final
npm run lint
```

---

## 🚢 Deployment

### Mobile

Use EAS to create store-ready builds:

```bash
cd komposvision_final
eas build -p android
```

### Backend (optional)

```bash
cd KomposVision_Online_Backend
uvicorn backend.main:app --reload
```

---

## 🤝 Contributing

We welcome contributions! Please follow these steps:

### 1. Fork & Branch

```bash
git fork https://github.com/your-org/komposvision.git
git checkout -b feat/your-feature-name
```

### 2. Development Workflow

```bash
cd komposvision_final
npm run start
```

### 3. Code Style

- Keep components small and focused.
- Prefer hooks for stateful logic.
- Match existing file structure in `components/`, `services/`, and `database/`.

### 4. Commit Convention

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add compost batch reminders
fix: handle empty scan results
docs: update vision pipeline diagram
```

### 5. Open a Pull Request

- Target the `main` branch
- Include a short description of _what_ and _why_
- Reference any related issues

---

## 👥 Team

KomposVision was built with ❤️ by:

| Name                           | Role                  |
| ------------------------------ | --------------------- |
| [Manta Yuana](https://github.com/mantayuana) | Backend & Project Manager |
| [Kadek Pindra](https://github.com/KadekPindra) | Frontend & Integration |
| [Nova Andini](https://github.com/novaandini) | AI & Data Engineering |
| [Dewa Surya](https://github.com/arisuryaa) | Frontend & UI/UX Designer |

---

## 📄 License

This project is licensed under the **MIT License**. See [LICENSE](LICENSE) for details.

---

<div align="center">

Built for sustainable communities · KomposVision

</div>
