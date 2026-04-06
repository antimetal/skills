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
- **`/antimetal-mcp-setup`** — Configure the MCP connection (OAuth or API key)

### MCP Server Tools

Connects to Antimetal's remote MCP server at `https://mcp.antimetal.com`, providing:

| Tool | Description |
|------|-------------|
| `search_issues` | Paginated list of issues with severity, status, and environment |
| `get_issue_report` | Full investigative report—root cause, timeline, causal graph |
| `get_issue_fixes` | Remediation steps (code changes, CLI commands, context) |
| `investigate_issue` | Create a new issue and start async automated investigation |
| `get_artifact` | Retrieve raw evidence: logs, traces, metrics, events, files, topology |
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
│   ├── investigate/
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