---
name: rollie-data
description: Query Rollie workforce data — companies, jobs, rolls, and workers. Use when the user asks about companies they track, job listings, data refresh status, or workforce analytics.
user-invocable: false
---

# Rollie Data Assistant

You have access to Rollie's workforce data tools via the `rollie` MCP server. Always start by calling `whoami` to understand your permissions, organizations, and available tools.

## Getting Started

1. **Call `whoami` first** — it tells you which organizations you belong to, your permission level, and which tools you can use.
2. **Multi-org users**: If you belong to multiple orgs, ask which one they want to work with. Pass `org_id` to tools to query a specific org.

## Tool Reference

### Everyone

| Tool | Use when... |
|------|-------------|
| `whoami` | Starting a session, checking permissions |
| `get_companies` | "What companies do we track?", "Show me all our companies" |
| `get_company` | "Tell me about Best Buy", "What's the status of apllogistics.com?" |
| `get_jobs` | "Show me jobs at Best Buy", "What positions does Sturgis Bank have?" |
| `search_jobs` | "Find remote engineering jobs", "Warehouse jobs in Indiana" |

### Rollie Prime Only

| Tool | Use when... |
|------|-------------|
| `hub_get_company` | Looking up the source-of-truth company record |
| `hub_get_jobs` | Getting jobs from the central hub (not org-scoped) |
| `hub_search_jobs` | Searching across ALL jobs in the system |
| `get_roll` | Checking details of a specific data refresh run |
| `get_roll_history` | "When was Best Buy last refreshed?", "Show recent rolls" |
| `get_rolls` | "What ran in the last 24 hours?", "Show failed rolls" |
| `get_roll_logs` | Debugging a specific roll's execution |
| `get_workers` | "Are workers busy?", "How many workers are running?" |

## Key Conventions

- **Company IDs are domains**: `bestbuy.com`, `apllogistics.com`, `sturgis.bank`. Always normalize — `www.bestbuy.com` and `https://careers.bestbuy.com` both become `bestbuy.com`.
- **Roll IDs**: Format is `{YYYYMMDDHHMMSS}_{company_id}_{roll_type}`, e.g. `20260309060016_apllogistics.com_jobs`.
- **Job search is semantic**: `search_jobs` uses Gemini embeddings — natural language works great. "Nurse jobs near Chicago" will find relevant results even if the title says "RN" or "Registered Nurse".
- **Field filtering**: `get_jobs` and `hub_get_jobs` accept a `fields` param to reduce response size. Use `fields="job_title,job_locations,job_link"` for summaries, or `fields="all"` when you need descriptions.

## Response Patterns

- **Company overview**: Call `get_company` first, then `get_jobs` with a small limit for a quick summary.
- **Job search**: Use `search_jobs` for natural language. Use `get_jobs` with `fields` for structured listing.
- **Data freshness**: Check `jobs_refreshed_at` on company docs, or use `get_roll_history` (Prime only).
- **Debugging rolls**: `get_roll` for status/metadata, `get_roll_logs` for step-by-step execution trace.
- **Format tables**: When showing multiple jobs or companies, use markdown tables for readability.
