# IQ File Processing

## IQ Concept

Complex baseband samples are represented as:

`x[n] = I[n] + jQ[n]`

The parser must know or receive:

- Sample encoding.
- Endianness where relevant.
- Interleaving.
- Signed/unsigned representation.
- Scaling.
- Sample rate.
- Center frequency when available.

## Parser Output

The decoder produces a common `SignalRecord`.

## Validation

The implementation should detect malformed lengths, unsupported encodings, inconsistent metadata, and impossible sample configurations.

## Frequency Interpretation

The relationship between FFT bins and absolute RF frequency depends on the sample rate and center frequency metadata. If center frequency is unavailable, results should clearly distinguish relative/baseband frequency from absolute RF frequency.
