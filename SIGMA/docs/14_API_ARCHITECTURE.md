# API Architecture

## Suggested Endpoints

### Upload

`POST /api/v1/files`

Uploads a supported recording and returns a file identifier.

### Create Analysis

`POST /api/v1/analyses`

Creates an analysis job.

### Job Status

`GET /api/v1/analyses/{analysis_id}`

Returns status and metadata.

### Results

`GET /api/v1/analyses/{analysis_id}/results`

Returns structured results.

### Segments

`GET /api/v1/analyses/{analysis_id}/segments`

Returns detected signal segments.

### Report

`GET /api/v1/analyses/{analysis_id}/report`

Returns a generated report reference.

## API Principles

- Version endpoints.
- Validate all inputs.
- Return consistent error structures.
- Authenticate protected deployments.
- Record analysis provenance.
- Avoid exposing internal filesystem paths.
