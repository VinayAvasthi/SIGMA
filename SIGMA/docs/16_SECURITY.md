# Security

Signal recordings may contain sensitive operational information. Security must therefore be designed into the platform.

## Controls

- Authentication and authorization for protected deployments.
- File-type validation.
- File-size limits.
- Safe temporary-file handling.
- No execution of uploaded files.
- Malware scanning where deployment policy requires it.
- Encryption in transit.
- Encryption at rest where required.
- Access logging.
- Result access control.
- Secrets stored outside source control.

## Privacy

Do not commit real sensitive recordings, operational metadata, credentials, or proprietary datasets to the public repository.

Use synthetic/sample files for demonstration.
