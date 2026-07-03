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
**API docs**: OpenAPI spec at `https://sleek.design/api/v1/spec.json`; browsable docs at `https://sleek.design/api/v1/docs`. Fetch the spec for any contract detail not covered here.

---

## Prerequisites: API Key

Create API keys at **https://sleek.design/dashboard/api-keys**. The full key value is shown only once at creation. Store it in the `SLEEK_API_KEY` environment variable.

**Required plan**: Pro or higher (API access is gated)

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

## Designing

The full request/response shapes for every endpoint used below are in the [API reference](#quick-reference-all-endpoints).

### 1. Create a project

Create a project with `POST /api/v1/projects` if one doesn't exist yet. Derive a name from the request.

Each project has its own theme, style, and design system. If the user wants multiple design variations, create a separate project for each variation.

### 2. Send a chat message

Send the request with `POST /api/v1/projects/:id/chat/messages`. Sleek has its own AI that plans screen content, visual style, and layout: pass the user's request as-is and let it plan. Don't add details the user didn't ask for, and don't decompose the request into screens; send the full intent as a single message. If the user described specific screens and styling, include those. Sleek produces richer designs when given room to plan.

**Seed a style with a reference**: Sleek curates a catalog of design references. When the user wants a specific look or asks for style options, list them with `GET /api/v1/references` (each has a `name` and `previewImageUrls` you can show) and pass the chosen id as `referenceId` on the first message to a project, so its style guide seeds the whole design.

**Identify your tool**: always send `source`, the slug of the tool making the request. The Sleek editor uses it to show the user who is designing while the run streams. Recognized values: `claude-code`, `claude`, `codex`, `chatgpt`, `cursor`, `openclaw`. If your tool isn't listed, send a short kebab-case slug for it anyway (max 64 chars). Unrecognized values are fine and get a generic label.

**Watch it live**: runs render in the Sleek editor in real time. After sending the first message to a project, tell the user they can watch their screens being designed live in Sleek, and share the editor link: `https://sleek.design/project/:projectId`. Don't open a browser yourself unless the user asks.

**Polling**: chat messages are async by default: you get a `runId` and poll `GET /api/v1/projects/:id/chat/runs/:runId`. Start at 2s interval, back off to 5s after 10s, give up after 5 minutes. You can also use `?wait=true` for a blocking call (up to 300s; falls back to polling if it times out with `202`).

**Editing a specific screen**: use `target.screenId` to direct changes to the right screen (uses the screen ID from operations, not the component ID).

**One run at a time**: only one active run is allowed per project. If you get `409 CONFLICT`, wait for the current run to complete before sending the next message. If the user changed their mind or a stale run is blocking the project, cancel it (see [Cancel Run](#chat-cancel-run)). Messages to different projects can run in parallel; use async polling (not `?wait=true`) when running multiple projects concurrently.

**Safe retries**: add an `idempotency-key` header (≤255 chars) to replay-safe re-sends. The server returns the existing run rather than creating a duplicate.

### 3. Show the results

After every chat run that produces `screen_created` or `screen_updated` operations, **take screenshots and show them to the user** using `POST /api/v1/screenshots`. The step is done only when the user has seen a screenshot of every screen the run created or updated; never complete a run silently.

- **New screens**: one screenshot per screen + one combined screenshot of all screens in the project.
- **Updated screens**: one screenshot per affected screen.

Use `background: "transparent"` unless the user explicitly requests a specific background color.

Save screenshots in the project directory (not a temporary folder) so the user can easily view them.

---

## Implementing Designs

When the user wants to implement the designs in code (not just preview them), **always fetch the component HTML code**. Do not rely on screenshots alone.

Use `GET /api/v1/projects/:id/components/:componentId` to fetch each screen's code. The `componentId` comes from the chat run's `result.operations`.

Component code can be large. When saving it to files, avoid writing the content through your text output: it's slow and wastes tokens. Instead, use shell commands to fetch the API response and write it directly to disk (e.g., pipe the response body into a file).

### Which version to use

Each component carries a `versions[]` array and an `activeVersion: number`. **By default, use the entry where `versions[i].version === activeVersion`**: that's the code currently shown in Sleek.

If the user's prompt pins specific versions, follow those instead (see [Pinned versions](#pinned-versions) below).

### Pinned versions

The user's prompt may include a pin block telling you to implement specific historical versions instead of the current ones, like this:

```
... at this exact state instead of the project's current version:
- component cmp_abc: version ver_001
- component cmp_def: version ver_002
- theme thm_ghi: version ver_003
```

When you see a pin block, implement those exact versions instead of `activeVersion`. Components not named in the pin block continue to use their active version. Theme IDs surface only inside pin blocks; this skill exposes no separate endpoint to enumerate them.

#### Fetching the right code

For each pinned component, find the entry in `versions[]` where `versions[i].id` matches the given version id (e.g. `ver_001`) and use its `code`. Do **not** fall back to `activeVersion` for pinned components.

#### Screenshots of pinned versions

Pass `componentVersionOverrides` and `themeVersionOverrides` to `POST /api/v1/screenshots`:

```json
{
  "componentIds": ["cmp_abc"],
  "projectId": "proj_xyz",
  "componentVersionOverrides": { "cmp_abc": "ver_001" },
  "themeVersionOverrides": { "thm_ghi": "ver_003" }
}
```

Keys are component / theme public ids; values are the corresponding `versions[i].id`. Entities missing from a map fall back to their active version. Include the override maps whenever the prompt specified pinned versions.

### HTML prototypes

The component `code` is a complete HTML document. Save it directly to a `.html` file. No build step needed.

### Native frameworks (React Native, SwiftUI, etc.)

Use both the HTML code and the screenshots together:

- **HTML code** is the implementation reference: it contains the exact structure, layout, styling, colors, spacing, content, image URLs, and icon names.
- **Screenshots** are the visual target: use them to verify your implementation matches the intended look.

The HTML tells you _how_ to build it; the screenshot tells you _what_ it should look like.

#### Icons

Sleek uses [Iconify](https://iconify.design) icons in the format `prefix:name` (e.g., `solar:heart-bold`, `material-symbols:search-rounded`, `lucide:settings`). The most common sets are **Solar**, **Hugeicons**, **Material Symbols** and **MDI**.

**Use the exact icons from the HTML code**. Do not substitute with a different icon set. Matching icons is important for design fidelity.

When implementing icons:

1. **Check if the project already has an icon system** that supports the same sets Sleek uses (Solar, Hugeicons, Material Symbols, MDI). If so, use it. Note: `@expo/vector-icons` does **not** support these sets, so do not use it as a substitute.
2. **Otherwise, fetch the SVGs from the Iconify API and embed them in the code:**

   ```
   GET https://api.iconify.design/{prefix}/{name}.svg
   ```

   Example: `https://api.iconify.design/solar/heart-bold.svg`

   Collect all icon names from the HTML, fetch their SVGs, and save them as static assets or string constants in the codebase. For **React Native / Expo**, render them with `react-native-svg`'s `SvgXml` component, which works in Expo Go with no additional native dependencies.

#### Fonts

The HTML includes Google Fonts via `<link>` tags in the `<head>`. Use the same fonts and weights when implementing in a native framework. Extract the font family names and weights from the `<link>` tags.

#### Navigation

The designs may include navigation elements like tab bars and headers. Update the project's navigation styling and structure to match the designs. Don't just implement the screen content while leaving the default navigation untouched.

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

---

## Endpoints

### Projects

#### List projects

```http
GET /api/v1/projects?limit=50&offset=0
Authorization: Bearer $SLEEK_API_KEY
```

Response `200`:

```json
{
  "data": [
    {
      "id": "proj_abc",
      "name": "My App",
      "slug": "my-app",
      "createdAt": "2026-01-01T00:00:00Z",
      "updatedAt": "..."
    }
  ],
  "pagination": { "total": 12, "limit": 50, "offset": 0 }
}
```

#### Create project

```http
POST /api/v1/projects
Authorization: Bearer $SLEEK_API_KEY
Content-Type: application/json

{ "name": "My New App" }
```

Response `201`: same shape as a single project.

#### Get / Delete project

```http
GET    /api/v1/projects/:projectId
DELETE /api/v1/projects/:projectId   → 204 No Content
```

---

### Components

#### List components

```http
GET /api/v1/projects/:projectId/components?limit=50&offset=0
Authorization: Bearer $SLEEK_API_KEY
```

Both list and get accept an optional `inlineIcons` query param (default `false`). When omitted, icons render as `<iconify-icon>` web components and the HTML pulls in the Iconify script, so leave it off by default. Pass `?inlineIcons=true` only when the consumer needs self-contained SVGs in the HTML (for example, importing into tools that don't run scripts).

Response `200`:

```json
{
  "data": [
    {
      "id": "cmp_xyz",
      "name": "Hero Section",
      "activeVersion": 3,
      "versions": [
        {
          "id": "ver_001",
          "version": 1,
          "code": "<!DOCTYPE html>...</html>",
          "createdAt": "..."
        }
      ],
      "createdAt": "...",
      "updatedAt": "..."
    }
  ],
  "pagination": { "total": 5, "limit": 50, "offset": 0 }
}
```

#### Get component

Fetches a single component by ID. Use this when you need the code for a specific screen (e.g., after a chat run returns a `componentId` in its operations).

```http
GET /api/v1/projects/:projectId/components/:componentId
Authorization: Bearer $SLEEK_API_KEY
```

Response `200`: `{ "data": ... }` with a single component in the same shape as a list item.

---

### References

References are curated design styles from featured Sleek projects. They are world-readable: any valid API key can list them, no scope needed.

```http
GET /api/v1/references?limit=50&offset=0
Authorization: Bearer $SLEEK_API_KEY
```

Response `200`:

```json
{
  "data": [
    {
      "id": "proj_ref1",
      "name": "Ember Fitness",
      "previewImageUrls": ["https://.../screenshot.png"]
    }
  ],
  "pagination": { "total": 44, "limit": 50, "offset": 0 }
}
```

To use one, pass its `id` as `referenceId` on [Send Message](#chat-send-message).

---

### Chat: Send Message

This is the core action: describe what you want in `message.text` and the AI creates or modifies screens.

```http
POST /api/v1/projects/:projectId/chat/messages?wait=false
Authorization: Bearer $SLEEK_API_KEY
Content-Type: application/json
idempotency-key: <optional, max 255 chars>

{
  "message": { "text": "Add a pricing section with three tiers" },
  "source": "claude-code",
  "imageUrls": ["https://example.com/ref.png"],
  "target": { "screenId": "scr_abc" },
  "referenceId": "proj_ref1"
}
```

| Field                    | Required | Notes                                                                                    |
| ------------------------ | -------- | ---------------------------------------------------------------------------------------- |
| `message.text`           | Yes      | 1+ chars, trimmed                                                                        |
| `source`                 | Yes      | Slug of the tool sending the request (see [step 2 of Designing](#2-send-a-chat-message)) |
| `imageUrls`              | No       | HTTPS URLs only; included as visual context                                              |
| `target.screenId`        | No       | Edit a specific screen using its `screenId` (not `componentId`); omit to let AI decide   |
| `referenceId`            | No       | Seed the design style from a reference (see [References](#references)); invalid id → `400` |
| `?wait=true/false`       | No       | Sync wait mode (default: false)                                                          |
| `idempotency-key` header | No       | Replay-safe re-sends                                                                     |

#### Response: async (default, `wait=false`)

Status `202 Accepted`. `result` and `error` are absent until the run reaches a terminal state.

```json
{
  "data": {
    "runId": "run_111",
    "status": "queued",
    "statusUrl": "/api/v1/projects/proj_abc/chat/runs/run_111"
  }
}
```

#### Response: sync (`wait=true`)

Blocks up to **300 seconds**. Returns `200` when completed, `202` if timed out.

```json
{
  "data": {
    "runId": "run_111",
    "status": "completed",
    "statusUrl": "...",
    "result": {
      "assistantText": "I added a pricing section with...",
      "operations": [
        {
          "type": "screen_created",
          "screenId": "scr_xyz",
          "screenName": "Pricing",
          "componentId": "cmp_xyz"
        },
        {
          "type": "screen_updated",
          "screenId": "scr_abc",
          "componentId": "cmp_abc"
        },
        { "type": "theme_updated" }
      ]
    }
  }
}
```

---

### Chat: Poll Run Status

Use this after async send to check progress.

```http
GET /api/v1/projects/:projectId/chat/runs/:runId
Authorization: Bearer $SLEEK_API_KEY
```

The response has the same `data` shape as send message: `result` is present when `completed`, `error` when `failed`:

```json
{
  "data": {
    "runId": "run_111",
    "status": "failed",
    "statusUrl": "...",
    "error": { "code": "execution_failed", "message": "..." }
  }
}
```

**Run status lifecycle**: `queued` → `running` → `completed | failed`

---

### Chat: Cancel Run

```http
POST /api/v1/projects/:projectId/chat/runs/:runId/cancel
Authorization: Bearer $SLEEK_API_KEY
```

Marks a `queued` or `running` run as `failed` with error code `cancelled` and returns the updated run; already-finished runs are returned unchanged. Use it when the user changes their mind mid-run or a stale run is blocking the project with `409 CONFLICT`.

---

### Screenshots

Takes a snapshot of one or more rendered components.

```http
POST /api/v1/screenshots
Authorization: Bearer $SLEEK_API_KEY
Content-Type: application/json

{
  "componentIds": ["cmp_xyz", "cmp_abc"],
  "projectId": "proj_abc",
  "format": "png",
  "scale": 2,
  "gap": 40,
  "padding": 40,
  "background": "transparent"
}
```

| Field                       | Default       | Notes                                                                                                                                      |
| --------------------------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `format`                    | `png`         | `png` or `webp`                                                                                                                            |
| `scale`                     | `2`           | 1–3 (device pixel ratio)                                                                                                                   |
| `gap`                       | `40`          | Pixels between components                                                                                                                  |
| `padding`                   | `40`          | Uniform padding on all sides                                                                                                               |
| `paddingX`                  | _(optional)_  | Horizontal padding; overrides `padding` for left/right when provided                                                                       |
| `paddingY`                  | _(optional)_  | Vertical padding; overrides `padding` for top/bottom when provided                                                                         |
| `paddingTop`                | _(optional)_  | Top padding; overrides `paddingY` when provided                                                                                            |
| `paddingRight`              | _(optional)_  | Right padding; overrides `paddingX` when provided                                                                                          |
| `paddingBottom`             | _(optional)_  | Bottom padding; overrides `paddingY` when provided                                                                                         |
| `paddingLeft`               | _(optional)_  | Left padding; overrides `paddingX` when provided                                                                                           |
| `background`                | `transparent` | Any CSS color (hex, named, `transparent`)                                                                                                  |
| `showDots`                  | `false`       | Overlay a subtle dot grid on the background                                                                                                |
| `radius`                    | `48`          | Squircle corner radius per component in pixels (integer ≥ 0); pass `0` for sharp corners                                                   |
| `componentVersionOverrides` | _(optional)_  | Map of `componentId` → `versions[i].id` to render at a pinned version instead of `activeVersion` (see [Pinned versions](#pinned-versions)) |
| `themeVersionOverrides`     | _(optional)_  | Map of `themeId` → `versions[i].id` to render with a pinned theme version (see [Pinned versions](#pinned-versions))                        |

Padding resolves with a cascade: per-side → axis → uniform. For example, `paddingTop` falls back to `paddingY`, which falls back to `padding`. So `{ "padding": 20, "paddingX": 10, "paddingLeft": 5 }` gives top/bottom 20px, right 10px, left 5px.

When `showDots` is `true`, a dot pattern is drawn over the background color. The dots automatically adapt to the background: dark backgrounds get light dots, light backgrounds get dark dots. This has no effect when `background` is `"transparent"`.

Response: raw binary `image/png` or `image/webp` with `Content-Disposition: attachment`.

---

## Error Shapes

```json
{ "code": "UNAUTHORIZED", "message": "..." }
```

| HTTP | Code                    | When                                   |
| ---- | ----------------------- | -------------------------------------- |
| 401  | `UNAUTHORIZED`          | Missing/invalid/expired API key        |
| 403  | `FORBIDDEN`             | Valid key, wrong scope or plan         |
| 404  | `NOT_FOUND`             | Resource doesn't exist                 |
| 400  | `BAD_REQUEST`           | Validation failure                     |
| 409  | `CONFLICT`              | Another run is active for this project |
| 500  | `INTERNAL_SERVER_ERROR` | Server error                           |

Chat run-level errors (inside `data.error`):

| Code               | Meaning                               |
| ------------------ | ------------------------------------- |
| `out_of_credits`   | Organization has no credits left      |
| `execution_failed` | AI execution error                    |
| `cancelled`        | Run cancelled via the cancel endpoint |

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
| Using `screenId` as `componentIds` in screenshots                       | `screenId` and `componentId` are different; always use `componentId` from operations for screenshots |
| Confusing `versions[i].version` (number) with `versions[i].id` (string) | When resolving pinned versions, match by `id` (e.g. `ver_001`); `version` is the numeric index       |
