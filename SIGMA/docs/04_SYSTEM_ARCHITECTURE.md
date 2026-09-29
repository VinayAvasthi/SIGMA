# System Architecture

## Layers

### 1. Presentation
Web dashboard for upload, analysis progress, plots, tables, and reports.

### 2. API/Orchestration
Creates analysis jobs, validates requests, coordinates pipeline stages, and exposes results.

### 3. Signal Engine
Contains decoders, preprocessing, FFT/PSD, detection, segmentation, features, and parameter estimation.

### 4. AI/ML
Contains trained models and inference services for classification/pattern recognition.

### 5. Storage
Stores source metadata, job status, processing configuration, extracted results, and report references.

## Architecture Diagram

See `../diagrams/system-architecture.mmd`.

## Component Boundaries

Every component should have a clear input/output contract. For example, detection receives processed signal representations and returns candidate segments. Parameter extraction receives candidate segments plus relevant samples and returns a typed parameter object.

## Failure Isolation

A failed optional ML stage should not invalidate deterministic measurements unless the requested workflow explicitly requires the ML result.
