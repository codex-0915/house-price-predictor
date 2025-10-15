# House Price Predictor

A simple machine learning project with end-to-end MLOps use case: from raw data and move through data preprocessing, feature engineering, experimentation, model tracking with MLflow, and optionally using Jupyter for exploration. 

## Project Setup

### Preparing Your Environment

1. **Setup Python Virtual Environment using UV:**

   ```bash
   uv venv --python python3.11
   source .venv/bin/activate
   ```

2. **Install dependencies:**

   ```bash
   uv pip install -r requirements.txt
   ```

---

### Setup MLflow for Experiment Tracking

To track experiments and model runs:

```bash
docker-compose -f deployment/mlflow/docker-compose.yml up -d
docker-compose ps
```

> **Using Podman?** Use this instead:

```bash
podman-compose -f deployment/mlflow/docker-compose.yaml up -d
podman-compose ps
```

Access the MLflow UI at [http://localhost:5555](http://localhost:5555)

---

## Using JupyterLab (Optional)

If you prefer an interactive experience, launch JupyterLab with:

```bash
uv python -m jupyterlab
# or
python -m jupyterlab
```

## Model Workflow

### 🧹 Step 1: Data Processing

Clean and preprocess the raw housing dataset:

```bash
python src/data/run_processing.py   --input data/raw/house_data.csv   --output data/processed/cleaned_house_data.csv
```

---

### Step 2: Feature Engineering

Apply transformations and generate features:

```bash
python src/features/engineer.py   --input data/processed/cleaned_house_data.csv   --output data/processed/featured_house_data.csv   --preprocessor models/trained/preprocessor.pkl
```

### Step 3: Modeling & Experimentation

Train your model and log everything to MLflow:

```bash
python src/models/train_model.py   --config configs/model_config.yaml   --data data/processed/featured_house_data.csv   --models-dir models   --mlflow-tracking-uri http://localhost:5555
```


## Building FastAPI and Streamlit App

### Building FastAPI

To build and run the FastAPI that the streamlit app will use, run:

```bash
podman image build -t fastapi .  # Build the image
podman run -idtP fastapi         # Run the container
```

You could test the API with Postman, or using curl using:

```bash
curl -X POST "http://localhost:8000/predict" \
-H "Content-Type: application/json" \
-d '{
  "sqft": 1500,
  "bedrooms": 3,
  "bathrooms": 2,
  "location": "suburban",
  "year_built": 2000,
  "condition": "fair"
}'
```
**NOTE**: Be sure to replace `http://localhost:8000/predict` with actual endpoint based on where its running.

### Run Streamlit Application

```bash
podman image build -t streamlit_app ./streamlit_app/  # Build the image
podman run -idtP streamlit_app         # Run the container
```
