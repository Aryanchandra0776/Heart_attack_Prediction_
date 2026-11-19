# Heart Disease Prediction System

A small Flask web application that demonstrates a machine-learning workflow for classifying heart-disease data. Users enter health measurements in a web form, and the application passes those values to a pre-trained model and displays its prediction.

> **Important:** This is an educational demonstration, not a medical device. It cannot diagnose heart disease or replace advice from a qualified healthcare professional. Do not use its output to make health decisions.

## Features

- Web form for entering 13 health-related features.
- Pre-trained classifier stored in a pickle file.
- Flask routes for the form and prediction result.
- Training script and a Cleveland heart-disease CSV file are included.

## Technology

- Python and Flask
- NumPy, pandas, and scikit-learn
- HTML and CSS
- Gunicorn, configured in the `Procfile` for deployment

## Project files

| File or directory | Purpose |
| --- | --- |
| `app.py` | Flask application and prediction route |
| `templates/main.html` | Input form |
| `templates/result.html` | Prediction result page |
| `static/style.css` | Page styling |
| `prediction.py` | Script that prepares the CSV data, trains a classifier, and writes a model file |
| `heart_cleveland_upload.csv` | Dataset used by the training script; `condition` is the target column |
| `heart-disease-prediction-knn-model.pkl` | Pre-trained classifier loaded by the Flask application |
| `requirements.txt` | Python dependency pins |
| `Procfile` | Gunicorn start command for a compatible hosting service |

## Run locally

### 1. Get the code

```bash
git clone https://github.com/Aryanchandra0776/Heart_attack_Prediction_.git
cd Heart_attack_Prediction_
```

### 2. Create and activate a virtual environment

On Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

On macOS or Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

The dependency pins in `requirements.txt` are from an older environment. Installation may require updating them for your Python and pip versions. In particular, `sklearn==0.0` is a deprecated package name; the maintained package is `scikit-learn`. The serialized model may also depend on the scikit-learn version used to create it.

### 4. Start the application

Run the command from the project root so the relative model path resolves:

```bash
python app.py
```

Open [http://127.0.0.1:5000](http://127.0.0.1:5000) in your browser, fill in the form, and select **Predict**.

The included `Procfile` specifies the production start command:

```text
web: gunicorn app:app
```

## Input fields

The form uses the following feature names and category values:

| Field | Meaning | Form values |
| --- | --- | --- |
| `age` | Age in years | Number |
| `sex` | Sex category used by the dataset | `0` = female, `1` = male |
| `cp` | Chest-pain type | `0` = typical angina, `1` = atypical angina, `2` = non-anginal pain, `3` = asymptomatic |
| `trestbps` | Resting blood pressure | Number, in mmHg |
| `chol` | Serum cholesterol | Number, in mg/dL |
| `fbs` | Fasting blood sugar category | `0` = at or below 120 mg/dL, `1` = above 120 mg/dL |
| `restecg` | Resting ECG result | `0` = normal, `1` = ST-T wave abnormality, `2` = probable or definite left ventricular hypertrophy |
| `thalach` | Maximum heart rate achieved | Number, in beats per minute |
| `exang` | Exercise-induced angina | `0` = no, `1` = yes |
| `oldpeak` | ST depression | Number |
| `slope` | Slope of the peak exercise ST segment | `0` = upsloping, `1` = flat, `2` = downsloping |
| `ca` | Number of major vessels | Number; the form suggests `0`–`4` |
| `thal` | Thalassemia category | `0` = normal, `1` = fixed defect, `2` = reversible defect |

The form's placeholders show typical example ranges for some numeric inputs. These are dataset-oriented examples, not recommended clinical limits. Use the category definitions expected by the dataset.

## Model and dataset

`prediction.py` reads `heart_cleveland_upload.csv`, renames the `condition` column to `target`, splits the data into training and test sets, scales the features, and trains a `RandomForestClassifier` with 20 estimators. It writes the resulting classifier to `heart-disease-prediction-knn-model.pkl`.

The model filename contains “knn”, but the classifier defined in the training script is a Random Forest. Running the training script requires its Python dependencies and the CSV file to be available in the project root.

### Current implementation limitations

- The training script scales feature values, but saves only the classifier, not the fitted scaler. The Flask application does not apply the same scaling before prediction.
- Form values are submitted as strings, and the prediction route does not consistently convert categorical selections to numeric values before calling the model.
- As a result, predictions may fail or may not match the model's training-time preprocessing. The code should be updated to validate and convert inputs and to save and reuse the exact fitted preprocessing pipeline before relying on predictions.
- A pickle file should only be loaded from a trusted source. Loading untrusted pickle files can execute code.

## Troubleshooting

- **Model file not found:** Run `python app.py` from the project root and confirm `heart-disease-prediction-knn-model.pkl` is present.
- **Dependency installation fails:** The pins are old and include the deprecated `sklearn` package name. Use a compatible Python environment and update the dependency list as needed.
- **Model loading or prediction errors:** The saved model may require a compatible scikit-learn version, and the current application has the preprocessing limitations described above.

## License

No license file is currently included. Ask the project owner before redistributing or reusing this project.
