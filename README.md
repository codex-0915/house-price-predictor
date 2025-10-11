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

---
