# Realtime Fraud Flagger

**Realtime Fraud Flagger** is a simple, growing project that detects and flags potentially fraudulent transactions in real-time. The system uses an XGBoost model trained on historical online fraud transaction data and exposes a FastAPI endpoint so company systems can send transaction events and receive a fraud flag. This repo contains code, docs, and deployment recipes to run the model and a real-time monitoring dashboard.

---

---

## 🚀 Project Overview

🔗 **Live minimal frontend demo:** [https://fraud-detection-api-mvmi.onrender.com/](https://fraud-detection-api-mvmi.onrender.com/)

* **Goal:** Flag malicious transactions in real-time and alert system admins.
* **Current model:** XGBoost trained on transaction dataset — **~90% accuracy** (baseline).
* **Serving:** FastAPI (REST) that accepts transaction payloads and returns a fraud prediction and metadata.
* **Planned:** Real-time monitoring dashboard, LLM-based interpretability layer, more model variants and A/B testing, stream-processing for scale.

---

---

## 📂 Dataset & Features

**Target**

* `Y`: `fraud_label` (0 = legitimate, 1 = fraud)

**Input features (X) — 18 features**

1. `TransactionID` — unique id for the transaction
2. `UserID` — id of user who performed the transaction
3. `Amount` — transaction amount
4. `Type` — `ATM`, `Bank`, `POS`, `Online`, ...
5. `DeviceType` — `Laptop`, `Mobile`, `Tablet`, ...
6. `AccountBalance`
7. `Location`
8. `MerchantCategory` — `electronics`, `clothing`, `restaurants`, ...
9. `IP_address_flag` — if IP is malicious
10. `PreviousFraudActivity`
11. `DailyTransactionsCount`
12. `AvgTransactionAmount7d`
13. `FailedTransactions7d`
14. `CardType`
15. `TransactionDistanceKM`
16. `AuthenticationMethod` — `Password`, `OTP`, `Biometric`, `PIN`, ...
17. `RiskScore`
18. `IsWeekend`

---

---

## 🏗 Architecture (High-level)

1. **Data ingestion** — batch dataset for training; realtime events via REST.
2. **Model training** — XGBoost (baseline). Saved as a model artifact.
3. **Model serving** — FastAPI app exposes `POST /predict`.
4. **Monitoring & alerting** — track latency, throughput, fraud-rate, model drift.

---

---

## 🔌 API Usage (FastAPI)

### **POST /predict**

Send transaction metadata and receive fraud prediction.

#### Example Request

```json
{
  "TransactionID": "TXN123456",
  "UserID": "U7890",
  "Amount": 1200.50,
  "Type": "Online",
  "DeviceType": "Mobile",
  "AccountBalance": 45000,
  "Location": "Delhi",
  "MerchantCategory": "electronics",
  "IP_address_flag": 0,
  "PreviousFraudActivity": 0,
  "DailyTransactionsCount": 5,
  "AvgTransactionAmount7d": 900.2,
  "FailedTransactions7d": 1,
  "CardType": "Credit",
  "TransactionDistanceKM": 12.5,
  "AuthenticationMethod": "OTP",
  "RiskScore": 0.32,
  "IsWeekend": 0
}
```

#### Example Response

```json
{
  "fraud": true,
  "fraud_probability": 0.87,
  "message": "High-risk transaction flagged"
}
```

---

---

## 📈 Future Roadmap

### 🔥 **1. Real-time monitoring dashboard**

* Fraud trends
* Peak fraud hours
* User-level fraud probability
* Performance metrics (latency, throughput, error rate)

### 🤖 **2. LLM-based interpretability**

* LLM-generated natural language explanations:

  * "This transaction was flagged because the distance is unusually high and IP is suspicious."
* Possible integration: GPT, LLaMA, Mistral, DeepSeek

### 📊 **3. Model variants**

* LightGBM
* CatBoost
* Neural networks
* Ensembles
* A/B testing between models

### 📡 **4. Deployment improvements**

* Docker + Render/AWS ECS
* Add Redis or Kafka for streaming
* Async inference for high throughput

---

---

## 📁 Project Structure

```
Fraud-Detection-In-...
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── model.py
│   ├── schemas.py
│   ├── utils.py
│   └── templates/
│
├── model/
│   ├── fraud_xgb.pkl
│   ├── scaler.pkl
│   └── synthetic_fraud_dataset.csv
│
├── train/
│   └── train_model.py
│
├── Dockerfile
├── Readme.txt
└── requirements.txt
```

---

## 🧪 Local Development

### Install dependencies

```bash
pip install -r requirements.txt
```

### Run FastAPI

```bash
uvicorn app:app --reload
```

### Train model

```bash
python train.py
```

---
