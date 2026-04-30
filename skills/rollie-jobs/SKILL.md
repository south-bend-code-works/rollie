---
name: rollie-jobs
description: Query Rollie workforce data — companies and jobs for an org. Use when the user asks about companies they track, job listings, or job search.
user-invocable: false
---

# Rollie Jobs

You have access to Rollie's workforce data tools via the `rollie-db` MCP server. Always start by calling `whoami` to understand your permissions, organizations, and available tools.

## Getting Started

1. **Call `whoami` first** — it tells you which organizations you belong to, your permission level, and which tools you can use.
2. **Multi-org users**: If you belong to multiple orgs, ask which one they want to work with. Pass `org_id` to tools to query a specific org.

## Tool Reference

| Tool | Use when... |
|------|-------------|
| `whoami` | Starting a session, checking permissions |
| `get_companies` | "What companies do we track?", "Show me all our companies" |
| `get_company` | "Tell me about Best Buy", "What's the status of apllogistics.com?" |
| `get_jobs` | "Show me jobs at Best Buy", "What positions does Sturgis Bank have?" |
| `search_jobs` | "Find remote engineering jobs", "Warehouse jobs in Indiana" |
| `search_companies` | "Find companies in logistics", "What healthcare companies do we have?" |

## Key Conventions

- **Company IDs are domains**: `bestbuy.com`, `apllogistics.com`, `sturgis.bank`. Always normalize — `www.bestbuy.com` and `https://careers.bestbuy.com` both become `bestbuy.com`.
- **Job search is semantic**: `search_jobs` uses Gemini embeddings — natural language works great. "Nurse jobs near Chicago" will find relevant results even if the title says "RN" or "Registered Nurse".
- **Field filtering**: `get_jobs` accepts a `fields` param to reduce response size. Use `fields="job_title,job_locations,job_link"` for summaries, or `fields="all"` when you need descriptions.

## Response Patterns

- **Company overview**: Call `get_company` first, then `get_jobs` with a small limit for a quick summary.
- **Job search**: Use `search_jobs` for natural language. Use `get_jobs` with `fields` for structured listing.
- **Data freshness**: Check `jobs_refreshed_at` on the company doc.
- **Format tables**: When showing multiple jobs or companies, use markdown tables for readability.
