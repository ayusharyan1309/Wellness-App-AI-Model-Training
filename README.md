# Sleep Suggestions Model — ML-Powered Sleep Quality Predictor

A **Flask REST API** that predicts sleep quality using a **Random Forest** model and generates **personalized improvement suggestions** via Google Gemini AI with **SHAP explainability**. Part of the [Somnofy Wellness App](https://github.com/ayusharyan1309/wellness-app-backend) ecosystem.

## Features

- **Sleep Quality Prediction** — Random Forest model classifies sleep into 4 quality levels
- **SHAP Explainability** — Identifies top features contributing to each prediction
- **AI-Powered Suggestions** — Google Gemini generates personalized improvement tips
- **Feature Analysis** — Uses hours slept, conversation, walking, and noise duration
- **REST API** — Simple Flask endpoint for integration with mobile/web frontends

## How It Works

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  User Input  │────▶│  Random      │────▶│  SHAP        │
│  (4 features)│     │  Forest      │     │  Explainer   │
└──────────────┘     │  Model       │     │              │
                     │  (predict)   │     │  (top 2      │
                     └──────┬───────┘     │  features)   │
                            │             └──────┬───────┘
                            │                    │
                     ┌──────▼────────────────────▼───────┐
                     │         Google Gemini AI            │
                     │  (personalized suggestions based   │
                     │   on prediction + SHAP features)   │
                     └──────────────────┬────────────────┘
                                        │
                     ┌──────────────────▼────────────────┐
                     │  JSON Response:                     │
                     │  - prediction score                 │
                     │  - qualitative quality              │
                     │  - AI-generated feedback            │
                     └────────────────────────────────────┘
```

## Quick Start

```bash
git clone https://github.com/ayusharyan1309/sleep-suggestions-model.git
cd sleep-suggestions-model
pip install -r requirements.txt

# Set Google AI API key
export GOOGLE_GENERATIVE_AI_API_KEY="your-api-key"

python app.py
```

Server runs on `http://localhost:8082`

## API Endpoints

### Health Check
```
GET /
```
**Response:** `Sleep Quality Prediction Server is Running!`

### Predict Sleep Quality
```
POST /predict
Content-Type: application/json
```

**Request:**
```json
{
  "hours_slept": 6.5,
  "sleep_conversation_interaction": 2.0,
  "walking_duration": 45.0,
  "noise_duration": 3.5
}
```

**Response:**
```json
{
  "predicted_sleep_quality_score": 2.3,
  "qualitative_sleep_quality": "Fairly Good Sleep Quality",
  "feedback": "Based on your sleep patterns, here are personalized suggestions..."
}
```

## Input Features

| Feature | Description | Example |
|---------|-------------|---------|
| `hours_slept` | Total hours of sleep | 6.5 |
| `sleep_conversation_interaction` | Audio conversation during sleep | 2.0 |
| `walking_duration` | Physical activity (walking) duration | 45.0 |
| `noise_duration` | Ambient noise exposure | 3.5 |

## Sleep Quality Classes

| Score Range | Quality Level |
|-------------|--------------|
| 1.0 - 1.5 | Very Good Sleep Quality |
| 1.5 - 2.5 | Fairly Good Sleep Quality |
| 2.5 - 3.5 | Fairly Bad Sleep Quality |
| 3.5 - 4.0 | Very Bad Sleep Quality |

## Project Structure

```
├── app.py                         # Flask API + ML pipeline
├── final_random_forest_model.pkl  # Trained Random Forest model
├── X_train.pkl                    # Training data (for SHAP explainer)
├── requirements.txt               # Python dependencies
├── ngrok.yml                      # ngrok config for public URL
└── .gitignore                     # Ignored files
```

## Tech Stack

| Component | Technology |
|-----------|------------|
| **API** | Flask 3.1.0 |
| **ML Model** | Random Forest (scikit-learn) |
| **Explainability** | SHAP 0.46.0 |
| **LLM** | Google Gemini (google-generativeai) |
| **Data** | NumPy, Pandas |
| **Deployment** | ngrok |

## Integration

This model is called by the [wellness-app-backend](https://github.com/ayusharyan1309/wellness-app-backend) Spring Boot API, which forwards sleep data from the Flutter frontend to this Flask prediction service.

```
Flutter App → Spring Boot API → This Flask API → Gemini AI → Response
```
