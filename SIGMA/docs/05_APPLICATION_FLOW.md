# Application Flow

## User Flow

1. Open dashboard.
2. Upload `.IQ` or `.WAV`.
3. Review detected metadata.
4. Select analysis configuration.
5. Start analysis.
6. Observe processing status.
7. Review spectrum/spectrogram/waterfall.
8. Review detected signal segments.
9. Inspect extracted parameters.
10. Inspect optional classification.
11. Export JSON/CSV/report.

## Processing States

```text
UPLOADED
 → VALIDATED
 → DECODING
 → PREPROCESSING
 → ANALYZING
 → DETECTING
 → EXTRACTING
 → CLASSIFYING
 → COMPLETED
```

Possible terminal states:

- `COMPLETED`
- `FAILED`
- `CANCELLED`

See `../diagrams/application-flow.mmd`.
