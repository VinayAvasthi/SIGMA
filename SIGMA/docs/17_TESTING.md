# Testing Strategy

## Unit Tests

Test each module independently:

- IQ parser.
- WAV parser.
- Metadata parser.
- Filters.
- FFT/PSD.
- Detection.
- Feature extraction.
- Parameter estimation.
- Result serialization.

## Integration Tests

Verify:

`file → ingestion → preprocessing → detection → extraction → result`

## Regression Tests

Keep known sample inputs and expected tolerances for deterministic calculations.

## ML Evaluation

Use held-out test data. Report:

- Accuracy.
- Precision/recall/F1 where appropriate.
- Confusion matrix.
- Per-class results.
- Dataset limitations.

## Performance Tests

Measure processing time versus:

- File size.
- Sample rate.
- Duration.
- FFT size.
- Number of detected segments.
