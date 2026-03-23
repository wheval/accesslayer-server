# Security Policy

## Reporting a vulnerability

Please do not open public GitHub issues for security vulnerabilities.

Instead:

1. Gather the affected area, impact, and reproduction details.
2. Contact the maintainers privately or use GitHub private vulnerability reporting if it is enabled for this repository.
3. Give the maintainers time to validate and coordinate a fix before public disclosure.

## Scope

Security reports are especially relevant for:

- auth and session flows
- API input validation
- secret handling and environment variables
- file upload or email utilities
- access control for gated resources

## Secret handling

- Never commit live `.env` files or service-account credentials.
- Use `.env.example` and sanitized examples instead.
- Rotate any secrets that were ever exposed outside trusted environments.
