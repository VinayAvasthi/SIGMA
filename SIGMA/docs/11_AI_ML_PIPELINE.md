# AI/ML Pipeline

## Role

ML is used for learned characterization, not as a substitute for all signal processing.

## Candidate Tasks

- Modulation classification.
- Signal-family classification.
- Anomaly detection.
- Learned feature extraction.
- Pattern recognition.

## Inference Flow

```text
Detected segment
 ↓
Feature extraction
 ↓
Feature normalization
 ↓
Model inference
 ↓
Prediction
 ↓
Confidence
 ↓
Result validation
```

## Model Card Requirements

Every model should document:

- Model name/version.
- Training dataset source.
- Dataset size.
- Classes.
- Train/validation/test split.
- Preprocessing.
- Metrics.
- Known limitations.
- Intended operating conditions.

## Safety of Interpretation

A model prediction is an estimate. The UI and API should distinguish prediction from directly measured DSP parameters.
