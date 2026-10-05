# SayBriefly REST API

The SayBriefly REST API powers the Zapier integration (and other automation tools). It gives you your
own SayBriefly meeting recaps and to-dos as JSON, and lets you add to-dos.

**Base URL:** `https://api.saybriefly.com/v1`

## Authentication

Every request needs your personal SayBriefly key:

```
Authorization: Bearer sbk_...
```

Create a key in SayBriefly: **Settings → Integrations → Connect your AI → Create key**. Keys start with
`sbk_`. A key reads and writes only the data of the user who created it, in that workspace. Remove a
key in the same place to revoke it at once.

## Endpoints

### `GET /me`
Who the key belongs to. Use it to test a connection.

```json
{ "id": "869c544a-...", "email": "you@studio.com", "name": "Maya Okafor", "workspace_id": "35231ee6-..." }
```

### `GET /meetings`
Recorded meetings that have a finished recap, newest first.

| Query | Default | Notes |
|---|---|---|
| `limit` | 25 | 1 to 100 |

Each item:

| Field | Type | Notes |
|---|---|---|
| `id` | string | Meeting id |
| `title` | string | |
| `start`, `end` | ISO 8601 | |
| `platform` | string | `zoom`, `google_meet`, `teams`, ... |
| `duration_minutes` | number or null | |
| `summary` | string | The recap |
| `key_decisions` | string[] or null | |
| `action_items` | array or null | `{text, assignee, context, mine}` |
| `participants` | string[] or null | |
| `since_last_call` | string or null | What changed since the previous call with this client |

### `GET /todos`
To-dos from the SayBriefly To-Do list, newest first.

| Query | Default | Notes |
|---|---|---|
| `status` | `all` | `all`, `open` or `done` |
| `limit` | 25 | 1 to 100 |

Each item:

| Field | Type | Notes |
|---|---|---|
| `id` | string | |
| `title` | string | |
| `notes` | string or null | |
| `status` | string | `pending`, `todo`, `in_progress` or `done` |
| `priority` | string | `low`, `medium`, `high`, `urgent` |
| `due_date` | `YYYY-MM-DD` or null | |
| `due_time` | string or null | |
| `project` | string or null | Project name |
| `source` | string | `manual`, `meeting`, `email`, `project`, ... |
| `source_title` | string or null | Meeting title or email subject it came from |
| `created_at` | ISO 8601 | |

### `POST /todos`
Add a to-do to the user's To-Do list.

```json
{ "title": "Send the sitemap to Acme", "due_date": "2026-10-10", "priority": "high", "notes": "From the kickoff", "project": "Acme website" }
```

| Field | Required | Notes |
|---|---|---|
| `title` | yes | Up to 300 characters |
| `due_date` | no | `YYYY-MM-DD` |
| `priority` | no | `low`, `medium` (default), `high`, `urgent` |
| `notes` | no | Up to 2,000 characters |
| `project` | no | Project name |

Returns the created to-do (same shape as `GET /todos`) with status `201`.

## Errors

JSON `{ "error": "message" }` with:

| Status | Meaning |
|---|---|
| 400 | Invalid input (for example a missing `title`) |
| 401 | Missing, invalid or revoked key |
| 404 | Unknown route |
| 500 | Server error, try again |

## Example

```bash
curl -H "Authorization: Bearer sbk_..." "https://api.saybriefly.com/v1/meetings?limit=5"
```

## Support

inbox@saybriefly.com · https://saybriefly.com/support · Privacy: https://saybriefly.com/privacy
