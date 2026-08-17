# Endpoints

Full contract for every row in the quick-reference table in [SKILL.md](SKILL.md).

## Projects

### List projects

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

### Create project

```http
POST /api/v1/projects
Authorization: Bearer $SLEEK_API_KEY
Content-Type: application/json

{ "name": "My New App" }
```

Response `201`: same shape as a single project.

### Get / Delete project

```http
GET    /api/v1/projects/:projectId
DELETE /api/v1/projects/:projectId   → 204 No Content
```

---

## Components

### List components

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

### Get component

Fetches a single component by ID. Use this when you need the code for a specific screen (e.g., after a chat run returns a `componentId` in its operations).

```http
GET /api/v1/projects/:projectId/components/:componentId
Authorization: Bearer $SLEEK_API_KEY
```

Response `200`: `{ "data": ... }` with a single component in the same shape as a list item.

---

## References

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

## Chat: Send Message

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
| `source`                 | Treat as required | Slug of the tool sending the request (see [step 2 of Designing](designing.md#2-send-a-chat-message)) |
| `imageUrls`              | No       | HTTPS URLs only; included as visual context                                              |
| `target.screenId`        | No       | Edit a specific screen using its `screenId` (not `componentId`); omit to let AI decide   |
| `referenceId`            | No       | Seed the design style from a reference (see [References](#references)); invalid id → `400` |
| `?wait=true/false`       | No       | Sync wait mode (default: false)                                                          |
| `idempotency-key` header | No       | Replay-safe re-sends                                                                     |

### Response: async (default, `wait=false`)

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

### Response: sync (`wait=true`)

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

## Chat: Poll Run Status

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

## Chat: Cancel Run

```http
POST /api/v1/projects/:projectId/chat/runs/:runId/cancel
Authorization: Bearer $SLEEK_API_KEY
```

Marks a `queued` or `running` run as `failed` with error code `cancelled` and returns the updated run; already-finished runs are returned unchanged. Use it when the user changes their mind mid-run or a stale run is blocking the project with `409 CONFLICT`.

---

## Screenshots

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
| `fullHeight`                | `false`       | Capture the entire scrollable screen instead of just the viewport (see below)                                                              |
| `radius`                    | `48`          | Squircle corner radius per component in pixels (integer ≥ 0); pass `0` for sharp corners                                                   |
| `componentVersionOverrides` | _(optional)_  | Map of `componentId` → `versions[i].id` to render at a pinned version instead of `activeVersion` (see [Pinned versions](implementing.md#pinned-versions)) |
| `themeVersionOverrides`     | _(optional)_  | Map of `themeId` → `versions[i].id` to render with a pinned theme version (see [Pinned versions](implementing.md#pinned-versions))                        |

Padding resolves with a cascade: per-side → axis → uniform. For example, `paddingTop` falls back to `paddingY`, which falls back to `padding`. So `{ "padding": 20, "paddingX": 10, "paddingLeft": 5 }` gives top/bottom 20px, right 10px, left 5px.

By default a component is captured at frame height, so anything the user would reach by scrolling is cut off. `fullHeight: true` expands each frame to the height of its own content before capturing. Use it when you're reviewing your own work; leave it off for the screenshots you show the user, where the phone-shaped framing is the point.

Frames are capped at **4× the default frame height**, so a screen longer than that is still cut off at the bottom even with `fullHeight: true`. On a very long screen, treat the component HTML as the authority for what's below the cap. Expanded frames make for tall images; prefer one component per request so each screen keeps its detail — and send those requests in parallel rather than one after another.

When `showDots` is `true`, a dot pattern is drawn over the background color. The dots automatically adapt to the background: dark backgrounds get light dots, light backgrounds get dark dots. This has no effect when `background` is `"transparent"`.

Response: raw binary `image/png` or `image/webp` with `Content-Disposition: attachment`.
