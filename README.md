# Health Insurance Premium Predictor

A machine learning web application that predicts health insurance premium costs based on personal, medical, and lifestyle factors.

## Overview

This project uses segmented regression models trained on healthcare premium data to provide cost predictions. The model is split into two segments:
- **Young model** — for individuals aged 25 and below
- **Rest model** — for individuals above 25

This segmentation improves prediction accuracy by capturing the different risk profiles across age groups.

## Features

The app takes the following inputs to predict insurance cost:

| Feature | Type | Options / Range |
|---|---|---|
| Age | Numeric | 18 – 100 |
| Number of Dependants | Numeric | 0 – 20 |
| Income (in Lakhs) | Numeric | 0 – 200 |
| Genetical Risk | Numeric | 0 – 5 |
| Gender | Categorical | Male, Female |
| Marital Status | Categorical | Married, Unmarried |
| BMI Category | Categorical | Normal, Obesity, Overweight, Underweight |
| Smoking Status | Categorical | No Smoking, Occasional, Regular |
| Employment Status | Categorical | Salaried, Self-Employed, Freelancer |
| Region | Categorical | Northwest, Southeast, Northeast, Southwest |
| Medical History | Categorical | No Disease, Diabetes, High Blood Pressure, Heart Disease, Thyroid, and combinations |
| Insurance Plan | Categorical | Bronze, Silver, Gold |

## Project Structure

```
Healthcare-Premium-Project/
├── app/
│   ├── main.py                  # Streamlit UI
│   ├── prediction_helper.py     # Preprocessing and prediction logic
│   └── artifacts/
│       ├── model_young.joblib   # Model for age <= 25
│       ├── model_rest.joblib    # Model for age > 25
│       ├── scaler_young.joblib  # Scaler for young segment
│       └── scaler_rest.joblib   # Scaler for rest segment
├── artifacts/                   # Root-level model artifacts
├── script.ipynb                 # Initial EDA and model training
├── Data Segmentation.ipynb      # Age-based data segmentation
├── script-young.ipynb           # Model training for young segment
├── script-rest.ipynb            # Model training for rest segment
├── script_young_with_gr.ipynb   # Young model with genetical risk feature
├── script_rest_with_gr.ipynb    # Rest model with genetical risk feature
├── premiums.xlsx                # Original dataset
├── premiums_young.xlsx          # Young segment dataset
├── premiums_rest.xlsx           # Rest segment dataset
├── premiums_young_with_gr.xlsx  # Young segment dataset with genetical risk
├── requirements.txt
└── runtime.txt
```

## How It Works

1. **Input collection** — User fills in personal and medical details via the Streamlit UI.
2. **Preprocessing** — Categorical features are one-hot encoded; medical history is converted to a normalized risk score (0–1) based on disease severity weights.
3. **Segmentation** — Age determines which model and scaler to use (young vs. rest).
4. **Scaling** — Numerical features are scaled using the appropriate pre-fitted scaler.
5. **Prediction** — The selected XGBoost model outputs the predicted annual premium in INR.

### Medical History Risk Scoring

| Disease | Risk Score |
|---|---|
| Heart Disease | 8 |
| Diabetes | 6 |
| High Blood Pressure | 6 |
| Thyroid | 5 |
| No Disease | 0 |

Combined conditions are summed and normalized against a maximum score of 14.

## Tech Stack

- **Python 3.12**
- **Streamlit** — web interface
- **scikit-learn** — preprocessing and scaling
- **XGBoost** — regression models
- **pandas / numpy** — data manipulation
- **joblib** — model serialization

## Setup and Installation

```bash
# Clone the repository
git clone <repo-url>
cd Healthcare-Premium-Project

# Install dependencies
pip install -r requirements.txt

# Run the app
cd app
streamlit run main.py
```

## Dataset

The training data (`premiums.xlsx`) contains healthcare insurance records with premium costs across various demographic and medical attributes. The data was segmented by age to train specialized models for better accuracy.
