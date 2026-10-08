# -Real-Time-ML-Inference-REST-API-Capstone
# 🚀 Real-Time ML Inference REST API & Capstone

A production-oriented Machine Learning inference service built using **FastAPI, Scikit-learn, Docker, Pydantic, and Pytest**.

This project packages a trained **Customer Churn Prediction champion model** into a REST API capable of receiving real-time customer information and returning a prediction with probability scores.

---

## 📌 Project Overview

Machine Learning projects often stop after model training and evaluation.

This capstone focuses on the next stage:

> **Serving the trained model as a real-time API.**

The system provides an HTTP endpoint that applications can call to obtain customer churn predictions.

### System Flow

```text
             Client Application
                    │
                    ▼
             HTTP POST /predict
                    │
                    ▼
             FastAPI Application
                    │
                    ▼
             Pydantic Validation
                    │
                    ▼
          Feature Preprocessing
                    │
                    ▼
           Champion ML Pipeline
                    │
                    ▼
          Prediction + Probability
                    │
                    ▼
             JSON Response
```

---

# 🎯 Objectives

The objectives of this capstone were:

* Package the trained champion model.
* Build a production-style FastAPI service.
* Create a `/predict` endpoint.
* Validate incoming JSON data.
* Return prediction probabilities.
* Add health-check functionality.
* Write automated API tests.
* Containerize the service with Docker.
* Pin application dependencies.
* Document the complete ML inference architecture.

---

# 🛠️ Technology Stack

| Technology   | Purpose                     |
| ------------ | --------------------------- |
| Python       | Application development     |
| FastAPI      | REST API framework          |
| Pydantic     | Request/response validation |
| Scikit-learn | Machine Learning model      |
| Joblib       | Model serialization         |
| Uvicorn      | ASGI server                 |
| Pytest       | Automated testing           |
| Docker       | Containerization            |
| OpenAPI      | API documentation           |

---

# 📁 Project Structure

```text
real-time-ml-inference-api/
│
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── schemas.py
│   └── model_loader.py
│
├── model/
│   └── champion_model.joblib
│
├── tests/
│   ├── __init__.py
│   └── test_api.py
│
├── Dockerfile
├── .dockerignore
├── .gitignore
├── requirements.txt
├── requirements-dev.txt
└── README.md
```

---

# 🤖 Model

The API serves the trained **Customer Churn Prediction champion model**.

The model predicts whether a customer is likely to churn.

### Target

```text
0 → Not Churn
1 → Churn
```

The serialized model is stored as:

```text
model/champion_model.joblib
```

The recommended approach is to save the complete preprocessing + model pipeline so that inference uses the same transformations as training.

---

# 🌐 API Endpoints

## GET `/`

Returns basic API information.

### Response

```json
{
  "message": "Customer Churn Prediction API",
  "status": "running"
}
```

---

## GET `/health`

Checks whether the API and ML model are available.

### Response

```json
{
  "status": "healthy",
  "model_loaded": true
}
```

---

# 🔮 POST `/predict`

Performs real-time customer churn prediction.

## Request

```json
{
  "tenure": 12,
  "monthly_charges": 75.5,
  "total_charges": 906.0,
  "contract": "Month-to-month",
  "internet_service": "Fiber optic",
  "payment_method": "Electronic check"
}
```

## Response

```json
{
  "prediction": 1,
  "label": "Churn",
  "probability": 0.87,
  "class_probabilities": {
    "not_churn": 0.13,
    "churn": 0.87
  }
}
```

---

# 📊 Response Fields

| Field               | Type    | Description                    |
| ------------------- | ------- | ------------------------------ |
| prediction          | integer | Predicted class                |
| label               | string  | Human-readable prediction      |
| probability         | float   | Probability of predicted class |
| class_probabilities | object  | Probability for each class     |

---

# ❌ Validation

FastAPI/Pydantic validates incoming requests before they reach the model.

For example, missing required fields result in:

```text
HTTP 422 Unprocessable Entity
```

Invalid input types are also rejected.

---

# 📚 Interactive API Documentation

After starting the application, open:

```text
http://localhost:8000/docs
```

Swagger UI allows users to:

* View available endpoints
* Inspect request schemas
* Send test requests
* View responses
* Test API validation

Alternative documentation:

```text
http://localhost:8000/redoc
```

---

# 💻 Local Installation

## 1. Clone Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd real-time-ml-inference-api
```

## 2. Create Virtual Environment

Windows:

```bash
python -m venv venv
venv\Scripts\activate
```

Linux/macOS:

```bash
python3 -m venv venv
source venv/bin/activate
```

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

# ▶️ Run the API

```bash
uvicorn app.main:app --reload
```

The API will be available at:

```text
http://127.0.0.1:8000
```

Swagger documentation:

```text
http://127.0.0.1:8000/docs
```

---

# 🧪 Run Tests

Install development dependencies:

```bash
pip install -r requirements-dev.txt
```

Run:

```bash
pytest
```

Run with verbose output:

```bash
pytest -v
```

The test suite validates:

* Root endpoint
* Health endpoint
* Prediction endpoint
* Prediction response schema
* Probability range
* Missing fields
* Invalid data types
* HTTP response codes

---

# 🐳 Docker

## Build Image

```bash
docker build -t ml-inference-api .
```

## Run Container

```bash
docker run -p 8000:8000 ml-inference-api
```

Open:

```text
http://localhost:8000/docs
```

---

# 🔄 Docker Architecture

```text
                Docker Container
┌─────────────────────────────────────┐
│                                     │
│          FastAPI Application        │
│                 │                   │
│                 ▼                   │
│        Request Validation           │
│                 │                   │
│                 ▼                   │
│        Champion ML Pipeline         │
│                 │                   │
│                 ▼                   │
│       Prediction Probability       │
│                                     │
└─────────────────────────────────────┘
                  ▲
                  │
             HTTP Request
                  │
             Client/App
```

---

# 🔐 Production Considerations

This project follows several production-oriented practices:

* Input validation
* Explicit response schemas
* Health endpoint
* Serialized model artifact
* Pinned dependencies
* Automated tests
* Docker containerization
* API documentation
* Separation of application and model files

For a larger production deployment, additional components could include:

* Authentication
* HTTPS
* Rate limiting
* Logging
* Monitoring
* Model versioning
* CI/CD
* Cloud deployment
* Request tracing
* Drift monitoring

---

# 📈 End-to-End ML Lifecycle

```text
Data
 │
 ▼
Data Preprocessing
 │
 ▼
Model Training
 │
 ▼
Model Evaluation
 │
 ▼
Champion Model Selection
 │
 ▼
Model Serialization
 │
 ▼
FastAPI Inference Service
 │
 ▼
Automated Testing
 │
 ▼
Docker Container
 │
 ▼
Deployment
 │
 ▼
Real-Time Predictions
```

---

# 🎓 Internship Learning Outcomes

Through this project, I gained practical experience in:

1. Machine Learning model serving
2. REST API development
3. FastAPI
4. Pydantic validation
5. Prediction probability handling
6. Model serialization with Joblib
7. Automated API testing
8. Docker containerization
9. Dependency management
10. API documentation
11. Production-oriented ML architecture
12. End-to-end ML deployment workflow

---

# 🏆 Capstone Deliverables

| Requirement                | Status      |
| -------------------------- | ----------- |
| Champion ML model          | ✅ Completed |
| FastAPI service            | ✅ Completed |
| `/predict` endpoint        | ✅ Completed |
| Prediction probabilities   | ✅ Completed |
| Request validation         | ✅ Completed |
| Unit tests                 | ✅ Completed |
| Dockerfile                 | ✅ Completed |
| Pinned dependencies        | ✅ Completed |
| Swagger documentation      | ✅ Completed |
| Architecture documentation | ✅ Completed |
| GitHub repository          | ✅ Completed |

---

# 🚀 Future Improvements

Potential improvements include:

* Deploy to AWS/Azure/GCP
* Add CI/CD with GitHub Actions
* Add Prometheus/Grafana monitoring
* Add model versioning
* Add structured logging
* Add authentication
* Add batch prediction endpoint
* Add model performance monitoring
* Add data drift detection

---


### Areas of Interest

* Python
* Machine Learning
* Deep Learning
* Data Science
* Generative AI
* RAG
* Agentic AI
* FastAPI
* MLOps

---

## ⭐ Conclusion

This capstone demonstrates the complete transition from a trained Machine Learning model to a **Dockerized real-time inference service**.

**Model → API → Testing → Docker → Documentation → Deployment-Ready ML Service**

If you find this project useful, consider giving the repository a ⭐.
