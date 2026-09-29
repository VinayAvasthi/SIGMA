# Signal Processing Pipeline

## Stage 1 — Validation

Check extension, magic/header where applicable, file size, readability, metadata consistency, and supported sample encoding.

## Stage 2 — Conditioning

Possible operations:

- DC removal.
- Filtering.
- Normalization.
- Resampling.
- Noise mitigation.
- Windowing.

## Stage 3 — Spectral Representation

Calculate FFT/PSD and, where useful, a short-time Fourier transform for spectrograms.

## Stage 4 — Detection

Estimate noise floor and identify candidate signal regions using configurable thresholds and/or adaptive methods.

## Stage 5 — Segmentation

Convert detections into time/frequency regions suitable for independent feature extraction.

## Reproducibility

Record:

- Algorithm version.
- Configuration.
- FFT parameters.
- Filter parameters.
- Detection thresholds.
- Input checksum.
- Software version.
