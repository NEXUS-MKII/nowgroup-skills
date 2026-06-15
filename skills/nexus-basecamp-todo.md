---
name: nexus-basecamp-todo
description: "Use when a user (Chris White, NOW Group) wants to create, list, or update Basecamp 5 to-dos / to-do lists / projects from code or from a NEXUS workflow. Triggers: 'add a to-do in Basecamp', 'create a Basecamp task', 'list my Basecamp projects', 'fire a basecamp job', 'wire X to Basecamp', any NEXUS automation that should drop work into Basecamp, or any question about the `nexus_basecamp.py` integration. Also use when a request mentions both Basecamp and code/CLI/programmatic/API in the same breath. Do NOT use for: Basecamp Classic, Basecamp 2, or Basecamp 3 (this is BC5 only); generic project-management advice; or any non-NOW-Group Basecamp account."
---

# NEXUS x Basecamp to-do integration

A skill for creating, listing, and updating Basecamp 5 to-dos from code or NEXUS workflows. The orchestrating script (`nexus_basecamp.py`) lives in the **private NEXUS MKII working directory** — this skill is the **public reference** for how to call it.

## Account context

- **Account:** "NOW Group" (renamed from "RISE Coaching" on 2026-06-16)
- **Login identity:** `chris@nowgroup.co.nz` (was `chris@risewellness.co.nz` pre-rename)
- **Product:** Basecamp 5, served by the `bc3` API surface (37signals kept the API stable through product renames)
- **Account ID + API base:** discovered automatically from `launchpad.37signals.com/authorization.json` — do not hardcode

## Files (on Chris's NEXUS MKII machine)

| File | Role |
|---|---|
| `nexus_basecamp.py` | The orchestrating script — stdlib only, no extra deps |
| `basecamp_token.json` | Access + refresh tokens (chmod 600, gitignored, **NEVER commit**) |
| `.env` → `BASECAMP_CLIENT_ID` + `BASECAMP_CLIENT_SECRET` | OAuth app credentials |

## One-time setup (already done)

OAuth app registered at https://launchpad.37signals.com/integrations as "NEXUS MKII" with redirect URI `http://localhost:8788/basecamp/callback`. Client ID + Secret in `.env`. Token issued via:

```bash
python3 nexus_basecamp.py auth
```

This opens a browser, captures the auth code via a one-shot local listener on port 8788, exchanges for tokens, fetches account info, and writes `basecamp_token.json`.

## CLI surface

```bash
# Discover what's there
python3 nexus_basecamp.py whoami
python3 nexus_basecamp.py projects                      # all active projects + IDs
python3 nexus_basecamp.py todolists --project <id>      # lists in a project's to-do set

# Create
python3 nexus_basecamp.py create-list --project <id> --name "..."
python3 nexus_basecamp.py todo --project <id> --todolist <id> --content "..." \
    [--description "..."] [--due YYYY-MM-DD] [--notify]

# Maintenance
python3 nexus_basecamp.py refresh                       # force a token refresh (auto on 401 anyway)
```

## Programmatic surface (from any NEXUS script)

```python
from nexus_basecamp import BasecampClient

bc = BasecampClient()                                   # loads token from disk
projects = bc.list_projects()                           # [{id, name, ...}, ...]
lists = bc.list_todolists(project_id)                   # [{id, name, completed_ratio}, ...]
todo = bc.create_todo(
    project_id, todolist_id,
    content="Send sign-off email to Lara",
    description="Optional long-form description (HTML allowed)",
    due_on="2026-06-23",
    assignee_ids=[123],
    notify=True,
)
# returns the created to-do dict including .id and .app_url
```

Auth + refresh are handled inside `BasecampClient._call`: a 401 triggers a transparent token refresh and one automatic retry. Caller code doesn't need to know.

## Discovering project + todolist IDs

Project IDs are not in this public skill on purpose (internal NOW Group business structure). Discover at runtime:

```bash
python3 nexus_basecamp.py projects                          # human-readable
python3 -c "from nexus_basecamp import BasecampClient; \
            import json; print(json.dumps(BasecampClient().list_projects(), indent=2))"
```

For NEXUS workflows that target a specific project repeatedly, store the project_id + todolist_id in the relevant profile / config so you're not paying the lookup on every call.

## Known operational gotchas

### Identity rekey revokes all tokens

If the Basecamp account email or login ever changes again (as it did 2026-06-16 going from risewellness → nowgroup), 37signals invalidates every existing OAuth token. Symptom on the next API call:

```
401 {"error":"OAuth token expired (rekeyed_identity). Refresh your token or acquire a new one at ..."}
```

**Fix:** re-run `python3 nexus_basecamp.py auth`. The OAuth app registration at launchpad survives identity rekeys — only tokens get revoked.

### User-Agent is mandatory

37signals rejects requests without an identifiable `User-Agent` header. The script sets `User-Agent: NEXUS MKII (chris@nowgroup.co.nz)` on every call. Don't strip it.

### Token lifetime ~14 days

Access tokens expire in ~14 days; refresh tokens have indefinite life until revoked. Auto-refresh happens transparently on 401. If a refresh ever fails (token revoked, identity rekey, etc.), surface it loudly — re-auth requires Chris at the keyboard.

### Empty `todolists` is a valid state

A project may have its To-dos section enabled but zero lists in it. `list_todolists()` will return `[]` — don't treat that as an error. Use `create_todolist(project_id, name)` to bootstrap a list before creating to-dos in it.

## When NOT to use this skill

- **Basecamp Classic / BC2 / BC3** as a product (different API hosts, different OAuth flow)
- Any account other than NOW Group's
- Operations the script doesn't expose yet (comments, message-board posts, files, schedule entries, kanban cards) — those are separate endpoints; extend the script first rather than improvising a one-off
- Generic project-management workflow questions ("should I split this into multiple to-dos?") — that's a different skill

## Reference

- Basecamp API docs (covers BC3/BC4/BC5 — same API): https://github.com/basecamp/bc3-api
- 37signals OAuth: https://github.com/basecamp/api/blob/master/sections/authentication.md
- Launchpad apps console: https://launchpad.37signals.com/integrations
