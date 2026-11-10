# Antimetal Skills

Bring [Antimetal's](https://antimetal.com) software investigation intelligence into Claude Code and Cursor. Triage problems, investigate root causes, fetch observability artifacts, and apply remediations—all from your terminal.

## Installation

### Claude Code

```bash
/plugin marketplace add antimetal/skills
/plugin install antimetal
```

> **Migrating from [`antimetal/claude-plugin`](https://github.com/antimetal/claude-plugin)?** Uninstall the old plugin first:
> ```bash
> /plugin uninstall antimetal
> /plugin marketplace remove antimetal
> ```
> Then run the install commands above.

### Cursor

Install from the [Cursor Marketplace](https://cursor.com/marketplace) by searching for **Antimetal**, or:

```bash
/add-plugin antimetal/skills
```

## What's Included

### Skills

- **`/investigate`** — Entry point for any software problem. Searches issues, queries Antimetal's AI, reads investigative reports and causal graphs, and routes to fix
- **`/fix`** — Apply Antimetal's remediation to your codebase, adapted for your local environment
- **`/review-priorities`** — List the most urgent issues, explain why they rank where they do, and report a better order
- **`/configure-semantic-tags`** — Define investigation and remediation vocabularies and selection criteria for future runs
- **`/configure-severity-rules`** — Update organization severity guidance for future assessments
- **`/antimetal-mcp-setup`** — Configure the MCP connection (OAuth or API key)

### MCP Server Tools

Connects to Antimetal's remote MCP server at `https://mcp.antimetal.com`, providing:

| Tool | Description |
|------|-------------|
| `search_issues` | Issues with severity, status, and environment, newest first or ranked by priority |
| `get_issue_report` | Full investigative report—root cause, timeline, causal graph |
| `get_issue_fixes` | Remediation steps (code changes, CLI commands, context) |
| `investigate_issue` | Create a new issue and start async automated investigation |
| `get_artifact` | Retrieve raw evidence: logs, traces, metrics, events, files, topology |
| `submit_ranking_feedback` | Correct the order of a priority list you were served, with the reason |
| `submit_tool_feedback` | Report a confusing result, missing field or workaround in Antimetal's tools |
| `get_semantic_tag_configuration` / `update_semantic_tag_configuration` | Read and update an organization's tag definitions for one source |
| `get_severity_rules` / `update_severity_rules` | Read and update organization severity guidance |
| `ask` | Ask Antimetal's AI about infrastructure, software, deployments, telemetry, logs, code |

## Authentication

### Claude Code: OAuth (Automatic)

When you first use an Antimetal tool or skill, Claude Code will open a browser window to log in to your Antimetal account via OAuth. Tokens are stored securely and refreshed automatically.

### Cursor: API Key

1. Sign up at [antimetal.com](https://antimetal.com) and generate an API key from [API Keys Settings](https://overlook.antimetal.com/settings/api-keys)
2. Set the environment variable:
   ```bash
   export ANTIMETAL_API_KEY="your-api-key-here"
   ```
3. Add this to your shell profile (`~/.zshrc`, `~/.bashrc`, etc.) so it persists
4. Restart Cursor to pick up the environment variable

## Repository Structure

```
antimetal/skills/
├── .claude-plugin/          # Claude Code config
│   ├── marketplace.json
│   └── plugin.json
├── .cursor-plugin/          # Cursor config  
│   └── plugin.json
├── skills/                  # Agent skills
│   ├── antimetal-mcp-setup/
│   ├── configure-semantic-tags/
│   ├── configure-severity-rules/
│   ├── investigate/
│   ├── review-priorities/
│   └── fix/
├── .mcp.json               # Claude Code MCP config
├── mcp.json                # Cursor MCP config
└── README.md
```

## Support

- [Documentation](https://docs.antimetal.com/connect)
- [GitHub Issues](https://github.com/antimetal/skills/issues)
- [Antimetal](https://antimetal.com)
- Email: support@antimetal.com