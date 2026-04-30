<p align="center">
  <img src="logo.png" alt="Rollie" width="80" />
</p>

<h1 align="center">Rollie Jobs MCP</h1>

<p align="center">
  Real-time, hyper-local and hyper-industry workforce intelligence — know what's hiring in your region / industry, and connect job seekers with real opportunities using our built-in job seeker CRM.
</p>

---

## What is Rollie Jobs?

Rollie Jobs turns scattered job postings into actionable workforce intelligence for the organizations that shape regional economies. Chambers of commerce, economic development organizations (EDOs), workforce boards, universities, and local governments use Rollie to understand what's actually hiring in their region — automatically, without manual curation.

Rollie continuously scans employer career pages and aggregates live job data scoped to your community. You define your region and your employers; Rollie keeps the data current. The result is a real-time picture of your local labor market that you can use to track hiring trends, identify industry clusters, support workforce planning, power embedded job boards, and fuel grant applications and strategic reports.

The two core lenses:
- **Hyper-Local** — every open job across every employer you track in your geography
- **Hyper-Industry** — hiring activity filtered by sector, surfacing where specific industries are growing or contracting

## What This Plugin Does

This plugin connects Claude directly to your Rollie Jobs data via the [Model Context Protocol](https://modelcontextprotocol.io). You can query, analyze, and act on your workforce data in plain English — no dashboards, no exports, no manual lookups.

**Ask questions:** *"What are the most in-demand roles across our tracked employers right now?"*

**Run analysis:** *"Find every employer with more than 5 open manufacturing positions and tell me what skills they're asking for."*

**Write content:** *"Draft a workforce summary for our EDO's quarterly report based on current hiring data."*

**Spot trends:** *"Which industries in our region are growing based on job posting volume over time?"*

**Match job seekers:** *"Find open positions that would fit someone with a CDL and 3 years of logistics experience."*

**Manage your pipeline:** *"Show me all job seekers in the Active status who haven't been updated in 30 days."*

## Install

In Claude Code, run once:

```
/plugin marketplace add south-bend-code-works/rollie
```

Claude Code will prompt you to install the plugin. On first use, you'll be asked to log in to your Rollie account via OAuth — no API key needed.

## Skills

### `rollie-jobs` (auto-invoked)
Claude automatically activates this skill when you ask about workforce data, hiring trends, companies, jobs, or regional labor market questions. Just ask naturally — no command needed.

### `/import-seekers` (user-invoked)
A guided workflow for importing job seekers from a spreadsheet, CSV, or CRM export (Salesforce, HubSpot, etc.). Covers field mapping, custom field creation, dedup handling, and row-by-row upsert. Type `/import-seekers` to start.

## Available Tools (21 total)

### Identity
| Tool | What it does |
|------|-------------|
| `whoami` | Returns your identity, authorized organizations, permission level, and the full list of tools available to you. Always called first. |

### Companies
| Tool | What it does |
|------|-------------|
| `get_companies` | List all employers tracked by your org |
| `get_company` | Profile a single employer — industry, job count, data freshness |
| `search_companies` | Find employers by industry, type, or characteristic |
| `add_company_to_org` | Add employers to your tracked list. New companies enter Rollie's crawl queue — **jobs appear within 24 hours** as Rollie indexes their openings. |
| `remove_company_from_org` | Remove employers from your tracked list |

### Jobs
| Tool | What it does |
|------|-------------|
| `get_jobs` | Pull structured job listings from a specific employer |
| `search_jobs` | Semantic search across all jobs — finds by concept, not just keyword. "Nurse jobs" finds RN, Registered Nurse, Staff Nurse. |

### Job Classifiers
| Tool | What it does |
|------|-------------|
| `get_classifiers` | List AI classification lenses defined for your org (e.g., entry-level, CDL required, remote-eligible) |
| `get_classifier` | Get a single classifier definition |
| `create_classifier` | Define a new AI lens — Rollie runs it against all jobs in your org to answer questions like "how many jobs require a forklift cert?" |

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
| `get_seeker_field_definitions` | List org-defined custom fields (e.g., branch of service, discharge status, cohort) |
| `upsert_seeker_field_definition` | Create or update a custom field |
| `delete_seeker_field_definition` | Soft-delete a custom field |

### Org Configuration
| Tool | What it does |
|------|-------------|
| `get_org_members` | List staff members and their user IDs |
| `get_seeker_statuses` | List available seeker pipeline statuses |
| `get_seeker_groups` | List available seeker groups |

## MCP Only (Cursor, Windsurf, other clients)

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
