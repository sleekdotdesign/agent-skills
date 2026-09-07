---
name: sleek-mobile-app-prd
description: Use when the user has a mobile app idea and needs to turn it into a concrete, buildable plan: an MVP feature cut, the user flows, and a screen-by-screen inventory ready to hand to design. Covers sharpening the idea into a one-liner, naming the core loop and activation moment, in/out/later scoping with reasons, choosing the navigation structure, per-screen specs with empty and error states, and a final design brief. Handles broad requests ("I want to build a habit tracker, what do I need", "turn my idea into a plan") and specific ones ("write a PRD for my app", "what screens does my app need", "cut this feature list down to an MVP", "map the flows for my app"). Not for visual design or building the screens themselves, detailed first-run onboarding flow and copy, App Store listing or ASO, pricing strategy, or engineering decisions like tech stack, data models, and API design.
---

# From App Idea to Screen Map

A screen map is the bridge between a one-line app idea and a designed app. It names the core loop, cuts the idea down to a first version you can actually ship, traces the main user flows, and lists every screen with what goes on it, so the next step, designing those screens, starts from something concrete. Done well it turns "I want to build a habit tracker" into a short, ordered list of screens that a person or a design tool can pick up and build from.

> **Scope.** This skill turns an idea into a plan: the one-line description, the core loop, the MVP cut, the user flows, the navigation structure, and a screen inventory with what belongs on each screen, ending in a design brief.
>
> Some things sit next to this and belong elsewhere:
> - **Visual design, UI systems, and building the screens.** This skill decides what screens exist and what goes on them, not how they look or how they are built.
> - **The detailed first-run onboarding flow and its copy.** The map reserves an onboarding slot and says roughly how many screens it needs; it does not design the sequence or write the words.
> - **App Store listing and ASO.** That is distribution, a separate phase after the app exists.
> - **Pricing and the monetization model.** The map reserves a paywall slot where one belongs; what to charge and how to package it is separate.
> - **Engineering: tech stack, data models, backend, and API design.** The map lists the app's main objects so screens have something to show, but it stops short of schemas and architecture.
> - **Deep market research and validation.** A short, time-boxed competitive scan is in scope as an optional step; validating the business is not.
>
> Default to mobile apps (iOS and Android). The approach carries over to other platforms, but the navigation and platform notes are written for mobile.

---

## Overview: what a good screen map optimizes for

The goal is a plan that is small enough to build and complete enough to design from. A few principles get you there.

- **The loop comes first.** An app is a short cycle the user repeats, not a pile of features. Name that loop before anything else, because every other decision serves it.
- **Flows before screens.** Screens fall out of flows. List screens first and you will forget half of them; walk the flows and the screens name themselves.
- **One job per screen.** A screen with a single clear purpose is easy to design, easy to test, and easy to move. Each screen has one designated primary action; secondary shortcuts are fine, but two co-equal primary actions competing for the same screen usually means it should be two.
- **States are screens too.** Empty, loading, error, and offline are not edge cases. They are most of what a new user sees, and skipping them is why apps feel broken on day one.
- **Every cut needs a reason.** An MVP is defined by what you leave out. "Later" is a decision you record, not a feature you forget.

### The main tension: cut deep, but not below the floor

The instinct with an MVP is to cut hard, and that instinct is right up to a point. The counterweight is that an app still has to do its job. Apple rejects apps that are too limited to be useful (App Store Review Guideline 4.2, minimum functionality): an app should be "useful, unique, or app-like," not a thin shell. So cut to the core loop, the point where the app can complete its cycle once and deliver the first real value, and not below it. Everything the loop needs stays in. Everything else is Out or Later.

---

## The artifact

The output of this skill is a screen map with these parts, in this order:

1. **One-liner.** What the app is, who it is for, and the wedge, in one sentence.
2. **Core loop and aha moment.** The cycle the user repeats, and the first time it pays off.
3. **MVP scope.** In, Out, and Later, with a reason on every item.
4. **User flows.** The happy path first, then the key alternates, as numbered steps.
5. **Navigation shell.** The top-level structure and what lives in it.
6. **Screen inventory.** Every screen in a table, plus a full spec for each data screen.
7. **The app's objects.** A plain list of the main things the app is about, the nouns its screens are views over.
8. **Design brief.** One paragraph that hands the whole thing to the design step.
9. **Open questions.** Assumptions and contested calls to resolve before building.

The inventory has two layers. Every screen gets a **table row** with its name, purpose, and primary action, so the whole app is visible at a glance. Every **data screen** additionally gets a full spec, because those are the screens design has to get right:

- **Purpose.** One line: the job of this screen.
- **Primary action.** The one thing the user came here to do.
- **Key elements.** The main content and controls on the screen.
- **States.** Empty, loading, error, and offline, wherever they apply.
- **Navigates to.** Where the primary and secondary actions lead.

A **data screen** is any screen whose main content is loaded or made of user records: a list, a feed, a detail view, a dashboard. Composers, settings, permission prompts, and static or informational screens are not data screens; their table row is enough.

> Every example below uses one made-up app, **Reps**, a workout tracker for people who lift at home or the gym without a coach. Reps' core loop is pick a workout, log your sets, see your progress. Its aha moment is the first logged workout showing up in history. ❌ marks a bad example, ✅ a good one.

---

## Best practices per stage

### Sharpen the idea

- Turn the one line into a specific one-liner with a shape: **who** it is for, **what** they do with it, and the **wedge** that makes it this app and not a generic one. A useful template is "[App] helps [who] [do what] [context or wedge]."
- If the idea arrives as a single vague line and the user is present, ask up to a handful of sharpening questions before mapping: who exactly is this for, what is the one thing they do with it, which platform, and is it free or paid. Then map. If the user is not around to answer, make reasonable assumptions and label them as assumptions in the open questions.
- Do not invent scope while sharpening. The point is to name the idea precisely, not to grow it.

**Examples**
- ❌ "An app for fitness people." It names a market, not a product, and you cannot map screens from it.
- ✅ "Reps helps people who lift at home or the gym log their workouts and watch their strength climb, without a coach." You can see the user, the job, and the wedge.

### Core loop and aha moment

- Name the **core loop**: the short cycle the user repeats every time they open the app. For Reps it is open, pick or start a workout, log the sets, see the numbers move. That cycle is the app.
- Name the **aha moment**: the first time the loop pays off, when a new user sees real value. For Reps it is the first workout recorded and visible in history.
- Use both as a filter. In the MVP, a feature either is part of the loop or it directly helps a new user reach the aha the first time. If it does neither, it is not in the first version.

**Examples**
- ❌ "Reps is a fitness app with workouts, a social feed, challenges, and AI coaching." Four directions, no loop.
- ✅ "Loop: pick a workout, log your sets, watch your numbers climb. Aha: your first workout recorded in history." One loop, one payoff, everything else measured against it.

### The MVP cut

- Sort every feature into **In**, **Out**, or **Later**, and write a reason next to each one. In means the core loop needs it to run once, well. Out means it is not this product. Later means it is real but comes after launch.
- Watch for the **"v2 disguised as v1"** feature: the thing that sounds essential but the loop runs fine without. Move it to Later. Social, gamification, and AI extras are the usual culprits.
- Respect the **floor**. Do not cut below a working loop. An app that cannot complete its loop once is not a lean MVP, it is a rejected app (Apple Guideline 4.2, minimum functionality). Cut to the loop, not through it.
- **Heuristic, not a rule:** if the first-version inventory runs much past fifteen screens, the cut is probably too generous somewhere. Count the screens in the inventory table, not their empty and error states or the reserved onboarding and paywall slots. Treat a high count as a smell to investigate, not a hard limit.

**Example (Reps)**
- **In:** log a workout (the loop); an exercise library to log against (the loop needs something to log); history and a simple progress view (the aha). 
- **Out:** a social feed (not the wedge, Reps is a solo tracker); AI-generated coaching (a different, bigger product).
- **Later:** saved workout templates; sync to a health platform; a watch app. All real, none needed to run the loop on day one.

### Map the flows

- Write the **happy path** first, as numbered steps from launch to the aha and through one full loop. This is the spine of the app.
- Then map the few key alternates that change what the user sees: **first run** with no data, the **returning user**, and the **main error** (usually no connection or a failed load).
- Keep flows as short numbered prose, not diagrams. Prose survives being pasted anywhere and does not rot when a screen changes.
- Every step names a screen and an action. That is how you surface screens you would otherwise forget, the app is only real once the steps connect end to end.

**Example (Reps, happy path)**
1. Launch to Home, which shows today's suggested workout.
2. Tap Start to open the workout screen with its list of exercises.
3. Log sets for the first exercise (reps and weight).
4. Finish the workout; Reps saves it and shows a short summary.
5. Land on History with the just-logged workout at the top. That first entry is the aha.

### Choose the navigation shell

Pick the top-level structure once the flows are known, not before. The flows tell you which sections the user returns to, and those are your navigation.

- **Common shells:** a **tab bar** for a few co-equal sections the user moves between; a **single stack** for a linear or single-purpose app; a **home base with pushed detail screens** for a content app; **modals** for short self-contained subtasks layered over any of these.
- **Match sections to the loop.** A section earns a tab only if the user comes back to it. If a "section" is visited once or reached from somewhere else, it is a pushed screen, not a tab.
- **Respect platform limits (verified).** On iPhone, use three to five tabs; go over and the system collapses the extras into a "More" list that buries them, so keep top-level sections to five or fewer (Apple Human Interface Guidelines). On Android, a navigation bar holds three to five destinations; for fewer use tabs, for more do not use a navigation bar (Material Design 3). Use modality sparingly, for a short task the user finishes or cancels, not as a second layer of navigation (Apple Human Interface Guidelines).

**Examples**
- ❌ Reps with five tabs: Home, History, Social, Store, Settings. Two of them (Social, Store) are not in the MVP, and Settings is not a place users return to.
- ✅ Reps with three tabs mapped to the loop: **Workout**, **History**, **Profile**. Settings is pushed from Profile, not a tab of its own.

### Inventory the screens

- Walk every flow and list each screen once in a table with its purpose and primary action, so the whole app is visible on one page. Then write the full spec (purpose, primary action, key elements, states, navigates to) for each data screen, the ones that load or show records.
- Include the unglamorous screens that flows tend to skip but every app needs: sign-in and sign-up (only if a feature requires an account), settings, profile or account, permission prompts, and legal (privacy policy, terms). Use the archetype reference below as a checklist so none go missing.
- Hold the line on **one job per screen**. If a screen has two co-equal primary actions, split it or demote one to a secondary shortcut.

**Example (Reps inventory, abbreviated)**

| Screen | Purpose | Primary action |
| ------ | ------- | -------------- |
| Home / Workout | Start today's session | Start workout |
| Workout detail | Log sets for each exercise | Log a set |
| Workout summary | Confirm the session was saved | Done |
| History | See past workouts | Open a workout |
| Progress | See strength over time | (view) |
| Exercise library | Browse and pick exercises | Add exercise |
| Profile | Account and stats | (view) |
| Settings | Preferences, sign out, legal | Change a setting |
| Sign-in / sign-up | Save progress across devices (deferred) | Continue |

One data screen, fully specced, as a model for the rest (each data screen in the table gets one of these):

> **History.** Purpose: let the user see and revisit past workouts. Primary action: open a workout for detail. Key elements: reverse-chronological list of sessions with date, name, and a headline number (total volume). States: empty ("Your workouts show up here. Log your first to start your streak." plus a Log a workout button); loading (skeleton rows); error ("Couldn't load your history" plus Retry); offline (show the last cached list, flag it as offline). Navigates to: a workout detail on tap; the Workout tab from the empty state's button.

### States pass

- Go back over every screen that shows data and specify its **empty, loading, error, and offline** states. This is a deliberate second pass because these states are easy to skip while listing screens and are exactly where a new user starts.
- Treat every **empty state as an onboarding surface**: say what belongs here and give the one action that fills it. An empty screen that only says "No data" wastes the most important moment.

**Examples**
- ❌ "History screen: shows past workouts." No empty state, no error, so the first-run and no-connection experiences are undefined.
- ✅ "History. Empty: 'Your workouts show up here, log your first to start.' plus a Log a workout button. Loading: skeleton rows. Error: 'Couldn't load your history' plus Retry. Offline: last cached list, marked offline."

### List the app's objects

- Name the handful of **objects** the app is about: the nouns its screens are views over. This is a plain list, usually three to seven items, that keeps the screens honest, every data screen should show or edit one of these.
- Stop at the nouns. Do not add fields, types, relationships, or schemas; that is engineering, and it comes later.

**Example (Reps)**
- Workout (a logged session), Exercise (an item in the library), Set (reps and weight within a workout), User (the person and their stats). The History and Progress screens are views over Workouts; the library is a view over Exercises.

### Write the design brief

- End with **one tight paragraph** that states the app, the intended feel (if the user gave one), the platform, and the full screen list. This is the handoff. It should read as something a designer or a design tool can start from without a meeting.
- **Keep it whole.** Describe the app as one brief and let the design step plan the screens. Do not chop the app into one instruction per screen, that throws away the coherence the map just built and tends to produce a disconnected set of screens.
- Fold in the contested calls and open questions so whoever designs next knows what was assumed and what is still open.

**Example (Reps design brief)**
> Reps, a workout tracker for people who lift at home or the gym without a coach, on iOS and Android. Clean and focused, numbers front and center, gets out of the way during a workout. Three tabs: Workout, History, Profile. Screens: Home with today's session, workout detail for logging sets, workout summary, History list, a Progress view, an exercise library, Profile, Settings, and a deferred sign-in offered only after the first workout is logged. Every data screen has an empty, loading, error, and offline state. Open: whether Progress ships in v1 or moves to Later; paywall placement is not decided here.

---

## Screen archetype reference

Most mobile screens are one of a handful of types. Use this as a checklist while inventorying: for each archetype the app needs, add the screen and its commonly forgotten parts.

| Archetype | Its job | Key elements | Commonly forgotten |
| --------- | ------- | ------------ | ------------------ |
| **List / feed** | Show many items to pick from | Rows or cards, sort or filter, item tap target | Empty, loading, error, and end-of-list states |
| **Detail** | Show and act on one item | Item content, primary action, related actions | Not-found or deleted-item state |
| **Composer / editor** | Create or edit an item | Inputs, save, cancel | Validation, unsaved-changes warning, save error |
| **Dashboard / home** | Orient and launch the core loop | Primary call to action, a few key numbers or shortcuts | First-run empty state before any data exists |
| **Search** | Find something specific | Query field, results, recents | No-results state, loading, cleared state |
| **Auth (sign-in / sign-up)** | Get the user into an account | Email or social login, forgot-password | A privacy-focused login option; skip or guest path |
| **Profile / account** | Show and edit the user's own data | Identity, stats, entry to settings | Signed-out vs signed-in versions |
| **Settings** | Change preferences and account | Grouped options, sign out, legal links | Legal (privacy, terms), account deletion entry |
| **Permission prompt** | Ask for a system permission in context | The value-first reason, allow and not-now | The denied path and how the app behaves without it |
| **Onboarding (slot)** | Get a new user to the aha (designed separately) | Reserve three to five screens | That this is a placeholder, not designed here |
| **Paywall (slot)** | Present the offer (priced separately) | Reserve its place in the flow | That pricing and copy are decided elsewhere |
| **Empty / error / offline** | Cover the states of every data screen | What goes here, the one action, retry | That these are required, not optional extras |

---

## Workflow: turning an idea into a screen map

Work in this order. Each step builds on the one before.

1. **Sharpen the idea** into a specific one-liner. If it arrives as a single vague line and the user is present, ask up to a handful of clarifying questions first; otherwise make assumptions and label them.
2. **Name the core loop and the aha moment.** Everything downstream is measured against these.
3. **Make the MVP cut.** Sort every feature into In, Out, or Later with a reason, then check the result against the floor: can the app still complete its loop once, and is the aha moment still reachable with only the In features?
4. **Map the flows.** Happy path first, then first-run, returning-user, and the main error path.
5. **Choose the navigation shell** from the flows, within platform limits.
6. **Inventory the screens.** Table every screen, then write the full spec for each data screen, using the archetype reference as a checklist so the boring-but-required screens are not missed.
7. **Run the states pass** over every data screen: empty, loading, error, offline.
8. **List the app's objects,** the three to seven nouns the screens are views over.
9. **Optionally, time-box a competitive scan.** Look at a few top apps in the category for screens or patterns you missed. Add what is genuinely missing; do not let it reopen the MVP cut.
10. **Write the design brief** and list the open questions and contested calls.
11. **Run the Self-check.**

---

## Self-check

Run the drafted map through this before calling it done. It turns the principles above into a pass or fail list.

- [ ] The app has a **one-line description**: what it is, who it is for, and the wedge.
- [ ] The **core loop** is named as a short, repeatable cycle.
- [ ] The **aha moment** is named, and the MVP still reaches it using only the In features.
- [ ] Every feature is sorted **In, Out, or Later,** and every item has a reason.
- [ ] The MVP still **completes its core loop at least once** (it clears the minimum-functionality floor).
- [ ] The **happy-path flow** is written as numbered steps from launch to the aha.
- [ ] **First-run, returning-user, and main-error** flows are covered.
- [ ] The **navigation shell** is chosen from the flows and fits platform limits (iPhone three to five tabs; Android navigation bar three to five destinations).
- [ ] **Every screen has one job** and one designated primary action (secondary shortcuts are fine).
- [ ] **Every screen is reachable:** it appears in a flow or is navigated to from another screen.
- [ ] The **required-but-boring screens** are present where needed: auth, settings, profile, permissions, legal.
- [ ] **Every data screen** specifies empty, loading, error, and offline states.
- [ ] The **app's objects** are listed, and every data screen shows or edits one of them.
- [ ] The **design brief** states the whole app in one paragraph plus the screen list.
- [ ] **Assumptions and contested calls** are listed as open questions.

---

## Common mistakes

| Mistake | Fix |
| ------- | --- |
| Listing screens before mapping flows | Map the flows first; the screens fall out of them, and you forget fewer |
| A feature list with no cuts | Sort every feature In, Out, or Later with a reason; an MVP is defined by what is out |
| "v2 disguised as v1" | Move features the loop runs without to Later, especially social, gamification, and AI extras |
| Cutting below the loop | Keep enough to complete the loop once; below that it is a rejected, minimum-functionality app (Apple 4.2) |
| Screens with two co-equal primary actions | One job per screen; keep one primary action and demote the rest to secondary shortcuts, or split the screen |
| Forgetting empty, loading, and error states | Run a dedicated states pass over every data screen |
| Forgetting settings, auth, and legal screens | Use the archetype reference as a checklist |
| Too many top-level tabs | Three to five on iPhone; extra tabs hide behind a More list (Apple HIG). Map tabs to the loop, push the rest |
| A design brief chopped into per-screen instructions | State the whole app in one brief and let the design step plan the screens |
| Requiring sign-in before any value | Defer auth unless a feature needs an account; keep it off the critical path (Apple 5.1.1) |

---

## Platform notes

Navigation structures and account rules differ by platform and change over time. Verify against the current Apple and Android docs before relying on a specific rule.

### iOS

- **Top-level navigation.** A tab bar is for three to five top-level sections on iPhone. If you exceed five, the system shows a More tab and hides the rest behind it, which buries content, so keep the top level to five or fewer (Apple Human Interface Guidelines). Verify current.
- **Modality.** Use modals sparingly, for a short self-contained task the user must finish or cancel, not as a second layer of navigation. Keep them simple: in Apple's words, "don't create an app within your app" (Apple Human Interface Guidelines). Verify current.
- **Accounts.** Do not require sign-in unless the app has significant account-based features. If the map includes an account, the app must offer in-app account deletion, and it may not require personal information to function unless that information is directly relevant to the core feature or required by law (Apple App Store Review Guideline 5.1.1). Keep auth off the critical path unless the loop genuinely needs it. Verify current.
- **Minimum functionality.** A first version still has to be "useful, unique, or app-like"; too-thin apps are rejected (Guideline 4.2). This is the floor the MVP cut must clear. Verify current.

### Android

- **Top-level navigation.** A navigation bar holds three to five top-level destinations. For fewer than three, use tabs; for more than five, do not use a navigation bar (use tabs within a page, or a navigation drawer or rail) (Material Design 3). Verify current.
- **Back.** Android has a system back gesture in addition to in-app up navigation, so every pushed screen and modal needs sensible back behavior, and recent versions add predictive back (Material Design 3 and the Android platform). Verify current.
- **Parity.** One screen map usually serves both platforms. Where they diverge is the shell (a tab bar on iOS versus a navigation bar on Android) and back behavior. If the app ships on both, note both in the map.

---

## Notes & verification

- Platform navigation limits and account rules (tab counts, navigation bar destinations, modality, minimum functionality, login requirements) change over time. Check the current Apple Human Interface Guidelines, the Apple App Store Review Guidelines, and the Material Design 3 guidelines before relying on a specific number or rule.
- The contested calls in this skill are judgment calls, not laws: where to draw the In, Out, and Later lines; the roughly fifteen-screen smell test; whether an MVP needs auth at all; and whether to run a competitive scan. Treat them as defaults to adjust per app, and record which way you went in the open questions.
- Screen counts and feature lists here are examples, not targets. The right number of screens is the fewest that run the core loop well.
- This skill is guidance only. It needs no API key and no network access.
