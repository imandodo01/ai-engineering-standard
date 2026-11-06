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
