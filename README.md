# 💊 Pharmacy Inventory AI: ML-Based Stock Prediction System

This is a comprehensive Pharmacy Management System integrated with an AI-driven inventory forecasting module. The system automates the procurement process by predicting required stock for the upcoming period based on historical sales data.

---

## 🎯 Project Overview
Managing pharmacy inventory is complex due to the volatility of drug demand, expiration risks, and the criticality of product availability. This system automates inventory forecasting using a **Decision Tree Regressor (CARD methodology)**, enabling pharmacy managers to make data-driven procurement decisions rather than relying on manual estimations.

---

## 🚀 Technologies Used

### Backend & ML Engine (Python)
*   **Machine Learning**: `Scikit-learn` (Decision Tree Regressor, GridSearchCV).
*   **API Framework**: `Flask` (RESTful service for real-time predictions).
*   **Dashboarding**: `Streamlit` (Real-time monitoring, drift analysis, and model performance dashboard).
*   **Data Processing**: `Pandas`, `NumPy`.

### Frontend & Database (Laravel)
*   **Framework**: `Laravel` 12.x (PHP 8.2+).
*   **Database**: `SQLite`.
*   **Communication**: `Guzzle HTTP Client` for integration with the Python ML API.
*   **Export/Import**: `maatwebsite/excel` for report generation.

---

## ✨ Key Advantages
*   **Intelligent Forecasting**: Uses ML-based predictions instead of manual estimation.
*   **Automated Pipeline**: End-to-end automation from Laravel data export, data preparation, model training, and API deployment.
*   **Actionable Insights**: Integrated Inventory Risk Matrix and recommendation system.
*   **Robustness Monitoring**: Includes drift analysis to detect when model accuracy degrades due to changing market patterns.
*   **Interpretable AI**: Feature importance analysis and decision-tree configuration allow stakeholders to understand the "why" behind recommendations.

---

## 🛠️ Local Setup Guide

### 1. Prerequisites
- PHP 8.2+ & Composer
- Python 3.10+
- Node.js & NPM

### 2. Backend (Python/ML) Setup
```bash
cd backend
# Create virtual environment
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### 3. Frontend (Laravel) Setup
```bash
cd frontend
composer install
cp .env.example .env
# Configure DB_CONNECTION=sqlite in .env
php artisan key:generate
php artisan migrate
php artisan serve
```

---

## 🚦 How to Run the System

### Step 1: Export Data & Train Model
The backend automates the training process.
```bash
cd backend
# Automates: Laravel Export -> Transfer to Dataset -> Data Prep -> Training
python app.py --full --months=12
```

### Step 2: Start API Service
```bash
cd backend
python app.py --api
```

### Step 3: Run Monitoring Dashboard
```bash
cd backend
streamlit run streamlit_app.py
```

### Step 4: Access Application
1. Access the Laravel application at `http://localhost:8000`.
2. The Laravel `PrediksiController` will communicate with the Flask API at `http://localhost:5000` to fetch real-time predictions.

---

## 🐛 Common Troubleshooting

### 1. `BadMethodCallException` in Reports
If you encounter `Call to undefined method ...Builder::links()` when viewing reports:
- This usually means a missing `@forelse` loop in the Blade view or a pagination issue in the controller.
- Check the controller (e.g., `LaporanController`) to ensure you are using `paginate()` or `simplePaginate()`.
- Ensure your Blade view contains:
  ```blade
  @forelse ($items as $item)
      ... row code ...
  @empty
      <tr><td>Tidak ada data</td></tr>
  @endforelse
  {{ $items->links() }}
  ```

### 2. Model Accuracy/Performance
If predictions seem inaccurate:
- Check the "Drift Analysis" tab in the Streamlit Dashboard.
- If Test R² score is below 0.7, retrain the model with recent data using `python app.py --full`.

---
*Developed for Pharmacy Management System with ML-Based Stock Prediction.*
