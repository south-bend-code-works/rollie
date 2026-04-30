---
name: import-seekers
description: >
  Import job seekers into Rollie — a CRM for organizations that walk alongside people in their job search.
  Rollie users include veteran placement programs, high school counselors helping students find jobs or internships,
  colleges placing graduates, workforce development agencies, and any organization that acts as a trusted guide
  helping clients find the right position. Use this skill when the user wants to import, upload, load, bring in,
  or add a list of people — whether from a spreadsheet, CSV, CRM export (Salesforce, HubSpot, etc.), or any
  other file — into their Rollie org so they can begin tracking and supporting those individuals in their job search.
user-invocable: true
---

# Import Seekers

Rollie is a CRM for organizations that walk alongside job seekers — veteran placement programs, high school
counselors, colleges, workforce agencies, and others who act as a trusted guide helping clients navigate their
job search and land the right role.

You are importing job seekers from an external data source into a Rollie org. Follow this workflow exactly — do not skip the orient phase and do not write any records until the user approves the mapping.

## Phase 1: Orient (do this before touching any records)

1. **Profile the file** — for every column:
   - Count total non-empty values and distinct values
   - If distinct count is low (≤ 20), list every distinct value — these are candidates for `enum` type
   - If a column looks numeric, verify there are no text values mixed in before assigning `number` type
   - If a column looks like a date, confirm the format is consistent across all rows
   - Note any columns that are almost always empty (may not be worth importing)
2. **Call `get_seeker_field_definitions(org_id)`** — see what custom fields already exist for this org.
3. **Call `get_org_members(org_id)`** — get user IDs for all staff members so you can resolve owner names to IDs.
4. **Call `get_seeker_statuses(org_id)`** — get all available statuses so you can pick the right one for imported records. Present the options to the user and ask which status to assign. Do not default to `lead` or any hardcoded value.
5. **Call `get_seeker_groups(org_id)`** — get all groups. If groups exist, ask the user whether imported records should be assigned to one.

## Phase 2: Propose the Mapping

Produce a mapping table for the user to approve before writing anything:

| Source Column | Maps To | Notes |
|---|---|---|
| First Name + Last Name | `fullName`, `firstName`, `lastName` | Always combined |
| Email | `email` | Standard field |
| Phone | `phone` | Standard field |
| Address fields | geocoded → `address1`, `city`, `state`, `zip`, `address_geo_coord`, `address_h3_1–8` | Always geocode via Google Maps before storing — raw values are normalized, coordinates and H3 indexes derived |
| LinkedIn URL | `linkedInUrl` | Standard field — never create a custom field for this |
| Education (free text) | `education` | Standard field — never create a custom field for this |
| Created Date | `sourceCreatedAt` | Preserves original creation date from source system |
| Preferred region / region to work / job search area | `preferredRegions` + `h3Resolution` | Standard field — any column expressing *where the seeker wants to work* maps here, never to a custom field. `preferredRegions` is an array: collect ALL region values across however many source columns or delimited values exist (e.g. "Central; Northeast" or separate region1/region2 columns), geocode each name as a place query, convert each to an H3 cell at resolution 3, then expand each cell to a k-ring of radius 1 (the cell + its 6 neighbors) to cover the full regional area. Deduplicate, store as a single combined array. Set `h3Resolution: 3`. Max 30 cells — if deduped result exceeds 30, trim to 30. If no region column exists but an address was geocoded, seed `preferredRegions` from the k-ring(1) of `address_h3_3` as a fallback. |
| Contact Owner / Assigned To | `ownerIds` | Resolve name → userId via `get_org_members`. Pass as array — `upsert_seeker` will auto-build the denormalized `owners` array for display. If no match found, flag it. |
| Status | `statusId` | Use `get_seeker_statuses` to get valid options — never hardcode. Ask the user which status to assign to imported records. |
| Group | `groupId` | Use `get_seeker_groups` to see available groups. Ask the user if records should be assigned to one. |
| *Custom fields* | `sf_{orgId}_{slug}` | Only for fields with no standard equivalent |
| *New field not in schema* | **PAUSE — ask user** | Do not create without approval |

Flag any column that:
- Has no obvious mapping (ask the user what to do)
- Would duplicate a standard field as a custom field (point this out and use the standard field instead)
- Is a new field not yet in the schema (propose creating it, wait for approval before calling `upsert_seeker_field_definition`)

## Phase 3: Get Approval

Present the full mapping table and wait. Do not proceed until the user says yes.

If the user asks to create new custom fields, call `upsert_seeker_field_definition` for each one now — before importing any records.

## Phase 4: Import Row by Row

For each row:
1. Build the seeker payload using the approved mapping.
2. Set `externalIds: { "<source_name>": "<unique_id_from_source>" }` — use email as the external ID if no better key exists. This enables safe re-imports.
3. Call `upsert_seeker(seeker_data, org_id)`.
4. Handle the response:
   - `created` or `updated` → continue
   - `probable_duplicate` → show the candidates to the user, ask whether to `merge_id` or `confirmed_new`
   - Error → log it with the row data, skip, continue

After every 50 records, report a running count: created / updated / skipped.

## Key Rules

- **Never create a custom field for**: `email`, `phone`, `linkedInUrl`, `education`, `address1/2`, `city`, `state`, `zip`, `notes`, `careerGoal`, `preferredRegions`
- **Owner resolution**: Always use `get_org_members` to turn a name string into a `userId`. If the name doesn't match any member, store it in a custom string field and flag it for manual follow-up.
- **No batching**: One `upsert_seeker` call per record. Never batch writes.
- **Mapping is a session contract**: Once approved, use it consistently for every row. Only pause again if a row contains a value in a column that was marked "skip" or a column that wasn't in the original mapping.
- **Re-imports are safe**: `externalIds` matching means running the same file twice updates rather than duplicates.
