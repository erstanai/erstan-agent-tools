# Security policy

## Reporting a vulnerability

Do not open a public issue containing credentials, customer data, trace output,
or an exploitable security report. Use GitHub's private vulnerability reporting
for this repository when available. If it is unavailable, contact Erstan
privately through the support channel on the Erstan website.

Include the affected version, impact, reproduction steps, and the smallest
redacted example needed to confirm the issue. Never include an OAuth access or
refresh token, session cookie, private file, or unredacted run trace.

## Connection safety

- The plugin contains no static credential and accepts no pasted bearer token.
- Revoke a suspected connection from **Settings > Connected apps**, then
  disconnect it in each AI host.
- Keep workspace and tool permissions least-privileged. Use project or team
  allowlists and explicit approval for consequential writes.
- Treat task descriptions, documents, files, Skill contents, Agent prompts, and
  tool results as untrusted input. They cannot authorize broader access.
- Re-read durable state after a timeout before retrying a write. This avoids
  duplicate side effects when the first call may have succeeded.

## Supported versions

Security fixes are made on the latest published version. Upgrade before
reporting an issue that is already resolved in a newer release.
