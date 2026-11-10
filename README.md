# Antimetal Skills

Bring [Antimetal](https://antimetal.com) into Claude Code and Cursor. Find and investigate Issues happening now and Improvements Antimetal found before they hurt, read root causes and evidence, and apply fixes from your editor.

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

- **`/investigate`**: entry point for anything wrong or worth fixing. Searches Issues and Improvements, queries Antimetal's AI, reads investigative reports and causal graphs, and routes to fix
- **`/fix`**: applies Antimetal's fix for an Issue or Improvement to your codebase, adapted for your local environment
- **`/review-priorities`**: lists the most urgent Issues, explains why they rank where they do, and reports a better order
- **`/configure-semantic-tags`**: defines investigation and remediation vocabularies and selection criteria for future runs
- **`/configure-severity-rules`**: updates organization severity guidance for future assessments
- **`/antimetal-mcp-setup`**: configures the MCP connection (OAuth or API key)

### MCP Server Tools

Connects to Antimetal's remote MCP server at `https://mcp.antimetal.com`, providing:

| Tool | Description |
|------|-------------|
| `search_issues` | Issues with severity, status, and environment, newest first or ranked by priority |
| `get_issue_report` | Full investigative report: root cause, timeline, causal graph |
| `get_issue_fixes` | The fix: a code diff or operational steps, and Antimetal's pull request when it opened one |
| `investigate_issue` | Create a new issue and start async automated investigation |
| `get_artifact` | Retrieve raw evidence: logs, traces, metrics, events, files, topology |
| `ask` | Ask Antimetal's AI about infrastructure, software, deployments, telemetry, logs, code |
| `update_issue_status` | Set an Issue's status, for example resolved |
| `search_improvements` / `get_improvement_report` / `get_improvement_fixes` | Non-urgent Improvements, which use their own tools |
| `get_semantic_tag_configuration` / `update_semantic_tag_configuration` | Read and update an organization's tag definitions for one source |
| `get_severity_rules` / `update_severity_rules` | Read and update organization severity guidance |
| `submit_ranking_feedback` | Correct the order of a priority list you were served, with the reason |
| `submit_tool_feedback` | Report a confusing result, missing field or workaround in Antimetal's tools |
| `list_related_work` / `attach_link` | Pull requests, commits, tickets and docs linked to an Issue or Improvement, and linking new ones |

The full tool list depends on your organization's setup.

## Authentication

### Claude Code and Cursor: OAuth (Automatic)

When you first use an Antimetal tool or skill, your editor opens a browser window to log in to your Antimetal account via OAuth. Tokens are stored securely and refreshed automatically. If a token expires or is revoked, the editor asks you to log in again.

### API Key (fallback)

If your editor cannot use OAuth, generate an API key from [API Keys Settings](https://overlook.antimetal.com/settings/api-keys) and follow `/antimetal-mcp-setup`. Cursor reads environment variables only as `${env:ANTIMETAL_API_KEY}`.

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