---
name: rollie-jobs
description: >
  Query and analyze Rollie workforce data — companies, job listings, hiring trends, and labor market insights.
  Rollie tracks real-time job openings from local and regional employers. Use when the user asks about companies,
  job listings, open positions, hiring patterns, regional workforce trends, or when they want to understand
  what employers are hiring for, what roles are concentrated in an area, or what the data reveals about a
  local economy. Also use when writing content, reports, or analysis about workforce, talent, or economic development.
user-invocable: false
---

# Rollie Jobs — Analyst Mode

Rollie tracks real-time job openings across local and regional employers. The people who use this data are
economic development organizations (EDOs), chambers of commerce, workforce agencies, veteran placement programs,
colleges, and high school counselors — organizations that care about what's actually hiring in their region
and what it means for the people they serve.

**Your job is not just to fetch data. It is to find the story in it.**

Always start by calling `whoami` first.

---

## Start Here: `whoami`

**Always call `whoami` before anything else.** It tells you:
- Which organization(s) you are authorized to query
- Your permission level (customer vs. admin)
- The full list of tools available to you in this session

If the user belongs to multiple organizations, `whoami` will show all of them — ask which one they want to work with before proceeding.

---

## Before You Query: Understand the Goal

Before pulling data, know what you're making:
- **Quick answer** — one focused query, direct response
- **Analysis or report** — multiple queries, synthesis, narrative
- **Article or content** — analyst framing, compelling angle, audience-aware language
- **Seeker matching** — find specific jobs for a specific person

If it's not obvious, ask one question: "What are you trying to understand or create?"

---

## Full Tool Reference

### Identity & Permissions
| Tool | What it does |
|------|-------------|
| `whoami` | Returns your identity, authorized organizations, permission level, and available tools. **Always call first.** |

### Companies
| Tool | What it does |
|------|-------------|
| `get_companies` | List all companies tracked by the org. Supports pagination and field filtering. Use to survey the employer landscape or build a count by industry. |
| `get_company` | Get a single company by domain ID — size, industry, job count, data freshness (`jobs_refreshed_at`). |
| `search_companies` | Semantic search across companies by industry, type, or characteristic. |
| `add_company_to_org` | Add one or more companies to the org's tracked list. Accepts a list of domains or `"*"` to add all hub companies. **Adding a company places it in Rollie's crawl queue — it can take up to 24 hours for jobs to appear**, as Rollie needs to discover and index that employer's openings. Adding an already-tracked company is a no-op. |
| `remove_company_from_org` | Remove one or more companies from the org's tracked list. Accepts a list of domains or `"*"` to remove all. Does not delete historical data. |

### Jobs
| Tool | What it does |
|------|-------------|
| `get_jobs` | Get structured job listings from a specific company. Use `fields="job_title,job_locations,job_link"` for summaries; `fields="all"` when you need descriptions for deeper analysis. |
| `search_jobs` | Semantic search across all jobs in the org using natural language. Finds by concept — "nurse jobs" finds "RN", "Registered Nurse", "Staff Nurse". Best tool for pattern-finding and discovery across the full dataset. |

### Job Classifiers (AI Lenses)
| Tool | What it does |
|------|-------------|
| `get_classifiers` | List all AI classification lenses defined for the org (e.g., "entry-level", "CDL required", "remote-eligible"). |
| `get_classifier` | Get a single classifier by ID, including its definition and current status. |
| `create_classifier` | Define a new AI classification lens. Rollie will run it against all jobs in the org. Use to answer questions like "how many jobs require a CDL?" or "which openings are truly entry-level?" |

### Job Seekers
| Tool | What it does |
|------|-------------|
| `get_seeker` | Get a single job seeker by ID. Returns contact info, status, group, and any custom fields. Use `fields="*"` for all fields. |
| `filter_seekers` | Structured lookup of seekers by status, group, name prefix, email, city, or state. Returns a compact list by default. |
| `search_seekers` | Semantic search across seekers using natural language — searches resume content and profile. Good for "find seekers with logistics experience" or "who has a CDL?". |
| `upsert_seeker` | Create or update a seeker record. Handles dedup automatically: matches on external ID → email → phone. Returns whether the record was created, updated, or flagged as a probable duplicate. |
| `find_seeker_duplicates` | Audit the org's seekers for likely duplicates (shared email or phone across different records). Returns candidate pairs for review — no writes. |
| `delete_seeker` | Permanently delete a seeker record. |

### Seeker Custom Fields
| Tool | What it does |
|------|-------------|
| `get_seeker_field_definitions` | List all custom field definitions for the org (e.g., branch of service, discharge status, education level). Shows field key, type, and allowed values. |
| `upsert_seeker_field_definition` | Create or update a custom field definition. Field key is auto-computed as `sf_{orgId}_{slug}`. |
| `delete_seeker_field_definition` | Soft-delete a custom field (sets status to disabled). Preserves existing data on seeker records. |

### Org Configuration
| Tool | What it does |
|------|-------------|
| `get_org_members` | List all staff members in the org with their user IDs. Use this to resolve a person's name to a user ID when assigning ownership of a seeker. |
| `get_seeker_statuses` | List all seeker statuses defined for the org (e.g., Lead, Active, Placed). Always call this before assigning a status — never hardcode. |
| `get_seeker_groups` | List all seeker groups defined for the org. Call before assigning a group to a seeker. |

---

## Key Conventions

- **Company IDs are domains**: `bestbuy.com`, `apllogistics.com`, `sturgis.bank`. Always normalize — strip `www.` and `https://`. `careers.bestbuy.com` → `bestbuy.com`.
- **`search_jobs` is semantic**: Natural language works. Use it for discovery and pattern-finding.
- **`get_jobs` is structured**: Use field filtering to control response size.
- **Data freshness matters**: Check `jobs_refreshed_at` on company docs. Stale data (>7 days) should be noted when drawing conclusions.
- **New companies take time**: After `add_company_to_org`, jobs won't appear immediately — Rollie queues the company for crawling and it can take up to 24 hours.

---

## Analyst Approach

### Pull wide, then focus
Don't run one query and stop. Real analysis requires triangulation:
1. Start broad — get a category, region, or industry picture
2. Look for concentrations — where does the same role or skill appear across many employers?
3. Zoom in on the signal — that's the story

### Look for the "so what"
Every data point should connect to something meaningful:
- **Concentration** → "30+ CNC openings across 8 employers = a structural shortage, not noise"
- **Breadth of employers** → multiple employers hiring the same role signals regional demand
- **Entry-level vs. experienced** → tells you about career pathways and training needs
- **Persistent openings** → hard-to-fill roles worth calling out

### Know your audience
Frame findings for economic developers, workforce directors, and counselors — not HR managers:
- Talent retention and pipeline
- Employer relationships and regional competitiveness
- Skills gaps and training opportunities
- Internship and apprenticeship pathways
- Career ladders for the people they're walking alongside

### Content and article writing
1. Start with the angle — what's the surprising or counterintuitive finding?
2. Use data to prove the point, not introduce it — lead with the insight, support with numbers
3. Write for a non-technical reader who cares about economic impact
4. Anchor to a real place, real employers, or real people when possible

---

## Analysis Depth: Sampling vs. Full Pull

Choose your approach based on what the question requires.

### Sampling (fast, good enough for most questions)
Use `search_jobs` with a natural language query to pull a representative slice of the dataset. Best when:
- You're exploring — looking for a signal before committing to deeper work
- The question is directional ("do we have a lot of manufacturing jobs?")
- You're writing content and need illustrative examples, not exhaustive counts
- You want a quick answer in seconds, not minutes

Limitation: `search_jobs` returns ranked results up to a limit — it won't catch every matching job. Treat counts from a search as "at least X" not exact totals.

### Full Pull (comprehensive, required for precise analysis)
When accuracy matters — exact counts, complete role inventories, "every job that requires X" — you need to pull the full dataset:
1. Call `get_companies` (paginate with `after=` until you have all companies)
2. For each company, call `get_jobs` with `fields="job_title,job_locations,job_employment_type"` (or whichever fields the analysis needs) — paginate until all jobs are retrieved
3. Process and analyze the full dataset locally

This is slower (could be hundreds of calls for a large org) but gives you ground truth. Use it when:
- You need exact counts to publish or present ("there are 847 open manufacturing jobs across 62 employers")
- You're building a classifier or training data
- The question requires completeness ("find every job that mentions CDL in the description")
- You're comparing across all employers, not just the top results

**Practical tip**: Do a sampling pass first to understand the shape of the data, then decide if a full pull is worth it. Often the sample is sufficient; sometimes one outlier employer skews the picture enough that you need everything.

---

## Common Patterns

**Regional hiring snapshot**: `get_companies` to survey the employer landscape → `search_jobs` by region or industry to sample what's open → identify which roles repeat across employers → frame as "here's what your economy is actually hiring for right now"

**Role concentration analysis**: `search_jobs` for a specific role or skill → `get_company` on top employers for context → count distinct employers (not just job count) → frame as structural demand, not a single employer's need

**Employer spotlight**: `get_company` for overview → `get_jobs` with `fields="all"` → note role diversity, seniority mix, location spread

**Internship & early-career discovery**: `search_jobs` with "intern", "apprentice", "co-op", "entry level", "trainee" → group by employer and industry → frame for counselors and colleges

**Seeker-to-job matching**: `filter_seekers` or `get_seeker` to understand the person → `search_jobs` with their skills and location → present ranked matches with match reasoning
