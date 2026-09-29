# Signal Detection

## Purpose

Detection identifies where meaningful signal activity exists before detailed characterization.

## Candidate Methods

### Energy Detection
Compare local signal power against an estimated noise floor.

### Spectral Peak Detection
Identify frequency regions whose spectral power exceeds configured criteria.

### Time-Frequency Detection
Detect connected regions in a spectrogram.

### Hybrid Detection
Combine multiple evidence sources to reduce false detections.

## Output

```json
{
  "segment_id": "SEG-001",
  "start_time_s": 1.2,
  "end_time_s": 3.8,
  "frequency_low_hz": 98000000,
  "frequency_high_hz": 98200000,
  "detection_score": 0.91
}
```

Values are illustrative.

## Important

Detection score is not automatically a probability. The implementation must document how it is calculated.
