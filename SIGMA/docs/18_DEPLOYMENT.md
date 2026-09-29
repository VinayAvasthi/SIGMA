# Deployment

## Development

Recommended components:

```text
Frontend
   ↓
Backend/API
   ↓
Signal Processing Worker
   ↓
Storage/Database
```

## Containerization

Use Docker to package reproducible runtime environments.

Suggested services:

- frontend
- api
- worker
- database
- object/file storage

## Scaling

Long-running signal analysis should run asynchronously through a job queue/worker model rather than blocking an HTTP request.

## Deployment Checklist

- Configure environment variables.
- Configure storage.
- Configure database.
- Configure authentication.
- Configure resource limits.
- Enable logging.
- Validate model availability.
- Run health checks.
