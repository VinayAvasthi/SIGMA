# Database Design

## Suggested Entities

### files
- id
- filename
- format
- size
- checksum
- created_at
- metadata

### analyses
- id
- file_id
- status
- configuration
- software_version
- created_at
- completed_at

### signal_segments
- id
- analysis_id
- start_time
- end_time
- frequency_low
- frequency_high
- detection_score

### parameters
- id
- segment_id
- parameter_name
- value
- unit
- method
- confidence

### classifications
- id
- segment_id
- model_name
- model_version
- label
- confidence

### reports
- id
- analysis_id
- format
- location
- created_at

Use foreign keys to preserve relationships and indexes for analysis/segment queries.
