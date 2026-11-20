# Security and Operational Safety

- Never commit credentials, tokens, private keys, connection strings, personal data, or production secrets.
- Do not print secrets into terminal output, reports, screenshots, or logs. Redact when necessary.
- Validate untrusted input at system boundaries; use parameterized queries and safe output encoding.
- Apply least privilege and verify authorization server-side, not only in the UI.
- Consider authentication, authorization, CSRF, injection, file upload, path traversal, SSRF, and sensitive-data exposure where relevant.
- Do not weaken security controls to make a test or workflow pass.
- Treat migrations, bulk updates, deletion, production configuration, deployments, and permission changes as high impact.
- Ask before destructive or irreversible actions, production operations, secret rotation, or access-control changes.
- Do not assume a local environment is safe for production credentials or real customer data.

## Multi-factor authentication

When implementing or changing MFA:

- Distinguish the factor and protocol accurately. Email-delivered OTP, SMS OTP, TOTP authenticator codes, push approval, and phishing-resistant factors such as WebAuthn are not interchangeable. Document the actual mechanism and its assurance limitations.
- Generate TOTP secrets with a cryptographically secure random source and sufficient entropy. Encrypt secrets at rest using the application's managed encryption facility; never log them or expose them after the enrollment flow.
- Require an authenticated, recently re-authenticated user to begin, replace, disable, or regenerate MFA credentials. Verify a newly enrolled factor before marking it enabled.
- Protect enrollment confirmation, login challenges, and recovery-code operations with server-side rate limits and abuse controls.
- Prevent TOTP replay. Persist the accepted time-step/counter atomically with verification, using a transaction and row lock or an equivalent concurrency-safe mechanism. A successful code must not be accepted again for the same account.
- Generate high-entropy recovery codes, store only secure hashes, reveal them once, and consume each code atomically on use. Regeneration must invalidate the previous set.
- Keep MFA challenge state bound to the intended account and purpose. Clear or expire pending state after success, expiry, cancellation, or account changes.
- Preserve existing MFA protection during migrations. Do not silently disable a user's current factor or switch methods until the replacement factor has been enrolled and confirmed. Provide an explicit, recoverable migration path.
- Do not send authenticator secrets or provisioning URIs to third-party QR-generation services. Generate QR codes locally or provide a secure manual-entry option.
- Treat MFA disablement, factor replacement, recovery-code regeneration, and secret rotation as security-sensitive actions; require re-authentication and record appropriate audit events where the application maintains an audit trail.
