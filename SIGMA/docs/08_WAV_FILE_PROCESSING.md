# WAV File Processing

## Supported Information

WAV metadata can include:

- Sample rate.
- Number of channels.
- Bit depth.
- Audio format.
- Sample count.

## Processing

1. Read WAV header.
2. Validate encoding.
3. Convert samples to analysis representation.
4. Preserve channel information.
5. Apply preprocessing.
6. Perform time/frequency analysis.
7. Detect candidate signal activity.
8. Extract supported parameters.

## Multi-channel Data

The pipeline should support per-channel analysis and optionally combined analysis. The selected mode must be recorded in the analysis configuration.
