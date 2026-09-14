# 🧠 Student Wellness Predictor

> An AI-powered web application that predicts a student's **mental health score from 0–10** based on lifestyle, academic, digital habits, and perceived stress.

---

## 🚀 Live Demo

🌐 **Try the application:**
https://studentwellnesspredictor-m99z.onrender.com/

Users can enter their profile, academic and digital habits, lifestyle information, and stress level to generate a predicted mental health score.

---

## 📌 Overview

**Student Wellness Predictor** is a machine learning project designed to estimate a student's mental health score using information about their academic routine, social media usage, lifestyle, sleep, physical activity, and perceived stress.

The project uses a **Random Forest Regression** model trained on student lifestyle and social-media-related data. The trained model is integrated into a **FastAPI backend** and connected to a web-based frontend where users can enter their information and receive a predicted score.

The application is intended for **informational and educational purposes only** and is not a clinical or medical assessment.

---

## ✨ Features

* 🧠 Predicts a mental health score from **0–10**
* 👤 Collects basic student profile information
* 🎓 Considers academic level and study habits
* 📱 Considers social media platform and usage
* 📊 Considers daily phone unlock frequency
* 🏃 Considers physical activity
* 😴 Considers sleep duration
* 😟 Considers perceived stress level
* 🌍 Handles multiple countries using country grouping
* ⚡ Fast predictions through a FastAPI backend
* 🔗 REST API endpoint for predictions
* 🌐 Deployed and accessible online

---

## 🧠 Machine Learning

### Problem Type

**Regression**

The goal is to predict a continuous **Mental Health Score** rather than classify students into predefined categories.

### Final Model

**Random Forest Regressor**

The final model was selected after comparing different approaches.

The model is saved as:

```text
Mental_Health_Model.pkl
```

The trained model includes the preprocessing pipeline, allowing the API to directly pass raw user inputs to the model.

---

## 📊 Model Performance

The project experimented with Linear Regression and Random Forest Regression.

| Model                   |    Test R² |   Test MAE |
| ----------------------- | ---------: | ---------: |
| Linear Regression       |     0.7398 |     0.5362 |
| Random Forest Regressor | **0.8776** | **0.3472** |
| Tuned Random Forest     |     0.8650 |     0.3689 |

The **Random Forest Regressor** performed best on the test set and was therefore selected as the final model.

### Final Model Performance

* **Test R²:** `0.8776`
* **Test MAE:** `0.3472`

An R² of approximately **0.88** indicates that the model explains a substantial portion of the variation in the target score on the test set.

---

## 📚 Dataset

The model was trained using:

```text
Student Social Media And Mental Health Impact.csv
```

The dataset contains **5,000 student records** and **13 original columns**, including the target variable.

### Input Features

| Feature                   | Description                                |
| ------------------------- | ------------------------------------------ |
| `Age`                     | Student's age                              |
| `Gender`                  | Student's gender                           |
| `Country`                 | Student's country                          |
| `Academic_Level`          | Academic level                             |
| `Most_Used_Platform`      | Most frequently used social media platform |
| `Purpose_Of_Use`          | Primary reason for social media usage      |
| `Avg_Daily_Usage_Hours`   | Average daily social media usage           |
| `Daily_Unlocks`           | Number of phone unlocks per day            |
| `Study_Hours`             | Daily study hours                          |
| `Physical_Activity_Hours` | Daily physical activity                    |
| `Sleep_Hours_Per_Night`   | Average nightly sleep                      |
| `Stress_Level`            | Perceived stress level                     |
| `Mental_Health_Score`     | Target score from the dataset              |

---

## ⚙️ Data Preprocessing

Several preprocessing techniques were applied before training the model.

### Numerical Features

Standard scaling was applied to numerical variables.

### Study Hours

Because `Study_Hours` showed skewness, a logarithmic transformation using `log1p` was applied before scaling.

### Stress Level

Stress levels were treated as an ordinal feature:

```text
Low → Medium → High → Very High
```

### Categorical Features

Categorical variables were transformed using **One-Hot Encoding**.

### Country Grouping

Countries with a small number of records were grouped into an `Other` category to reduce the number of unique categorical values.

The model therefore uses:

```text
India
USA
Canada
Australia
UK
Germany
Mexico
Turkey
France
Other
```

This preprocessing was implemented using a Scikit-learn `ColumnTransformer` and integrated into the final model pipeline.

---

## 🔬 Model Development

The project followed a typical machine learning workflow:

```text
Dataset
   ↓
Exploratory Data Analysis
   ↓
Data Preprocessing
   ↓
Feature Engineering
   ↓
Train/Test Split
   ↓
Model Training
   ↓
Model Evaluation
   ↓
Model Comparison
   ↓
Random Forest Selection
   ↓
Model Serialization
   ↓
FastAPI Integration
   ↓
Web Application
```

The dataset was divided using a **70/30 train-test split** with `random_state=42`.

---

## 🌲 Random Forest

The initial Random Forest model achieved:

```text
Training R² : 0.9808
Testing R²  : 0.8776
Testing MAE : 0.3472
```

Hyperparameter tuning was also performed using `RandomizedSearchCV` with 5-fold cross-validation.

The tuned search explored:

* `n_estimators`
* `max_depth`
* `min_samples_split`
* `min_samples_leaf`

The best parameters found were:

```text
n_estimators = 200
max_depth = 15
min_samples_split = 5
min_samples_leaf = 2
```

However, the original Random Forest pipeline produced better test performance, so it was selected as the final model.

---

## 🌐 Application Architecture

The application consists of three main components:

```text
┌──────────────────────┐
│    Web Frontend      │
│ HTML / CSS / JS      │
└──────────┬───────────┘
           │
           │ POST /predict
           ▼
┌──────────────────────┐
│      FastAPI         │
│     REST Backend     │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│  ML Model Pipeline   │
│ Random Forest +      │
│ Preprocessing        │
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Mental Health Score  │
│       0 – 10         │
└──────────────────────┘
```

---

## 🔌 API

The FastAPI backend provides a prediction endpoint:

```http
POST /predict
```

Example request:

```json
{
  "age": 21,
  "gender": "Male",
  "country": "India",
  "academic_level": "Undergraduate",
  "most_used_platform": "Instagram",
  "purpose_of_use": "Education",
  "avg_daily_usage_hours": 4.5,
  "daily_unlocks": 120,
  "study_hours": 5.0,
  "physical_activity_hours": 1.5,
  "sleep_hours_per_night": 7.0,
  "stress_level": "Medium"
}
```

Example response:

```json
{
  "predicted_mental_health_score": 6.85
}
```

The backend validates incoming data using **Pydantic** before passing it to the model.

---

## 🛠️ Tech Stack

### Machine Learning

* Python
* Pandas
* NumPy
* Scikit-learn
* Joblib

### Backend

* FastAPI
* Pydantic
* Uvicorn

### Frontend

* HTML
* CSS
* JavaScript

### Development

* Jupyter Notebook
* Git
* GitHub

### Deployment

* Render

---

## 📂 Project Structure

```text
Mental-Health-Score/
│
├── .vscode/
│
├── Mental_Health_Model.pkl
│
├── Student Social Media And Mental Health Impact.csv
│
├── notebook.ipynb
│
├── main.py
│
├── index.html
│
├── style.css
│
├── script.js
│
├── requirements.txt
│
└── README.md
```

---

## ⚙️ Installation

### 1. Clone the repository

```bash
git clone https://github.com/pushpank-singh1/Mental-Health-Score.git
```

### 2. Navigate to the project directory

```bash
cd Mental-Health-Score
```

### 3. Create a virtual environment

```bash
python -m venv venv
```

### 4. Activate the environment

**Windows:**

```bash
venv\Scripts\activate
```

**macOS/Linux:**

```bash
source venv/bin/activate
```

### 5. Install dependencies

```bash
pip install -r requirements.txt
```

---

## ▶️ Run Locally

Start the FastAPI server:

```bash
uvicorn main:app --reload
```

The API will be available at:

```text
http://127.0.0.1:8000
```

FastAPI's interactive API documentation can also be accessed at:

```text
http://127.0.0.1:8000/docs
```

---

## 📖 Jupyter Notebook

The complete machine learning workflow is available in:

```text
notebook.ipynb
```

The notebook covers:

* Data loading
* Exploratory Data Analysis
* Data cleaning
* Feature engineering
* Country grouping
* Preprocessing
* Train-test splitting
* Linear Regression
* Random Forest Regression
* Model evaluation
* Hyperparameter tuning
* Model selection
* Model serialization

---

## ⚠️ Disclaimer

**Student Wellness Predictor is an educational and informational machine learning project.**

The predicted score should **not** be interpreted as a medical diagnosis, psychological assessment, or professional mental health evaluation.

If someone is experiencing mental health difficulties, they should seek appropriate support from a qualified healthcare professional or a trusted person.

---

## 🔮 Future Improvements

Possible improvements include:

* 📈 Add personalized lifestyle insights alongside the prediction
* 📊 Add visual analytics for sleep, screen time, study time, and stress
* 🧠 Experiment with additional regression algorithms
* 🔍 Add explainable AI techniques such as SHAP
* 📱 Build a responsive mobile-first interface
* 🔐 Add user authentication and secure data handling
* 📉 Monitor model performance after deployment
* 🧪 Perform more extensive cross-validation and model evaluation
* 🌍 Improve handling of countries and demographic variations

---

## 👨‍💻 Author

**Pushpank Singh**

AI/ML Engineer | Machine Learning & Data Science Enthusiast

* GitHub: [@pushpank-singh1](https://github.com/pushpank-singh1)
* LinkedIn: [Pushpank Singh](https://www.linkedin.com/in/pushpanksingh/)

---

## ⭐ Support

If you found this project interesting, consider giving the repository a ⭐ on GitHub.

**Repository:**
https://github.com/pushpank-singh1/Mental-Health-Score

**Live Demo:**
https://studentwellnesspredictor-m99z.onrender.com/
