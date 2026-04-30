<p align="center">
  <img src="logo.png" alt="Rollie" width="80" />
</p>

<h1 align="center">Rollie</h1>

<p align="center">
  A Claude Code plugin for organizations that walk alongside job seekers — query workforce data, analyze regional hiring, and import the people you serve.
</p>

---

Rollie is a CRM and workforce intelligence platform for organizations that help people find jobs. That includes veteran placement programs, high school counselors helping students find work or internships, colleges placing graduates, chambers of commerce, economic development organizations (EDOs), and workforce agencies — any organization that acts as a trusted guide in someone's job search journey.

This plugin connects Claude to your Rollie data so you can ask questions, run analysis, write content, and manage job seekers directly from your AI tools.

## What You Can Do

- **Query jobs and companies** — search by role, location, industry, or plain English
- **Analyze regional hiring trends** — spot role concentrations, skills gaps, employer growth
- **Write workforce content** — articles, reports, and insights grounded in real local data
- **Manage job seekers** — look up, add, update, and match clients to open positions
- **Import seekers** — bring in people from spreadsheets, CSVs, or CRM exports with the `/import-seekers` skill

## Install

In Claude Code, run once:

```
/plugin marketplace add south-bend-code-works/rollie
```

Claude Code will prompt you to install the plugin. On first use, you'll be asked to log in to your Rollie account via OAuth — no API key needed.

## Skills

### `rollie-jobs` (auto-invoked)
Claude automatically uses this skill whenever you ask about workforce data — companies, job listings, hiring trends, or regional labor market questions. You don't need to type a command; just ask naturally.

**Examples:**
- *"What manufacturing jobs do we have in Kosciusko County?"*
- *"Find companies with more than 10 open positions"*
- *"Write a summary of what our region is hiring for right now"*
- *"How many CNC machinist openings are there across all our employers?"*

### `/import-seekers` (user-invoked)
A guided workflow for importing job seekers from an external source. Walks you through profiling the file, mapping columns to Rollie fields, creating any needed custom fields, and importing records one by one with dedup handling.

**Works with:** spreadsheets, CSVs, Salesforce exports, HubSpot exports, or any tabular data.

**Type `/import-seekers` to start.**

## Available Tools (21 total)

### Identity
| Tool | What it does |
|------|-------------|
| `whoami` | Shows your identity, authorized organizations, permission level, and available tools. Always called first. |

### Companies
| Tool | What it does |
|------|-------------|
| `get_companies` | List all companies tracked by your org |
| `get_company` | Profile a single employer — size, industry, job count, data freshness |
| `search_companies` | Find companies by industry, type, or characteristic |
| `add_company_to_org` | Add employers to your tracked list. New companies enter a crawl queue — **jobs appear within 24 hours** as Rollie indexes their openings. |
| `remove_company_from_org` | Remove employers from your tracked list |

### Jobs
| Tool | What it does |
|------|-------------|
| `get_jobs` | Pull structured job listings from a specific employer |
| `search_jobs` | Semantic search across all jobs — natural language, finds by concept not just keyword |

### Job Classifiers
| Tool | What it does |
|------|-------------|
| `get_classifiers` | List AI classification lenses defined for your org (e.g., entry-level, CDL required) |
| `get_classifier` | Get a single classifier and its definition |
| `create_classifier` | Define a new AI lens — Rollie runs it against all jobs in your org |

### Job Seekers
| Tool | What it does |
|------|-------------|
| `get_seeker` | Look up a single job seeker by ID |
| `filter_seekers` | Find seekers by status, group, name, email, city, or state |
| `search_seekers` | Semantic search across seeker profiles and resumes |
| `upsert_seeker` | Create or update a seeker — handles dedup automatically |
| `find_seeker_duplicates` | Audit for likely duplicate seeker records |
| `delete_seeker` | Permanently delete a seeker record |

### Seeker Custom Fields
| Tool | What it does |
|------|-------------|
| `get_seeker_field_definitions` | List custom fields defined for your org |
| `upsert_seeker_field_definition` | Create or update a custom field |
| `delete_seeker_field_definition` | Soft-delete a custom field |

### Org Configuration
| Tool | What it does |
|------|-------------|
| `get_org_members` | List staff members and their user IDs |
| `get_seeker_statuses` | List available seeker statuses |
| `get_seeker_groups` | List available seeker groups |

## MCP Only (No Claude Code)

For Cursor, Windsurf, or other MCP clients, add to your MCP settings:

```json
{
  "mcpServers": {
    "rollie-db": {
      "url": "https://mcp.rolliejobs.com/sse"
    }
  }
}
```

The server uses OAuth 2.1 — your client will prompt you to authenticate on first connection.

## Learn More

- [rolliejobs.com](https://www.rolliejobs.com) — Product site
- [app.rolliejobs.com](https://app.rolliejobs.com) — Log in and manage your data
- [Model Context Protocol](https://modelcontextprotocol.io) — The open standard powering this integration

## License

MIT
