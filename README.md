# Student Health Risk Prediction(multi-task-project using Functional API)

# DATASET LINK:- https://www.kaggle.com/competitions/playground-series-s6e7/data?utm_source=chatgpt.com

A Artificial Neural Network web application that predicts a student's health-condition category and estimates a numerical health-risk score from lifestyle and physical activity information. The project combines **classification and regression** in a single neural network and provides an interactive interface built with Streamlit.


## Features

- Interactive Streamlit interface for entering student details.
- Predicts one of three health-condition classes: `fit`, `at-risk`, or `unhealthy`.
- Estimates a health-risk score on a 0-100 scale.
- Displays classification confidence and probabilities for each class.
- Engineers additional risk features from BMI, stress, sleep, activity, heart rate, water intake, sleep quality, and lifestyle.
- Uses a shared neural-network architecture with two outputs:
  - **Classification head:** softmax output for the three classes.
  - **Regression head:** linear output for the numerical health-risk score.
- Uses a preprocessing pipeline for numerical, nominal, and ordinal features.

## Input features

| Category | Inputs |
|---|---|
| Physical | Sleep duration, heart rate, BMI |
| Activity | Calorie expenditure, step count, exercise duration, water intake |
| Lifestyle | Diet type, stress level, sleep quality, physical activity level, smoking/alcohol, gender |

The app derives additional features such as `bmi_risk`, `stress_risk`, `sleep_duration_risk`, `activity_risk`, `heart_rate_risk`, `water_risk`, `sleep_quality_risk`, and `lifestyle_risk`.

## Model workflow

1. Load and inspect the training data.
2. Engineer risk-related features.
3. Separate the input features from the health-condition classification target and health-risk regression target.
4. Split the data into training and test sets.
5. Encode the health-condition labels.
6. Preprocess numeric columns using median imputation and standardization.
7. Encode nominal columns using one-hot encoding and ordinal columns using ordinal encoding.
8. Train a neural network with a shared dense representation and two output heads.
9. Evaluate classification using a classification report and regression using MAE, MSE, RMSE, and R2².
10. Save the trained model and preprocessing object for use by the Streamlit app.

## Project structure

Suggested repository layout:

```text
student-health-risk/
* app.py
* student-health-risk(project).ipynb
* train.csv                 # training dataset; not necessarily included
* preprocessor.pkl          # fitted preprocessing pipeline
* model.keras               # trained two-output Keras model
* student.webp              # optional header image
* requirements.txt
* README.md
```

The filenames above are suggested. Update the paths in the app to match your actual filenames.

## Requirements

Install the core packages with:

```bash
pip install streamlit pandas numpy scikit-learn tensorflow keras
```

Use package versions compatible with the Python and TensorFlow/Keras versions in your environment.

## Configure the application

The uploaded `app(1).py` currently contains absolute Windows paths for the dataset, preprocessor, model, image, and an additional model load. These paths will not work on another computer or in a GitHub deployment unless changed.

Before running the app:

1. Put `train.csv`, the fitted preprocessor pickle, and the trained two-output `.keras` model in your project directory (or configure their locations).
2. Update `BASE_PATH`, `DATA_PATH`, `PREPROCESSOR_PATH`, and `MODEL_PATH` in the app to match your folder.
3. Ensure the model file is the final **two-output** model expected by the app. The app checks for both classification and regression outputs.
4. Remove or update the separate hard-coded `ANN1.keras` load near the top of `app(1).py`; it points to a local OneDrive path and is not used by the prediction flow shown.
5. Avoid committing private data or large model files to GitHub. Consider Git LFS or a trusted model-hosting service if the artifacts are too large.

## Run locally

From the project folder, activate your Python environment and run:

```bash
streamlit run app.py
```

Streamlit prints a local URL, usually `http://localhost:8501`. Open it in your browser, enter the student's details, and click **PREDICT HEALTH RISK**.

## Outputs

The app displays:

- **Health Condition:** `fit`, `at-risk`, or `unhealthy`
- **Confidence:** the model probability for the predicted class
- **Health Risk Score:** a score clipped to the range 0-100
- **Class Probabilities:** probabilities for each health-condition class
- **Risk interpretation:** low, moderate, or high according to the thresholds implemented in the app

Thresholds in the current app:
- Below 30: Low Health Risk
- 30 to below 60: Moderate Health Risk
- 60 and above: High Health Risk

These thresholds are application-defined and should not be treated as clinically validated cutoffs.

## Evaluation

The notebook evaluates classification with `classification_report` and regression with MAE, MSE, RMSE, and RÂ². Add your actual measured results here after running the notebook; do not report metrics that have not been measured.

## Dataset

The notebook loads `train.csv` from a local Windows directory. Add the dataset source and usage terms here before publishing the repository. Follow the dataset provider's terms and do not upload data unless redistribution is permitted.

## Limitations

- The health-risk score is generated from engineered features and the notebook's target formula; it is not a clinically validated measure.
- Model confidence is not the same as real-world accuracy.
- Results depend on the dataset, feature engineering, training process, and consistency between training and deployment preprocessing.
- The app and notebook currently use hard-coded local paths that must be updated for another environment.
- The app expects the saved preprocessing pipeline and model to match the exact input columns and feature order used during training.

## Future improvements

- Replace absolute paths with configurable relative paths or environment variables.
- Add unit tests for feature engineering and prediction.
- Document dataset provenance and measured evaluation metrics.
- Add input validation and clearer handling for invalid values.
- Validate the risk-score methodology before any real-world use.
