# Erstan Agent Tools

Official Erstan integration for AI coding agents. This repository packages one
broad plugin that connects to Erstan's hosted MCP server and supplies focused
instructions for building and reviewing Agents, operating runs, managing
Skills, and working with authorized workspace content.

The plugin contains no Erstan API key. Each user signs in to
`https://api.erstan.com/v1/mcp` with OAuth through their AI host.

## What is included

- `erstan-agent-builder` — create, revise, validate, test, and publish Agent
  graphs.
- `erstan-agent-review` — review Agent definitions and diagnose runs from
  durable evidence.
- `erstan-run-operator` — launch published Agents and safely handle waits,
  approvals, and traces.
- `erstan-skill-manager` — manage complete, versioned Erstan Skill packages.
- `erstan-work-manager` — work with authorized tasks, projects, documents,
  folders, and files.

## Permission model

Erstan is the source of truth for what the connection may access. Users manage
connected apps, workspace scope, allowed tool families, project or team
allowlists, and Erstan-side approval policy in **Settings > Connected apps**.

Initial authorization defaults to the recommended **Build** profile. It includes
**View agents**, **Build agents**, **Run agents**, **Publish agents**, and
the read-only **View runs**, **View work**, **View documents**, and **View
files** permissions. It does not permit task, document, or file writes. Use
**Review** for read-only access. For writes, use **Custom** with the applicable
**Manage work**, **Edit documents**, or **Edit files** permission, or choose
**Full access**.

Users manage and revoke the connection in **Settings > Connected apps**.
Reducing access takes effect directly. Increasing permissions or changing to a
broader profile requires explicit OAuth reauthorization so a background agent
cannot expand its own grant.

Claude, Codex, ChatGPT, or another MCP host owns the local connection session
and its own confirmation controls. A host may narrow access or require an extra
confirmation, but it cannot grant permissions that Erstan denied. Revoking the
connection in Erstan invalidates it for every host session that uses it.

This two-layer model keeps permissions understandable:

1. Erstan decides the maximum authorized data and actions.
2. The AI host decides whether to invoke an action within that boundary.

## Install in Codex

After this repository is published to GitHub:

```text
codex plugin marketplace add erstanai/erstan-agent-tools
codex plugin add erstan@erstan
```

Open a new Codex session if needed, connect the `erstan` MCP server when
prompted, and complete the browser sign-in. The plugin uses the remote server
configuration in `plugins/erstan/.mcp.json`.

## Install in Claude Code

```text
claude plugin marketplace add erstanai/erstan-agent-tools
claude plugin install erstan@erstan
```

Run `/reload-plugins`, then `/mcp` to connect `erstan` and complete OAuth.
Claude Code initiates the remote MCP authorization; the plugin does not ask the
user to paste a bearer token or API key.

## ChatGPT and other hosts

ChatGPT uses the same hosted MCP endpoint through an approved OpenAI connector
or app listing. This repository intentionally does not contain `.app.json`: an
OpenAI app mapping must use the real registered `plugin_asdk_app...` or
`asdk_app_...` identifier assigned during publication. See
[Publishing](docs/PUBLISHING.md) for the external handoff.

Other MCP clients can connect directly to:

```text
https://api.erstan.com/v1/mcp
```

The deployed server must expose standards-compliant OAuth discovery and
authorization. There is no static-key fallback in this distribution.

## Verify locally

Requirements: Node.js 20 or later, Python 3, and optionally Claude Code for its
native validator.

```text
npm run verify
python C:/Users/<you>/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py plugins/erstan
python C:/Users/<you>/.codex/skills/.system/skill-creator/scripts/quick_validate.py plugins/erstan/skills/erstan-agent-builder
claude plugin validate .
```

Run the skill validator once for each directory under
`plugins/erstan/skills`. CI runs the repository-owned release verifier; the
native Codex, Skill, and Claude validators remain required release gates because
their availability and schemas are owned by their respective hosts.

## Toolkit separation

This repository is the small, installable end-user distribution. Maintainer
CLI utilities and offline diagnostic fixtures belong in the separate
`erstan-agent-toolkit` repository and must not be copied into a plugin release.

## License

Apache License 2.0. See [LICENSE](LICENSE).
