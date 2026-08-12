# Publishing Erstan Agent Tools

This repository packages public integration metadata and agent instructions.
It does not deploy the hosted MCP service or register listings in external
directories.

## Release checklist

1. Confirm `https://api.erstan.com/v1/mcp` is production-ready and exposes the
   OAuth discovery and authorization behavior required by each target host.
2. Confirm tool annotations, input and output schemas, error behavior, and
   approval boundaries match the public Skills.
3. Bump the same semantic version in `package.json`, both plugin manifests, and
   the Claude marketplace entry. The release checker enforces alignment.
4. Run `npm run verify` and all native validators documented in the README.
5. Review the exact Git archive. The release checker rejects files outside the
   allowlist and any symlink in the distribution.
6. Tag the reviewed commit and create the GitHub release.
7. Test a fresh Codex install and a fresh Claude Code install with a new user,
   OAuth grant, narrow permissions, approval, denial, revocation, and expiry.

## OpenAI / ChatGPT handoff

Submit the production MCP endpoint through the current OpenAI connector or app
publication flow. Supply reviewer access and test cases that exercise reads,
writes, user approvals, interactive waits, errors, and revocation.

There is deliberately no `.app.json` in this repository. A local OpenAI app
mapping is valid only after OpenAI assigns the actual technical identifier,
whose value begins with `plugin_asdk_app` or `asdk_app_`. If developer-mode
mapping is later required, copy that exact registered identifier, add it in a
reviewed release, and validate the installable bundle. Never publish a guessed,
example, or substitute identifier.

## Claude handoff

The root marketplace is `.claude-plugin/marketplace.json`, and its Erstan entry
must continue to use the relative source `./plugins/erstan`. The plugin's
`.mcp.json` points directly to the hosted URL. Users initiate OAuth with `/mcp`;
do not add `userConfig`, headers, bearer-key prompts, or token variables.

Before directory submission, run:

```text
claude plugin validate .
```

Then follow the current Anthropic directory requirements and record any manual
review steps in the release issue rather than adding reviewer secrets here.

## Codex handoff

The repository marketplace is `.agents/plugins/marketplace.json`. Validate
`plugins/erstan` with the Codex plugin validator, install from the released
repository in a clean profile, and confirm that the host presents OAuth rather
than requesting a static key.

## External readiness owned outside this repository

- Production OAuth discovery, consent, token expiry, refresh, and revocation.
- Stable tool schemas, safe annotations, idempotency behavior, and durable run
  reconciliation.
- **Settings > Connected apps** UI for profiles, permissions, allowlists,
  approvals, active
  sessions, audit visibility, and revoke controls.
- Directory registrations, reviewer credentials, privacy and terms review, and
  support operations.
