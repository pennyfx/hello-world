# CLAUDE.md

This file provides guidance for AI assistants working with this codebase.

## Project Overview

This is a **Datmo Hello World** project - a machine learning example that classifies Iris flower species using the classic Iris dataset. It demonstrates ML workflow management with Datmo for experiment tracking and reproducibility.

### What It Does

- Trains classification models (K-Nearest Neighbors or Gaussian Naive Bayes) on the Iris dataset
- Predicts flower species based on sepal and petal measurements
- Tracks experiments with Datmo snapshots
- Provides a simple Flask API for function execution

## Codebase Structure

```
hello-world/
├── classifier.py      # Main training script - trains and saves model
├── predict.py         # Prediction script - loads model and predicts
├── python_api.py      # Flask API server (simple add function demo)
├── config.json        # Algorithm configuration (KNeighbors or GaussianNB)
├── model.dat          # Serialized trained model (pickle format)
├── stats.json         # Training metrics (accuracy, training_time)
├── Iris.csv           # Dataset (150 samples, 3 species)
├── datmo.json         # Datmo model metadata
├── Dockerfile         # Environment definition
└── .gitignore         # Ignores Datmo internal directories
```

## Key Files

### classifier.py
The main training script that:
1. Loads `Iris.csv` dataset
2. Reads algorithm choice from `config.json`
3. Splits data into train/test sets
4. Trains either KNeighborsClassifier or GaussianNB
5. Outputs `model.dat` (serialized model) and `stats.json` (metrics)

### predict.py
Loads the trained model and predicts species for sample features.
Maps predictions to: `Iris-setosa`, `Iris-versicolor`, `Iris-virginica`

### config.json
```json
{"algorithm": "KNeighbors"}
```
Valid values: `"KNeighbors"` or `"GaussianNB"`

## Development Workflows

### Training a Model
```bash
python3 classifier.py
```
This outputs training stats and saves `model.dat` and `stats.json`.

### Running Predictions
```bash
python3 predict.py
```
Uses hardcoded test features in the script.

### Running the Flask API
```bash
python3 python_api.py
```
Starts server on `0.0.0.0:5000`. Example usage:
```bash
curl -X POST http://localhost:5000/add -H "Content-Type: application/json" -d '{"a": 1, "b": 2}'
```

### Using Datmo (if installed)
```bash
datmo clone hello-world          # Clone this model
datmo snapshot ls                # List existing snapshots
datmo task run "python3 classifier.py"  # Run training as tracked task
datmo snapshot create -m "message"       # Create snapshot
```

## Dependencies

Requires Python 3 with:
- pandas
- scikit-learn
- flask (for API)
- pickle (standard library)

Docker environment: `frolvlad/alpine-python-machinelearning:latest`

## Dataset

The Iris dataset contains 150 samples across 3 species (50 each):
- **Iris-setosa** (linearly separable)
- **Iris-versicolor**
- **Iris-virginica**

Features:
- SepalLengthCm, SepalWidthCm, PetalLengthCm, PetalWidthCm

## Conventions and Patterns

### Configuration
- Algorithm selection via `config.json`
- Model serialization uses Python pickle format
- Stats stored as JSON for easy parsing

### Code Style
- Uses scikit-learn for ML operations
- Pandas for data handling
- Random seed fixed at 3 for reproducibility

### File Outputs
- `model.dat` - Binary pickle file, overwritten on each training run
- `stats.json` - JSON with `training_time` and `accuracy` keys

## Important Notes for AI Assistants

1. **Don't modify Iris.csv** - This is the source dataset
2. **config.json controls algorithm** - Only `KNeighbors` or `GaussianNB` are valid
3. **model.dat is binary** - Don't try to read it as text
4. **python_api.py is a demo** - The `add` function is just an example, not related to ML
5. **Datmo directories are gitignored** - `.datmo/`, `datmo_tasks/`, `datmo_snapshots/`
6. **Legacy code** - Uses deprecated pandas methods (`as_matrix`) that may need updating for newer pandas versions
