# TRAFFIX AI - Intelligent Traffic Management System

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.10+-green.svg)](https://python.org)
[![React](https://img.shields.io/badge/React-18+-61DAFB.svg)](https://reactjs.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-009688.svg)](https://fastapi.tiangolo.com)

> A real-time, AI-powered traffic optimization dashboard for Mumbai's road network using Game Theory (Nash Equilibrium), Machine Learning, and Explainable AI.

---

## 📸 Dashboard Preview

### Current State (Nash Equilibrium)
*Shows inefficient routing with PoA > 1.0*

<img width="1919" height="907" alt="Screenshot 2026-02-08 120725" src="https://github.com/user-attachments/assets/da4b862d-b1d9-40e0-a42c-6754c2113906" />



### Optimized State (System Optimum)
*Shows AI-optimized routing with PoA = 1.0*

<img width="1919" height="915" alt="Screenshot 2026-02-08 120735" src="https://github.com/user-attachments/assets/47be8f2d-a7c8-490d-8757-2449606293f6" />


---


## 🚀 Features

### Core Capabilities
- **🗺️ Interactive Map Visualization**: Real-time traffic flow on Mumbai's road network using Leaflet.js
- **🎯 Route Optimization**: Calculate optimal paths using Dijkstra's algorithm with congestion-aware weights
- **📊 Dynamic Metrics**: Live calculation of travel costs (₹), vehicle throughput, and system latency
- **🤖 ML Traffic Prediction**: Random Forest model predicting congestion based on time, weather, and events
- **💡 AI Explainability Engine**: LLM-powered insights explaining traffic patterns and recommendations

### Technical Highlights
- **Game Theory**: Price of Anarchy (PoA) calculation comparing Nash Equilibrium vs System Optimum
- **BPR Cost Function**: Bureau of Public Roads formula for realistic travel time estimation
- **Mechanism Design**: Toggle between "Current" (selfish routing) and "Optimized" (socially optimal) states
- **INR Localization**: All monetary values displayed in Indian Rupees (₹)

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        FRONTEND (React + Vite)                   │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐  │
│  │  Leaflet    │  │  Metric     │  │  AI Explainability      │  │
│  │  Map        │  │  Cards      │  │  Console                │  │
│  └─────────────┘  └─────────────┘  └─────────────────────────┘  │
└───────────────────────────┬─────────────────────────────────────┘
                            │ REST API (Axios)
┌───────────────────────────▼─────────────────────────────────────┐
│                        BACKEND (FastAPI)                         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐  │
│  │  Traffic    │  │  ML         │  │  AI Insight             │  │
│  │  Status API │  │  Inference  │  │  (Featherless AI)       │  │
│  └─────────────┘  └─────────────┘  └─────────────────────────┘  │
│  ┌─────────────┐  ┌─────────────┐                               │
│  │  Game       │  │  Path       │                               │
│  │  Theory     │  │  Finder     │                               │
│  └─────────────┘  └─────────────┘                               │
└───────────────────────────┬─────────────────────────────────────┘
                            │
┌───────────────────────────▼─────────────────────────────────────┐
│                        DATA LAYER                                │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │  traffic_data_simulated.csv (48 edges, 20 nodes, 24h data)  ││
│  │  Columns: timestamp, u, v, flow, speed, congestion_level,   ││
│  │           rain_intensity, visibility, temperature, etc.     ││
│  └─────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
```

---

## 📁 Project Structure

```
Datathon_PS1/
├── backend/
│   ├── main.py              # FastAPI server, all endpoints
│   ├── ml_inference.py      # ML prediction module (Random Forest)
│   ├── train_model.py       # Script to train the ML model
│   ├── game_theory.py       # PoA and Nash calculations
│   ├── models/
│   │   ├── traffic_model.joblib
│   │   └── label_encoders.joblib
│   └── requirements.txt
├── frontend/
│   ├── src/
│   │   ├── App.jsx          # Main dashboard component
│   │   ├── main.jsx         # React entry point
│   │   ├── ErrorBoundary.jsx
│   │   └── index.css        # Tailwind + custom styles
│   ├── index.html
│   └── package.json
├── traffic_data_simulated.csv  # Simulated traffic dataset
├── .env                        # API keys (FEATHERLESS_API_KEY)
└── README.md
```

---

## ⚙️ Installation

### Prerequisites
- Python 3.10+
- Node.js 18+
- npm or yarn

### 1. Clone the Repository
```bash
git clone https://github.com/Mayank-Chourasia77/Datathon_PS1.git
cd Datathon_PS1
```

### 2. Backend Setup
```bash
cd backend
pip install -r requirements.txt

# (Optional) Train the ML model
python train_model.py
```

### 3. Frontend Setup
```bash
cd frontend
npm install
```

### 4. Environment Variables
Create a `.env` file in the root directory:
```env
FEATHERLESS_API_KEY=your_api_key_here
```

---

## 🚀 Running the Application

### Start Backend (Terminal 1)
```bash
cd backend
python -m uvicorn main:app --reload --port 8000
```

### Start Frontend (Terminal 2)
```bash
cd frontend
npm run dev -- --host
```

### Access the Dashboard
Open your browser and navigate to:
- **Local**: `http://localhost:5173`
- **Network**: `http://<your-ip>:5173`

---

## 📡 API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/traffic-status` | GET | Returns all edges, metrics (PoA, costs), and bottleneck |
| `/nodes` | GET | Returns list of all node names for dropdowns |
| `/get-path` | POST | Calculates shortest path between two nodes |
| `/predict-congestion` | POST | ML prediction for a specific edge |
| `/ai-insight` | POST | LLM-generated traffic analysis |

### Example: Predict Congestion
```bash
curl -X POST http://localhost:8000/predict-congestion \
  -H "Content-Type: application/json" \
  -d '{"u": "Andheri East", "v": "Goregaon", "hour": 9, "rain_intensity": 0, "visibility": 1, "temperature": 30, "event_type": "None"}'
```

---

## 🧠 Key Algorithms

### 1. Price of Anarchy (PoA)
```
PoA = Total System Cost (Nash) / Total System Cost (Optimal)
```
- **Nash Equilibrium**: Selfish routing where each driver minimizes their own travel time
- **System Optimum**: Socially optimal routing that minimizes total network delay

### 2. BPR Travel Time Function
```
T(x) = T₀ × (1 + α × (x/C)^β)
```
- `T₀`: Free-flow travel time
- `x`: Current flow
- `C`: Capacity
- `α = 0.15`, `β = 4` (standard BPR parameters)

### 3. Dynamic Metrics Formulas
- **Travel Cost**: `₹250,000 + (Congestion% × 8,500)`
- **Throughput**: `25,000 + (Speed × 850)` vehicles/hr

---

## 🎨 UI Components

| Component | Description |
|-----------|-------------|
| **Map View** | Leaflet map with polylines colored by congestion |
| **Current/Optimized Toggle** | Switch between Nash and Optimal routing |
| **ML Forecast Card** | Shows predicted congestion, speed, confidence |
| **Total Travel Cost** | Dynamic cost in ₹ based on congestion |
| **Vehicle Throughput** | Dynamic flow based on average speed |
| **AI Console** | LLM-generated explanations and recommendations |

---

## 🔐 Security Notes

- API keys are stored in `.env` (not committed to Git)
- Backend includes safety-net fallbacks if AI API fails
- All user inputs are validated before processing

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- [Featherless AI](https://featherless.ai) for LLM API
- [Leaflet.js](https://leafletjs.com) for mapping
- [FastAPI](https://fastapi.tiangolo.com) for backend framework
- [Tailwind CSS](https://tailwindcss.com) for styling

---

<p align="center">
  Made with ❤️ for Mumbai's Traffic
</p>
