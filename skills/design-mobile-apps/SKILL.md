---
name: sleek-design-mobile-apps
description: Use when the user wants to design a mobile app or UI screens, when they mention their Sleek (sleek.design) projects, or when implementing Sleek designs in code (HTML, React Native, SwiftUI).
compatibility: Requires SLEEK_API_KEY environment variable. Network access limited to https://sleek.design only.
metadata:
  requires-env: SLEEK_API_KEY
  allowed-hosts: https://sleek.design
---

# Designing with Sleek

[![Design mobile apps in minutes](https://raw.githubusercontent.com/sleekdotdesign/agent-skills/main/assets/hero.png)](https://sleek.design)

## Overview

[sleek.design](https://sleek.design) is an AI-powered mobile app design tool. You interact with it via a REST API at `/api/v1/*` to create projects, describe what you want built in plain language, and get back rendered screens. All communication is standard HTTP with bearer token auth.

**Base URL**: `https://sleek.design`
**Auth**: `Authorization: Bearer $SLEEK_API_KEY` on every `/api/v1/*` request
**Content-Type**: `application/json` (requests and responses)
**CORS**: Enabled on all `/api/v1/*` endpoints
**Parsing responses**: write the body to a file (`curl -o run.json`) and parse the file. Don't pipe JSON through `echo`: in zsh it expands the escaped `\n` inside string values into real newlines, which makes the body invalid JSON.
**API docs**: OpenAPI spec at `https://sleek.design/api/v1/spec.json`; browsable docs at `https://sleek.design/api/v1/docs`. Fetch the spec for any contract detail not covered here.

---

## Two directions

Every run goes one way or the other, and the user's request says which.

**Designing** — the user asked for something new: an app, more screens, or a change to an existing screen. Read [designing.md](designing.md) before the first chat message; it carries the create → chat → screenshot loop, how to author a style direction, and when to seed one from a reference instead.

**Implementing** — the user named designs that already exist and wants them built as real code (React Native, SwiftUI, or an HTML prototype). Read [implementing.md](implementing.md) **before writing any line of code**: it decides whether the result works. A component document is a mockup, not a page, and the conversions it spells out — flex direction, `<Text>`, safe-area insets, the route tree — fail silently, so an app built without it compiles, runs, and is wrong.

---

## Prerequisites: API Key

If `SLEEK_API_KEY` is not set, use the device flow so the user never handles the raw key:

1. `POST https://sleek.design/api/v1/device/start` (no auth) with body `{"source": "your-tool-slug"}`. The response contains a `verificationUrl`, a human-checkable `userCode`, a secret `deviceCode`, and a poll `interval` in seconds.
2. Show the user the `verificationUrl` and the `userCode`, and tell them to confirm the code matches before approving.
3. Poll `POST https://sleek.design/api/v1/device/poll` with `{"deviceCode": "..."}` every `interval` seconds. When the user approves, the poll returns `{"status": "approved", "key": "sk_..."}` exactly once: store it as `SLEEK_API_KEY`. Codes expire after 15 minutes; on `expired`, start over.

Fallback: send the user to **https://sleek.design/agents/setup**, which handles sign-in, plan upgrade, and key creation in one place, and ask them to paste the key back to you. Keys can also be managed at **https://sleek.design/dashboard/api-keys**. The full key value is shown only once at creation.

**Plans**: free accounts can try the API with their one-time trial credits (about one design run), so a new user can see their first design before any payment decision. Sustained use requires the Pro plan or higher ($49.99/month, or $30/month billed yearly at $360/year; includes 20,000 monthly AI credits, roughly 650 screens). When cost becomes relevant (the user asks, an upgrade is needed to continue, or you're about to send them to a payment page), state this pricing plainly, including the yearly option. Never let a payment step come as a surprise.

### Key scopes

| Scope             | What it unlocks              |
| ----------------- | ---------------------------- |
| `projects:read`   | List / get projects          |
| `projects:write`  | Create / delete projects     |
| `components:read` | List components in a project |
| `chats:read`      | Get chat run status          |
| `chats:write`     | Send chat messages           |
| `screenshots`     | Render component screenshots |

Create a key with only the scopes needed for the task.

---

## Security & Privacy

- **Single host**: All requests go exclusively to `https://sleek.design`. No data is sent to third parties.
- **HTTPS only**: All communication uses HTTPS. The API key is transmitted only in the `Authorization` header to Sleek endpoints.
- **Minimal scopes**: Create API keys with only the scopes required for the task. Prefer short-lived or revocable keys.
- **Image URLs**: When using `imageUrls` in chat messages, those URLs are fetched by Sleek's servers. Avoid passing URLs that contain sensitive content.

---

## Quick Reference: All Endpoints

| Method   | Path                                           | Scope             | Description       |
| -------- | ---------------------------------------------- | ----------------- | ----------------- |
| `GET`    | `/api/v1/projects`                             | `projects:read`   | List projects     |
| `POST`   | `/api/v1/projects`                             | `projects:write`  | Create project    |
| `GET`    | `/api/v1/projects/:id`                         | `projects:read`   | Get project       |
| `DELETE` | `/api/v1/projects/:id`                         | `projects:write`  | Delete project    |
| `GET`    | `/api/v1/projects/:id/components`              | `components:read` | List components   |
| `GET`    | `/api/v1/projects/:id/components/:componentId` | `components:read` | Get component     |
| `GET`    | `/api/v1/references`                           | any valid key     | List references   |
| `POST`   | `/api/v1/projects/:id/chat/messages`           | `chats:write`     | Send chat message |
| `GET`    | `/api/v1/projects/:id/chat/runs/:runId`        | `chats:read`      | Poll run status   |
| `POST`   | `/api/v1/projects/:id/chat/runs/:runId/cancel` | `chats:write`     | Cancel run        |
| `POST`   | `/api/v1/screenshots`                          | `screenshots`     | Render screenshot |

Every row above is specified in full — request body, response shape, field defaults, status codes — in [endpoints.md](endpoints.md). This table is enough to pick an endpoint; read the reference when you need a contract detail it doesn't carry.

---

## Error Shapes

```json
{ "code": "UNAUTHORIZED", "message": "..." }
```

| HTTP | Code                    | When                                                    |
| ---- | ----------------------- | ------------------------------------------------------- |
| 401  | `UNAUTHORIZED`          | Missing/invalid/expired API key                         |
| 403  | `FORBIDDEN`             | Valid key, wrong scope or plan                          |
| 404  | `NOT_FOUND`             | Resource doesn't exist                                  |
| 400  | `BAD_REQUEST`           | Validation failure                                      |
| 409  | `CONFLICT`              | Another run is active for this project                  |
| 429  | `TOO_MANY_REQUESTS`     | Too many requests; back off and retry later             |
| 500  | `INTERNAL_SERVER_ERROR` | Server error                                            |

`401`, `403`, and `429` bodies may include `data.url`: a page where the user can fix the condition (create a key, upgrade the plan). When present, share that URL with the user instead of improvising one.

Chat run-level errors (inside `data.error`):

| Code               | Meaning                               |
| ------------------ | ------------------------------------- |
| `out_of_credits`   | Organization has no credits left      |
| `execution_failed` | AI execution error                    |
| `cancelled`        | Run cancelled via the cancel endpoint |

An `out_of_credits` error includes `error.url`, the page where the user can top up credits. Relay it to the user; don't retry the run until they have.

---

## Pagination

All list endpoints accept `limit` (1–100, default 50) and `offset` (≥0). The response always includes `pagination.total` so you can page through all results.

```http
GET /api/v1/projects?limit=10&offset=20
```

---

## Common Mistakes

| Mistake                                                                 | Fix                                                                                                  |
| ----------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| Omitting `source` on chat messages                                      | Always send `source` so the run is attributed in the Sleek editor                                    |
| Using `wait=true` on long generations                                   | It blocks 300s max; have a fallback to polling for `202` response                                    |
| Assuming `result` is present on `202`                                   | `result` is absent until status is `completed`                                                       |
| Piping a JSON response through `echo` to parse it                       | zsh expands the `\n` in `assistantText` and breaks the JSON; parse from a file instead               |
| Treating an unreadable run status as "not done yet"                     | The loop then spins to its cap long after the run finished; stop and report instead                  |
| Calling a screen incomplete based on a viewport screenshot              | The content is usually below the fold; re-shoot with `fullHeight: true` or check the component HTML before reporting anything missing |
| Using `screenId` as `componentIds` in screenshots                       | `screenId` and `componentId` are different; always use `componentId` from operations for screenshots |
| Confusing `versions[i].version` (number) with `versions[i].id` (string) | When resolving pinned versions, match by `id` (e.g. `ver_001`); `version` is the numeric index       |
