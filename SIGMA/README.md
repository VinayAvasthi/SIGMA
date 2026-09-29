# SIGMA — Automated `.IQ` & `.WAV` Signal Analysis Platform

> **Smart India Hackathon (SIH) — Technical Project Repository Blueprint**

SIGMA is an automated signal-analysis platform designed to reduce the manual effort involved in analysing raw off-air signal recordings. The system accepts `.IQ` and `.WAV` recordings, validates and preprocesses them, detects candidate signals, extracts measurable signal parameters, generates time/frequency-domain visualizations, applies machine-learning models where appropriate, and produces structured analysis results and reports.

## 1. Problem Context

Raw signal recordings collected off the air can span from low-frequency bands to GHz-range RF environments. Conventional analysis often requires an operator to manually inspect waveforms, spectra, spectrograms, and other signal characteristics before entering useful parameters into downstream processing systems.

This creates several challenges:

- Manual analysis is time-consuming.
- Different analysts can produce inconsistent interpretations.
- Large recordings may contain multiple signals.
- Important parameters can be missed.
- Repeated analysis does not scale efficiently.
- Results may not be available in a consistent machine-readable format.
- Visual inspection alone does not provide a repeatable processing pipeline.

SIGMA addresses this workflow by creating a modular, auditable processing chain.

## 2. Core Objective

The platform is designed to transform:

`Raw .IQ/.WAV recording → validated signal data → processed signal → detected signal segments → extracted parameters → optional AI/ML classification → visualization → structured result → report/API output`

The system is intentionally divided into deterministic DSP stages and probabilistic AI/ML stages. Classical signal-processing methods are used for measurable parameters wherever possible; ML is used for tasks such as classification or pattern recognition when a trained model and suitable data are available.

## 3. High-Level Workflow

1. User uploads an `.IQ` or `.WAV` recording.
2. The ingestion service validates the file.
3. Metadata and recording properties are parsed.
4. The appropriate decoder converts the recording into a common internal representation.
5. Preprocessing removes or mitigates unwanted artefacts.
6. Frequency-domain and time-domain representations are calculated.
7. Candidate signals are detected and segmented.
8. Features are calculated for each candidate.
9. Signal parameters are estimated.
10. Optional AI/ML models classify or characterize the signal.
11. Confidence and provenance information are attached to results.
12. Results are visualized on the dashboard.
13. A machine-readable result is stored.
14. A human-readable report can be generated.
15. Results can be exposed through an API for downstream sensor/processing systems.

## 4. Architecture

```text
┌────────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER                      │
│ Upload │ Dashboard │ Spectrum │ Spectrogram │ Reports      │
└──────────────────────────────┬─────────────────────────────┘
                               │
┌──────────────────────────────▼─────────────────────────────┐
│                       API / ORCHESTRATOR                   │
│ Job Management │ File API │ Analysis API │ Report API     │
└──────────────────────────────┬─────────────────────────────┘
                               │
┌──────────────────────────────▼─────────────────────────────┐
│                  SIGNAL ANALYSIS ENGINE                    │
│ IQ/WAV Parser │ Preprocessing │ FFT/PSD │ Detection       │
│ Segmentation │ Feature Extraction │ Parameter Estimation  │
└──────────────┬───────────────────────────────┬──────────────┘
               │                               │
               ▼                               ▼
┌──────────────────────────────┐   ┌─────────────────────────┐
│          AI / ML             │   │      DATA / STORAGE     │
│ Classification              │   │ Raw/processed metadata  │
│ Modulation recognition      │   │ Features/results        │
│ Pattern/anomaly detection   │   │ Job/report information  │
└──────────────────────────────┘   └─────────────────────────┘
```

## 5. Common Internal Signal Representation

After decoding, both supported input families should be mapped into a common internal object so downstream modules do not need to know the original file format.

Conceptually:

```text
SignalRecord
├── samples
├── sample_rate_hz
├── center_frequency_hz (optional)
├── channel_count
├── data_type
├── start_time (optional)
├── duration_seconds
├── source_format
├── source_file
└── metadata
```

For complex IQ data, samples are represented as:

`x[n] = I[n] + jQ[n]`

For WAV data, the channel/sample structure is retained and the analysis mode determines whether the signal is treated as real-valued audio/baseband data.

## 6. Processing Pipeline

### 6.1 Ingestion

Responsibilities:

- Accept supported file types.
- Calculate file identity/checksum.
- Validate file readability.
- Parse metadata.
- Determine sample format.
- Determine sample rate where encoded.
- Record center frequency when available.
- Reject unsupported or malformed input with an actionable error.

### 6.2 Preprocessing

Typical stages:

- DC-offset removal.
- Amplitude normalization when appropriate.
- Optional resampling.
- Anti-alias filtering when resampling.
- Band-pass/low-pass/high-pass filtering according to analysis configuration.
- Noise mitigation.
- Windowing before spectral analysis.
- Data-quality checks.

Preprocessing parameters must be recorded so that an analysis can be reproduced.

### 6.3 Spectral Analysis

The platform can use FFT-based analysis to estimate frequency-domain characteristics.

For a windowed signal:

`X[k] = Σ x[n]w[n]e^(-j2πkn/N)`

The implementation should retain:

- FFT size.
- Window type.
- Overlap.
- Frequency-bin spacing.
- PSD calculation method.
- Scaling/reference information.

### 6.4 Signal Detection

Candidate detection can combine methods such as:

- Energy thresholding.
- Noise-floor estimation.
- Spectral peak detection.
- Connected regions in time-frequency maps.
- Configurable minimum duration.
- Configurable minimum bandwidth.
- Optional adaptive thresholds.

The output is a set of candidate signal segments:

```text
CandidateSignal
├── segment_id
├── start_time
├── end_time
├── frequency_low
├── frequency_high
├── peak_frequency
└── detection_score
```

Detection is separate from classification: first determine where signal activity exists, then characterize what it may be.

### 6.5 Parameter Extraction

Depending on the signal and available metadata, the engine may estimate:

- Center/peak frequency.
- Lower and upper occupied frequency bounds.
- Bandwidth.
- Duration.
- Signal power.
- Noise power.
- SNR.
- Sample rate.
- Spectral peak information.
- Spectral centroid/spread.
- Time-domain statistics.
- Modulation-related features.
- Symbol-rate-related estimates where technically supported.
- Confidence/quality indicators.

Not every parameter is valid for every recording. The result schema therefore supports `null`, unavailable, or not-applicable values.

### 6.6 AI/ML Layer

AI/ML is an optional analytical layer rather than a replacement for DSP.

Possible functions:

- Modulation classification.
- Signal-family classification.
- Pattern recognition.
- Anomaly detection.
- Learned signal embeddings.
- Confidence estimation.

Each model result should include:

```text
model_name
model_version
input_features
prediction
confidence
timestamp
```

The repository should never imply that a model is production-ready without evaluation data. Model accuracy, validation methodology, dataset provenance, class distribution, and limitations should be documented when models are added.

## 7. Visualization

The dashboard can expose:

- Raw waveform.
- I/Q waveform.
- FFT spectrum.
- Power spectral density.
- Spectrogram.
- Waterfall.
- Constellation plot where applicable.
- Detected-signal bounding regions.
- Parameter table.
- Classification results.
- Confidence indicators.
- Processing/job status.

Visualizations are analytical aids; they should not silently replace numerical results.

## 8. Example Result

Illustrative only:

```json
{
  "analysis_id": "ANL-000001",
  "source": {
    "filename": "sample.iq",
    "format": "IQ"
  },
  "signal": {
    "start_time_s": 1.25,
    "end_time_s": 4.05,
    "peak_frequency_hz": 98420000,
    "bandwidth_hz": 180000,
    "snr_db": 21.0
  },
  "classification": {
    "label": "example-class",
    "confidence": 0.94,
    "model_version": "demo-1.0"
  }
}
```

These numbers are placeholders for documentation and are not claimed measurements.

## 9. Suggested Repository Structure

```text
sigma/
├── README.md
├── LICENSE
├── requirements.txt
├── Dockerfile
├── docker-compose.yml
├── .env.example
├── docs/
│   ├── 01_PROJECT_OVERVIEW.md
│   ├── 02_PROBLEM_STATEMENT.md
│   ├── 03_SOLUTION_APPROACH.md
│   ├── 04_SYSTEM_ARCHITECTURE.md
│   ├── 05_APPLICATION_FLOW.md
│   ├── 06_SIGNAL_PROCESSING_PIPELINE.md
│   ├── 07_IQ_FILE_PROCESSING.md
│   ├── 08_WAV_FILE_PROCESSING.md
│   ├── 09_SIGNAL_DETECTION.md
│   ├── 10_PARAMETER_EXTRACTION.md
│   ├── 11_AI_ML_PIPELINE.md
│   ├── 12_VISUALIZATION.md
│   ├── 13_REPORT_GENERATION.md
│   ├── 14_API_ARCHITECTURE.md
│   ├── 15_DATABASE_DESIGN.md
│   ├── 16_SECURITY.md
│   ├── 17_TESTING.md
│   ├── 18_DEPLOYMENT.md
│   ├── 19_PERFORMANCE.md
│   ├── 20_FUTURE_SCOPE.md
│   └── 21_SIH_MAPPING.md
├── diagrams/
├── schemas/
├── src/
│   ├── ingestion/
│   ├── preprocessing/
│   ├── signal_detection/
│   ├── feature_extraction/
│   ├── parameter_extraction/
│   ├── classification/
│   ├── visualization/
│   ├── reporting/
│   ├── api/
│   └── utils/
├── frontend/
├── backend/
├── tests/
├── notebooks/
├── models/
├── configs/
└── scripts/
```

## 10. Design Principles

- Modular DSP pipeline.
- Reproducible analysis.
- Explicit metadata and provenance.
- Deterministic measurements separated from ML predictions.
- Machine-readable outputs.
- Human-readable reports.
- Configurable processing parameters.
- Graceful handling of unsupported parameters.
- Testable components.
- Container-friendly deployment.
- No fabricated measurements.

## 11. Important Limitations

The exact parameters that can be extracted depend on:

- File format and metadata.
- Sample rate and bandwidth.
- Center-frequency information.
- Recording duration.
- Signal-to-noise ratio.
- Signal type.
- Frequency resolution.
- Whether the recording contains multiple overlapping signals.
- Availability and validation of ML models.

Therefore, the system should report a parameter only when the underlying data and algorithm support a meaningful estimate.

## 12. SIH Alignment

The solution directly addresses the core workflow of automated `.IQ` and `.WAV` analysis:

| Requirement | SIGMA module |
|---|---|
| `.IQ` processing | IQ ingestion/decoder |
| `.WAV` processing | WAV ingestion/decoder |
| Automated analysis | Analysis orchestrator |
| Signal detection | Detection engine |
| Parameter extraction | Parameter engine |
| Signal characterization | DSP + feature extraction |
| Intelligent classification | AI/ML layer |
| Visual analysis | Visualization dashboard |
| Structured output | Result schema/API |
| Reduced manual effort | End-to-end automation |

## 13. Project Status

This repository is intended to document the proposed architecture and implementation plan. Modules should be marked as `prototype`, `experimental`, or `production-ready` based on actual implementation and validation rather than assumed capability.

## 14. License

Add the project team's selected license before public release.
