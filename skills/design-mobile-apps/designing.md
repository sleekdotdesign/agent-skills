# Designing

The full request/response shapes for every endpoint used below are in the [API reference](endpoints.md).

## 1. Create a project

Create a project with `POST /api/v1/projects` if one doesn't exist yet. Derive a name from the request.

Each project has its own theme, style, and design system. If the user wants multiple design variations, create a separate project for each variation.

## 2. Send a chat message

Send the request with `POST /api/v1/projects/:id/chat/messages`. Sleek plans screen content and layout from your message, and will invent a visual style if you don't give it one. Don't decompose the request into screens and don't add product details the user didn't ask for; send the full intent as a single message. If the user described specific screens, include those. Sleek produces richer designs when given room to plan.

**Author a style direction**: write one whenever the user has given you anything to ground it in — reference images, apps they like, vibe adjectives, things to avoid — or whenever you're producing variations, one direction per variation. Pass the request through unchanged only when it's bare. A style direction is a single comprehensive paragraph, included in the message, covering mood (2–3 adjectives), color strategy (the logic, not hex codes), typography feel, layout philosophy, component style (radii, borders vs shadows, nav treatment), imagery and illustration style, and one or two distinctive details. Commit to a palette, a type direction, and an overall feel — anything that only sets a mood reads as a hint, not a direction. Be opinionated; don't hedge. Put the personality in color, type, and imagery rather than in unusual layout or navigation.

Extend what the user gave you and never contradict it. When they point at reference images or apps they like, study each one and carry what you take into the direction — Sleek only sees images passed as `imageUrls`, so for anything local the direction is how those references reach it. Borrow patterns, never the source's branding, content, or name.

Use a style direction or a `referenceId`, not both — a reference already carries a full style guide of its own.

**Seed a style with a reference**: Sleek curates a catalog of design references. When the user wants a specific look or asks for style options, list them with `GET /api/v1/references` (each has a `name` and `previewImageUrls` you can show) and pass the chosen id as `referenceId` on the first message to a project, so its style guide seeds the whole design.

**Identify your tool**: always send `source`, the slug of the tool making the request. The Sleek editor uses it to show the user who is designing while the run streams. Recognized values: `claude-code`, `claude`, `codex`, `chatgpt`, `cursor`, `openclaw`, `grok`. If your tool isn't listed, send a short kebab-case slug for it anyway (max 64 chars). Unrecognized values are fine and get a generic label.

**Watch it live**: runs render in the Sleek editor in real time. After sending the first message to a project, tell the user they can watch their screens being designed live in Sleek, and share the editor link: `https://sleek.design/project/:projectId`. Don't open a browser yourself unless the user asks.

**Polling**: chat messages are async by default: you get a `runId` and poll `GET /api/v1/projects/:id/chat/runs/:runId`. Start at 2s interval, back off to 5s after 10s, give up after 5 minutes. Exit on `completed` or `failed`; if you can't read the status, stop and report it rather than counting it as "not done yet". You can also use `?wait=true` for a blocking call (up to 300s; falls back to polling if it times out with `202`).

**Editing a specific screen**: use `target.screenId` to direct changes to the right screen (uses the screen ID from operations, not the component ID).

**One run at a time**: only one active run is allowed per project. If you get `409 CONFLICT`, wait for the current run to complete before sending the next message. If the user changed their mind or a stale run is blocking the project, cancel it (see [Cancel Run](endpoints.md#chat-cancel-run)). Messages to different projects can run in parallel; use async polling (not `?wait=true`) when running multiple projects concurrently.

**Safe retries**: add an `idempotency-key` header (≤255 chars) to replay-safe re-sends. The server returns the existing run rather than creating a duplicate.

## 3. Show the results

After every chat run that produces `screen_created` or `screen_updated` operations, **take screenshots and show them to the user** using `POST /api/v1/screenshots`. The step is done only when the user has seen a screenshot of every screen the run created or updated; never complete a run silently.

- **New screens**: one screenshot per screen + one combined screenshot of all screens in the project.
- **Updated screens**: one screenshot per affected screen.

Use `background: "transparent"` unless the user explicitly requests a specific background color.

Save screenshots in the project directory (not a temporary folder) so the user can easily view them.

**Showing vs reviewing**: the defaults capture only the viewport, which is the right framing for the user — screens look like phone screens. They are the wrong framing for judging your own work, because everything below the fold is cropped away. When you're reviewing what a run produced, re-shoot the screen with `fullHeight: true` (one screen per request) to see the whole scrollable page.

Screenshot requests are independent, so issue them in parallel — the user-facing shot and your `fullHeight` review shot go out together, as do the shots for different screens. "One screen per request" governs what goes into each image, not how fast you send them; it is not a reason to wait for one response before starting the next. Back off only if you actually get a `429`.

**Never call a screen incomplete from a viewport screenshot.** Content that looks missing is almost always just below the fold. Before telling the user something is absent, or sending a follow-up message asking Sleek to add it, confirm it against the whole screen: a `fullHeight: true` screenshot, or the component HTML from `GET /api/v1/projects/:id/components/:componentId`, which is the ground truth for what's on the screen. The screenshot is the default and answers most review questions on its own — don't go to the code to double-check something it already shows. Reach for the code only when you're about to claim something is missing: a render can omit what's really there (past the height cap, in a collapsed section, on a later carousel slide), so a negative conclusion is the one worth a second source. Note the reverse too — an element present in the HTML may still not be visible to the user.
