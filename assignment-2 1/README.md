# Power Load Type Prediction using Machine Learning

## Overview

This project develops a machine learning model to predict the **Load Type** of a power system using historical electricity consumption data. The workflow includes data preprocessing, feature engineering, model training, evaluation, visualization, and model persistence.

The project compares multiple machine learning algorithms and selects the best-performing model based on prediction accuracy.

---

# Objective

The objective of this project is to build a classification model that predicts the **Load Type** from historical power consumption data.

The project includes:

* Data preprocessing
* Feature engineering
* Missing value handling
* Model training
* Model evaluation
* Feature importance analysis
* Model comparison
* Model saving and loading

---

# Dataset

The dataset is loaded from Google Drive.

```text
load_data.csv
```

The dataset contains historical power consumption information along with the target column:

* **Load_Type** (Target Variable)

---

# Project Workflow

## 1. Data Loading

* Load the dataset using Pandas.
* Display dataset information.
* Check missing values.
* Generate descriptive statistics.

---

## 2. Data Preprocessing

The following preprocessing steps are performed:

* Convert `Date_Time` into datetime format.
* Extract:

  * Year
  * Month
  * Day
  * Hour
* Remove unnecessary columns.
* Handle missing values using median imputation.
* Standardize numerical features.
* Encode target labels using `LabelEncoder`.

---

## 3. Train-Test Split

The dataset is divided using a time-based approach.

* Training Data:

  * Months **1–11**
* Testing Data:

  * Month **12**

This preserves the chronological order of the data.

---

## 4. Machine Learning Models

The following models are implemented and compared:

* Random Forest Classifier
* Gradient Boosting Classifier
* XGBoost Classifier

Each model is trained using the same preprocessing pipeline.

---

## 5. Model Evaluation

The models are evaluated using:

* Accuracy Score
* Classification Report
* Confusion Matrix
* Cross Validation (TimeSeriesSplit)

---

## 6. Feature Importance

Feature importance is computed using the trained Random Forest model to identify the most influential variables affecting load prediction.

---

## 7. Model Comparison

A comparison table and visualization are generated to compare:

* Random Forest
* Gradient Boosting
* XGBoost

based on prediction accuracy.

---

## 8. Model Persistence

The final trained model is saved using Joblib.

```text
Power_Load_Model.pkl
```

The saved model can later be loaded for prediction without retraining.

---

# Technologies Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn
* XGBoost
* Joblib
* Google Colab

---

# Required Libraries

Install the required packages before running the notebook.

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost joblib
```

---

# Running the Project

1. Open the notebook in Google Colab.
2. Mount Google Drive.
3. Place `load_data.csv` in the specified Google Drive location.
4. Run all notebook cells sequentially.
5. Train the models.
6. Evaluate model performance.
7. Save the trained model.
8. Use the saved model for prediction.

---

# Output

The project generates:

* Dataset summary
* Missing value analysis
* Feature engineered dataset
* Trained machine learning models
* Classification report
* Confusion matrix
* Feature importance chart
* Model comparison chart
* Saved model (`Power_Load_Model.pkl`)

---

# Results

The notebook compares three classification algorithms and selects the model with the highest accuracy for predicting the power system load type.

The trained model can be reused for predicting future load categories on unseen data.

---

# Future Improvements

* Hyperparameter optimization using GridSearchCV or RandomizedSearchCV.
* Support additional machine learning algorithms.
* Deploy the model using Flask or FastAPI.
* Integrate real-time power consumption data for live prediction.
* Perform automated feature selection to improve model performance.

---

# Conclusion

This project demonstrates a complete machine learning pipeline for power load type prediction. It covers data preprocessing, feature engineering, model training, evaluation, comparison, and deployment readiness. The resulting model provides an efficient solution for classifying power load types and can be extended for real-world energy management applications.
