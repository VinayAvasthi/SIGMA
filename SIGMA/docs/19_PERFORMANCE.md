# Performance and Scalability

## Main Cost Drivers

- Input duration.
- Sample rate.
- Number of channels.
- FFT size.
- Spectrogram overlap.
- Number of candidate signals.
- ML inference cost.

## Optimization Techniques

- Chunk large files.
- Stream processing where practical.
- Use efficient numerical arrays.
- Cache reusable representations.
- Process independent segments in parallel.
- Move heavy work to background workers.
- Downsample only when it does not compromise the target measurement.

## Measurement

Do not claim a performance target without benchmarking. Record benchmark hardware, software version, input characteristics, and processing configuration.
