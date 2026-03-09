<p align="center">
  <img src="logo.png" alt="Rollie" width="80" />
</p>

<h1 align="center">Rollie</h1>

<p align="center">
  Your friendly roly-poly workforce assistant, ready to roll up and help you explore your regional workforce data.
</p>

---

Rollie helps economic development organizations track companies, jobs, and hiring activity across their region. This plugin brings all that data right into your AI tools — just ask Rollie what you want to know.

**"How many jobs does Best Buy have?" "Find me warehouse jobs in Indiana." "When was our data last refreshed?"** — Rollie's got you covered.

## What Rollie Can Do

- **Search jobs** across all your organizations using plain English
- **Look up companies** — profiles, job counts, job board membership, refresh status
- **Explore your workforce** — trends, hiring activity, regional insights
- **Check data health** — when things last refreshed, what's running, what failed

## Setup

### Claude Cowork

1. Go to **Browse plugins** → **Personal** → **+** → **"Add marketplace by URL"**
2. Paste the marketplace URL from your [Rollie profile](https://app.rolliejobs.com) under **AI Integrations**
3. Click **Sync**

### Claude Code

Add to `.mcp.json` in your project root (or `~/.claude/.mcp.json` for global access):

```json
{
  "mcpServers": {
    "rollie": {
      "url": "https://rollie-db-mcp-4qqvujfleq-uc.a.run.app/sse",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

### Cursor / Windsurf / Other MCP Clients

Most MCP-compatible tools use the same config format above. Add it to your tool's MCP settings.

## Getting Your API Key

1. Log in to [Rollie](https://app.rolliejobs.com)
2. Go to **My Profile** → **AI Integrations**
3. Click **New Connection** and give it a label
4. Copy the key — you'll only see it once

## Available Tools

| Tool | What Rollie Does | Who Can Use It |
|------|-----------------|----------------|
| `whoami` | Introduces himself and shows your orgs and permissions | Everyone |
| `get_companies` | Lists all the companies your org tracks | Everyone |
| `get_company` | Digs into a specific company's profile | Everyone |
| `get_jobs` | Pulls up jobs for a company | Everyone |
| `search_jobs` | Searches jobs using natural language | Everyone |
| `hub_get_company` | Looks up the source-of-truth company record | Rollie Prime |
| `hub_get_jobs` | Gets jobs from the central hub | Rollie Prime |
| `hub_search_jobs` | Searches across ALL jobs in the system | Rollie Prime |
| `get_roll` | Shows details of a data refresh run | Rollie Prime |
| `get_roll_history` | Shows recent refresh history for a company | Rollie Prime |
| `get_rolls` | Queries rolls by status, type, or time | Rollie Prime |
| `get_roll_logs` | Shows step-by-step execution logs | Rollie Prime |
| `get_workers` | Checks on the worker crew | Rollie Prime |

## How It Works

This plugin connects to Rollie's [MCP](https://modelcontextprotocol.io) server, giving your AI assistant read-only access to your organization's workforce data. Your API key determines which organizations you can access.

- **Regular users** see data scoped to their organizations
- **Rollie Prime users** get the full toolkit — hub data, roll management, and worker status

## Learn More

- [Rollie](https://www.rolliejobs.com) — Marketing site
- [Rollie App](https://app.rolliejobs.com) — Log in and manage your data
- [MCP (Model Context Protocol)](https://modelcontextprotocol.io) — The open standard that makes this work

## License

MIT
