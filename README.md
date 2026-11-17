# 🌿 KrishiVora (कृषिवोरा)
## AI-Powered Agricultural Quality Assessment, Market Price Prediction and Post-Harvest Advisory System

> **B.Tech Major Project Demonstration & Academic Submission**  
> **Platform Status:** Operational & Fully Functional (`http://127.0.0.1:8000`)

---

## 1. Project Overview & Problem Statement
KrishiVora is an integrated agricultural decision-support platform designed to empower smallholder farmers, FPOs (Farmer Producer Organizations), and mandis with data-driven insights across the harvest-to-sale lifecycle.

Agricultural stakeholders face three major bottlenecks:
1. **Subjective Quality Evaluation:** Produce quality is graded visually without objective metrics, leading to arbitrary price deductions by intermediaries.
2. **Market Price Volatility:** Farmers lack visibility into multi-day mandi arrival price movements, causing distress sales during seasonal gluts.
3. **Severe Post-Harvest Losses:** Inadequate storage protocols and delayed risk detection cause up to 25–35% spoilage before produce reaches consumers.

KrishiVora integrates computer vision, machine learning time-series forecasting, and science-backed agronomic guidance into a unified platform.

---

## 2. Core Modules Architecture

### 🔬 Module 1: AI Produce Quality & Freshness Assessment
- **Live Mobile Camera Stream:** Real-time WebRTC camera viewfinder with HUD reticle and animated laser scanline, optimized for field smartphones.
  - **Rear / Field Camera Lens Default:** Prioritizes `facingMode: "environment"` to instantly target crops and fruit batches.
  - **1-Tap Camera Switcher:** Toggle seamlessly between front and rear cameras.
  - **Instant Shutter Snapshot:** Captures high-resolution frame directly to canvas, encodes to JPEG blob, and submits to the MobileNetV3 AI engine.
  - **Native Mobile Camera Fallback:** `<input capture="environment">` fallback ensuring compatibility across all mobile browsers and WebView containers.
- **Backbone Architecture:** MobileNetV3-Small deep convolutional neural network optimized for low-latency inference on edge/mobile devices.
- **Dual Classifier Heads:**
  - `fruit_classifier`: Linear layer (576 → 256 → 8 classes) classifying **Apple, Banana, Grape, Guava, Jujube, Orange, Pomegranate, Strawberry**.
  - `freshness_classifier`: Sigmoid classification head (576 → 128 → 1) predicting continuous **Fresh Probability vs. Spoiled Probability**.
- **Model Performance:** **99.38% Fruit Identification Accuracy**, **99.17% Freshness Classification Accuracy** on the validation test set.
- **Explainable Quality Grading:** Generates standardized commercial quality grades (Grade A, B, C, D) alongside surface defect detection heuristics.

### 📈 Module 2: Agricultural Market Price Forecasting
- **Dataset Integration:** Simulated Indian APMC market arrival patterns modeling 10 commodities across 10 major agricultural states (Maharashtra, Karnataka, Tamil Nadu, Uttar Pradesh, Madhya Pradesh, Gujarat, Rajasthan, Punjab, West Bengal, Andhra Pradesh).
- **ML Algorithm:** Random Forest Regressor incorporating:
  - Calendar features: Day of year, month, day of week, week of year.
  - Lag features: 7-day, 14-day, and 30-day price lags.
  - Rolling metrics: 7-day and 30-day moving averages and volatility bands.
- **Evaluation Metrics:** Evaluated with chronological train/test split (80/20):
  - **R² Score:** ~0.89+
  - **Mean Absolute Error (MAE):** Provided in INR/Quintal for every prediction.
  - **Root Mean Squared Error (RMSE):** Real-time validation display.
- **Visualization:** Interactive Chart.js timeline with historical 90-day arrivals and dashed future forecasting curves.

### 🛡️ Module 3: Smart Post-Harvest Storage & Handling Advisory
- **Agronomic Knowledge Base:** Scientifically verified storage parameters covering 14+ fruit and vegetable crops.
- **Multi-Factor Spoilage Risk Engine:**
  $$\text{Risk Score} = 0.40 \cdot P_{\text{spoil}} + 0.25 \cdot \text{AgeFactor} + 0.20 \cdot \Delta\text{Temp} + 0.15 \cdot \Delta\text{Humidity}$$
- **Threshold Comparison:** Real-time variance comparison between actual warehouse conditions and agronomic optima for temperature, relative humidity, respiration rate, and ethylene sensitivity.

### 📡 Module 1E (Research): Non-Destructive Internal Rot Inspection
- Comprehensive technical analysis of **Near-Infrared (NIR) Spectroscopy, X-Ray Radiography, Hyperspectral Imaging (HSI), and Ultrasound Resonance**.
- Realistic physical constraints: Explains why standard RGB cameras cannot see beneath the peel, providing an extensible ingestion API pipeline for hardware integration.

---

## 3. UI/UX Design System
- **Theme:** Modern Light Mode SaaS Dashboard.
- **Primary Color:** Deep Forest Green (`#102A22`) & Moss Green (`#1F3D32`).
- **Accent Color:** Fresh Lime Green (`#A3E635`) with subtle glow effects.
- **Typography:** Plus Jakarta Sans.
- **3D Dimensionality:** Layered soft elevation shadows with responsive mobile navigation drawer.

---

## 4. Quick Start & Execution

### Prerequisites
- Python 3.10+ (Tested on Python 3.14 on macOS Apple Silicon MPS / CPU)
- Virtual environment recommended

### Installation & Launch
```bash
# 1. Navigate to project root
cd /Users/gangadharreddy/Downloads/KRISHIVORA

# 2. Launch using startup script
./run.sh

# Or launch directly with uvicorn
python3 -m uvicorn backend.main:app --host 127.0.0.1 --port 8000 --reload
```

Open your browser at:
👉 **`http://127.0.0.1:8000`**

---

## 5. API Endpoints Reference

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/health` | Server health check and version info |
| `POST` | `/api/analyze` | Multipart image inspection (produce ID, freshness, grade) |
| `POST` | `/api/risk` | Storage risk score calculation from warehouse inputs |
| `GET` | `/api/market/commodities` | List available commodities |
| `GET` | `/api/market/states` | List covered states |
| `GET` | `/api/market/districts/{state}` | Get districts for selected state |
| `GET` | `/api/market/markets/{state}/{district}` | Get APMC mandis for selected district |
| `POST` | `/api/market/predict` | Multi-day market price prediction with ML metrics |
| `POST` | `/api/advisory/recommend` | Comprehensive agronomic storage and risk guidance |
| `GET` | `/api/advisory/weather/{city}` | Live weather integration (OpenWeatherMap) |
# KRISHIVORA
