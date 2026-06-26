---
name: sleek-mobile-app-onboarding
description: Use when the user wants to design or improve a mobile app's onboarding: the first-run flow and the copy in it. Covers flow structure (welcome and value screens, a personalization or survey step, permission priming, sign-up and paywall placement, the activation or "aha" moment, first-run empty states) and the copy inside it (value props, microcopy, button labels, permission rationale), plus reducing drop-off and time-to-value. Handles broad requests ("design onboarding for my app", "reduce onboarding drop-off") and specific ones ("write a notification priming screen", "where should the paywall go", "rewrite this welcome screen"). Not for App Store listing or ASO, visual design or screen building, long-term retention and re-engagement past the first session, or pricing strategy (this covers where the paywall goes and how to write it, not what to charge).
---

# Mobile App Onboarding

Onboarding is everything between the first launch and the moment a new user gets real value from your app. It shows what the app is for and gets the user to their first real win, without asking for more than the app needs along the way. Done well it raises activation and cuts the drop-off that happens in the first few minutes, when a new user is deciding whether the app is worth keeping.

> **Scope.** This skill is about the onboarding flow and its copy: the sequence of screens a new user sees and the words on them. It covers the screen types, what order they go in, permission priming, where sign-up and the paywall belong, the activation moment, and first-run empty states.
>
> Some things sit next to onboarding but belong elsewhere:
> - **App Store listing and ASO.** That is pre-install, a separate concern from the first-run flow this skill covers.
> - **Visual design, UI systems, and building the screens.** This skill is about the flow and the copy, not how the screens look or how they are built.
> - **Long-term retention, re-engagement, and lifecycle messaging past the first session.** That is a later phase with its own playbook.
> - **Acquisition, growth, and ad creative.** Also pre-install.
> - **Pricing strategy and the monetization model.** This skill covers where the paywall goes and how to write it, not what to charge.
>
> Default to mobile apps (iOS and Android). For web or SaaS onboarding the principles carry over, but ask the user before assuming the same patterns apply.

---

## Overview: what good onboarding optimizes for

The metric is activation, not screen count. Before designing anything, get clear on three numbers and define them for the specific app.

- **Activation**. The share of new users who reach the first real value, the "aha" moment. This is the number onboarding exists to move. Name the aha moment for the app first, because the whole flow is a path to it.
- **Time-to-value**. How long it takes to get from launch to that first win. Shorter is usually better, with one real exception below.
- **Completion**. The share who finish the flow instead of dropping out. Every extra screen, field, and tap is a place to lose people.

Screen count is a means, not a target. A short flow that never delivers value is worse than a longer one that does. Keep each screen focused on a single job so the flow stays easy to reorder and test.

### The main tension: friction vs investment

There are two schools here, and they pull in opposite directions. Pick one on purpose before you design.

- **Get to value fast.** Fewest screens, defer every ask you can, drop the user into the product quickly. This fits utilities, tools, social apps, anything where the value is obvious once the user is inside.
- **Build investment first.** A longer guided survey that personalizes a result and makes the user feel they have put work in, then converts to a subscription. This is the quiz-led pattern common in fitness, wellness, health, education, and horoscope apps. In those categories a longer onboarding can lift paywall conversion, because the user has invested effort and seen a tailored result before being asked to pay.

This choice is contested and depends on the business model. A free utility wrapped in a fifteen-screen quiz will bleed users. A subscription wellness app with a one-screen onboarding often converts worse than one that personalizes first. State which model applies, then design for it. The rest of this skill gives you both default sequences.

---

## Onboarding flow reference

The common screen types, what each one does, and where it tends to sit. Treat the order as a starting point, not a fixed template.

| Screen type | What it does | When to use it | Typical position |
| ----------- | ------------ | -------------- | ---------------- |
| **Welcome / value** | States the core value in a line and reassures the user the download was a good call | Almost always | First |
| **Value highlights** | A few benefit-led screens for value that needs more than one line | When the value is not obvious from a single screen | Early, skippable |
| **Personalization survey** | Tailors the app to the user and, in subscription apps, builds investment | When the answers visibly change the product | Early, after the value screens |
| **Social proof** | Authentic ratings or testimonials at a trust-sensitive moment | Near the paywall or another big ask | Repeated, especially before the paywall |
| **Processing / personalizing** | A short, honest "building your plan" beat | Only when the app actually assembles something | After the survey |
| **Permission priming** | A value-first screen shown before a system permission prompt | Before each sensitive permission the app needs | In context, per feature |
| **Sign-up / login** | Account creation | Only when a feature needs an account | As late as the app allows |
| **Paywall** | The subscription offer | Subscription apps | After the aha moment or the personalized result (contested) |
| **Activation / first action** | Guides the first real use, the aha moment | Always | The goal of the flow |
| **First-run empty states** | Tells a new user what goes on an empty screen and the one action to fill it | Always | Throughout the first session |

### Two default sequences

**Investment-style subscription app** (the quiz-led pattern: fitness, wellness, education):

Welcome value → short value highlights → survey, one input per screen → social proof → honest processing → notification priming → personalized result → paywall → activation → sign-up (here or deferred).

**Get-to-value-fast app** (utility, tool, social):

Welcome value → optional one or two value screens → straight to the first action (the aha) → permission priming only when a feature needs it → sign-up only when needed → paywall later or in context.

Both reach the same goal, activation. They differ in how much the user does before getting there. Choose the sequence that matches the model you picked in the Overview, then adjust.

---

## Best practices per screen type

> Every example below uses one made-up app, **Reps**, a home and gym workout tracker. Reps' aha moment is logging a first workout, the point where the user sees their effort recorded. ❌ marks a bad example, ✅ a good one.

### Welcome / value

- Open with the benefit in plain words, not a logo and a generic greeting. Give the user a reason to take the next step.
- One primary action on the screen. Do not split attention.
- Reaffirming the user's choice is fine. Congratulating someone for downloading before they have done anything tends to feel hollow, so lead with value rather than applause. (The "congratulations screen" is a tactic some practitioners push; treat it as optional.)

**Examples**
- ❌ A Reps logo over "Welcome to Reps!" with a "Get Started" button. It says nothing about what the app does for you.
- ✅ "Build a workout habit that sticks." Subtext: "Guided home and gym routines that track every set." Button: "Start". The value is stated before anything is asked.

### Value highlights

- Keep it to a few screens, benefit-led, and let people skip. This is not a manual.
- Show the real product behind each claim where you can, not abstract art.

**Examples**
- ❌ A six-screen carousel listing every feature in Reps, with no skip button.
- ✅ Three screens, each with one benefit over a real screen: "Train anywhere", "Watch your strength climb", "Never lose a set." Skip stays visible.

### Personalization survey

- One input per screen. A wall of fields reads as work and drives people out.
- Lead with low-friction questions and move the more personal ones later, once the user has some momentum.
- Ask only for answers that visibly change the product. If an answer does not alter what the user gets, cut the question.
- Give the reason for every question. A short line tying the ask to a benefit earns the answer. (Example pattern: "We need your birth date to set your horoscope" for an astrology app.)
- Framing questions around the user's own goal helps them connect their need to the app and gives you the data to personalize. Keep this honest. Do not manufacture anxiety to make the app look like the cure.

**Examples**
- ❌ One screen with six fields: age, height, weight, goal, experience, equipment. It looks like a form at the doctor's office.
- ✅ One question per screen. "What's your main goal?" [Build muscle / Lose fat / Stay consistent]. Then "Where do you train?" [Home / Gym / Both]. Each answer changes the plan Reps builds, and a line under the experience question reads "We use this to set your starting weights."

> **Name capture, flagged as optional.** Some flows collect the user's name early and use it throughout to feel personal. It works in coaching or companion-style apps, but it adds a field and trades against the friction rule, so only do it if the app's voice will actually use the name. For Reps it is optional.

### Social proof

- Use real ratings and real testimonials. Fabricated reviews read as fake and break trust, and inventing them violates app store rules.
- Place it at trust-sensitive moments, especially right before the paywall.

**Examples**
- ❌ Five gold stars with "Best app ever! - a happy user" invented for the screen.
- ✅ Reps' actual App Store rating and review count, plus one short real testimonial, shown on the screen just before the paywall.

### Processing / personalizing

- Use this only when the app genuinely assembles something. A brief "building your plan" beat sets expectations and makes the result feel made for the user.
- Do not fake the delay. A spinner engineered purely to look like hard work is manipulative and costs you trust the moment a user notices nothing was computed. This "processing illusion" is a contested tactic; the honest version is fine, the fake version is not.

**Examples**
- ❌ A ten-second "Analyzing your data..." bar when Reps computes nothing and the screen is on a timer.
- ✅ A short "Building your starting plan" while Reps actually assembles a routine from the survey answers, then shows the plan.

### Permission priming

- Explain the value before the system prompt fires. The OS prompt can usually be shown only once, so a prompt the user dismisses is often gone for good. A priming screen lets you ask only the people likely to say yes, and you can re-show your own screen later without burning the system prompt.
- Ask in context, right before the feature needs the permission. One permission at a time.
- Your priming screen must not imitate the system alert or use a button that implies it grants access. Platform specifics are in [Platform notes](#platform-notes).

**Examples**
- ❌ On first launch, Reps fires the notification, tracking, and health prompts back to back before the user has done anything.
- ✅ After the user sets a goal, Reps shows: "Want a nudge on workout days? We'll remind you only on the days you planned to train." [Maybe later] [Turn on reminders]. Tapping "Turn on reminders" then triggers the real system prompt.

### Sign-up / login placement

- Delay it. Let the user reach value first. Apple's rules say not to require an account unless a feature genuinely needs one (Guideline 5.1.1).
- Offer a guest path or skip where you can. If you offer a social login, you must also offer a privacy-focused option (Apple Guideline 4.8; Sign in with Apple meets this).

**Examples**
- ❌ A sign-up wall as the first screen, before Reps has shown a single workout.
- ✅ Let the user log their first workout, then prompt: "Create an account to save your progress across devices," with Sign in with Apple offered next to Google.

### Paywall placement

This is the most-tested and least-settled decision in the flow. Present the options, do not pretend one is the rule.

- **Hard paywall, before any value.** Can raise revenue per install for apps with a strong brand or already-proven value. It suppresses activation and is risky for an unknown app, since people pay before they have seen anything work.
- **Post-aha or post-result paywall.** Show value first, then make the offer. The safer default for most apps.
- Make the offer legible: what the user gets, the price, and the trial terms in plain language. (What to charge is out of scope; this is about placement and copy.)

**Examples**
- ❌ A paywall on launch with a pre-ticked annual plan, fine-print trial terms, and no clear statement of what Pro unlocks.
- ✅ After Reps shows the personalized plan: "Unlock your full plan. 7-day free trial, then $39.99/year. Cancel anytime," with three value bullets and the trial terms in one honest sentence.

> Whichever placement you pick, flag it as an A/B test, not a settled answer.

### Activation / first action

- This is the point of the whole flow. Get the user to the first real action quickly, then mark the genuine win.
- Define the aha for the app. For Reps it is logging the first workout.

**Examples**
- ❌ Ending onboarding on the paywall and dropping the user onto an empty home screen.
- ✅ Guide the first action: "Log your first workout," with a one-tap example set, then show it recorded in the history. That first logged workout is the real success worth celebrating.

### First-run empty states

- A new user's first screens have no data yet. Treat each empty state as an onboarding surface: say what belongs here and give the one action that fills it.

**Examples**
- ❌ An empty History tab that shows only "No data."
- ✅ "Your workouts will show up here. Log your first one to start your streak," with a button that does it.

---

## Copy that cuts across screens

These rules apply to every screen, not one type.

- **Button labels**. Use the specific verb for the action over a generic word. "Log a workout" and "Turn on reminders" beat "Continue", "Next", and "Submit". The label should say what happens when the user taps.
- **Microcopy voice**. Plain, second person, short. Tell the user what happens next and what they get.
- **Permission rationale**. Name the benefit to the user and set expectations on how often and how the access is used. "We'll remind you only on training days" beats "Enable notifications."
- **The "why" on every ask**. Any field or permission gets a one-line reason. No reason, no ask.
- **Progress**. In a multi-step flow, show progress so the user can see the end of it. Keep it honest. Do not show "step 2 of 4" and then add steps.

**Examples**
- ❌ Button reads "Submit"; permission screen reads "Reps would like to send you notifications."
- ✅ Button reads "Build my plan"; permission screen reads "Get a reminder on the days you planned to train. We won't message you on rest days."

---

## Workflow: designing an onboarding flow

Work in this order. Each step leans on the sections above.

1. **Define the aha moment and pick the model.** What is the first real value for this app, and is it a get-to-value-fast utility or an investment-style subscription app? This single choice sets the shape of everything else.
2. **List what the app genuinely needs.** Which permissions, account, and data, and at what point each is actually required. Cut anything not tied to reaching first value.
3. **Draft the screen sequence.** Start from the matching default sequence above, then adjust for this app.
4. **Write each screen's copy.** One job per screen, benefit-led, a reason on every ask, specific button labels.
5. **Place permission primes in context.** One at a time, each with a value-first rationale screen before the system prompt.
6. **Place sign-up and the paywall.** Sign-up as late as the app allows; the paywall per the model you chose, flagged for testing.
7. **Mark the activation step and the first-run empty states.** Make sure the flow ends on the first real action, not on a wall.
8. **Run the Self-check.**
9. **Output the flow** as an ordered list of screens with the copy for each, and flag every contested choice (paywall placement, survey length, name capture) and anything that needs checking against current platform docs (the items marked "Verify current" in Platform notes).

---

## Self-check

Run the drafted flow through this before calling it done. It turns the rules above into a pass/fail list.

- [ ] The **aha moment** is named, and the flow reaches it as early as the model allows.
- [ ] The **business model** is chosen on purpose (fast-to-value vs investment), and the flow length matches it.
- [ ] **Every screen has one job.**
- [ ] **Every data request has a stated, user-facing reason.**
- [ ] The **survey**, if any, uses one input per screen and asks only for answers that change the product.
- [ ] **No permission is requested at first launch without context.** Each has a priming screen before the system prompt, asked one at a time.
- [ ] The **priming screen does not imitate the system alert.**
- [ ] **Sign-up is deferred** until value is shown, or justified by a core feature, and a privacy-focused login is offered if a social login is.
- [ ] The **paywall offer** states what you get, the price, and the trial terms in plain language, and its placement is flagged for testing.
- [ ] **Multi-step flows show honest progress.**
- [ ] **Button labels are specific verbs,** not "Continue" or "Submit".
- [ ] **First-run empty states** tell the user what goes there and give one action.
- [ ] **Contested choices and any platform facts needing verification are flagged** in the output.

---

## Common mistakes

| Mistake | Fix |
| ------- | --- |
| Sign-up wall before any value | Defer sign-up until after the first win, offer guest or skip, and gate on an account only when a feature needs it (Apple 5.1.1) |
| Firing every permission prompt at launch | Prime in context, one at a time, value first, right before the feature needs it |
| A long survey bolted onto a free utility | Match flow length to the model; utilities get to value fast, surveys belong to investment-style subscription apps |
| Asking for data with no reason | Put the "why" on every field and cut anything that does not change the product |
| A fake "analyzing" delay | Show processing only when the app actually computes something; faking it costs trust |
| Fabricated reviews or testimonials | Use real ratings and real testimonials; fake ones break trust and violate store rules |
| Generic button labels ("Continue", "Submit") | Use the specific action verb ("Log a workout", "Turn on reminders") |
| Ending on a paywall into an empty home screen | End on the activation action and design first-run empty states with one clear next step |
| Treating paywall placement as settled | A/B test it; hard vs post-value depends on the app |

---

## Platform notes

Permission flows and account rules differ by platform and change over time. Verify against the current Apple and Android docs before relying on a specific rule.

### iOS

- **Requesting permission**. Ask in context, only when a feature needs it, with a usage-description string in `Info.plist` explaining why (required for each sensitive permission). A custom priming screen before the system prompt is allowed, but it must not imitate the system alert or use a button that implies it grants access (Apple Human Interface Guidelines). Verify current.
- **Notifications**. The standard permission alert is shown once. If the user declines, you cannot re-trigger the system prompt; they would have to enable it in Settings, so prime first. **Provisional authorization** (`UNAuthorizationOptions.provisional`, iOS 12 and later) lets you deliver notifications quietly to Notification Center with no upfront prompt, and the user keeps or turns them off after seeing a few.
- **App Tracking Transparency**. If the app tracks users across other companies' apps and sites, it must request permission through the ATT framework with an `NSUserTrackingUsageDescription` string. The prompt is one-time; the choice is remembered until the user changes it in Settings or reinstalls the app, and the advertising identifier stays zeroed unless the user allows tracking. A pre-prompt explaining value is common practice, but Apple bars screens that imitate the system alert. Verify current.
- **Accounts**. Do not require sign-in unless account features are integral to the app (Guideline 5.1.1). If the app supports account creation, it must offer in-app account deletion. If it offers a third-party or social login, it must also offer an equivalent privacy-focused login: one that limits collection to the user's name and email, lets the user keep the email private, and does not collect interactions with the app for advertising without consent (Guideline 4.8; Sign in with Apple meets this). Verify current.
- **Launch**. Apple treats onboarding as optional and tells you to get people to content fast. The launch screen is not an onboarding screen or a splash screen (Apple Human Interface Guidelines).

### Android

- **Runtime permissions**. Request sensitive permissions at runtime, in context, when the feature needs them. Use `shouldShowRequestPermissionRationale()` to show an explanation before re-asking.
- **Notifications**. `POST_NOTIFICATIONS` is a runtime permission introduced in **Android 13 (API level 33)**. On Android 13 and later, notifications are off by default for new installs and the app must request the permission, so show a rationale first and ask in context rather than blindly on launch. For apps that target API level 32 or lower, the system requests it automatically when the app creates its first notification channel.
- **Health data**. Health and fitness data on Android goes through Health Connect (the iOS equivalent is HealthKit). Request only the data types the app actually uses. Verify current.

---

## Notes & verification

- Platform rules (permission flows, ATT, notification behavior, account and login requirements, Android API levels) change over time. Check the current Apple Human Interface Guidelines, the Apple App Store Review Guidelines, and the Android developer docs before relying on a specific rule.
- The contested calls in this skill are genuinely unsettled and app-dependent: paywall placement, onboarding length, how much a personalization survey helps, name capture, and processing screens. Treat them as defaults to test, not laws, and A/B test where you can.
- Opt-in and conversion rates move constantly and vary by category, so this skill avoids quoting them. If you need a benchmark, measure your own funnel rather than trusting a number from a blog.
- This skill is guidance only. It needs no API key and no network access.
