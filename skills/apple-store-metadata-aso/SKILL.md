---
name: sleek-apple-store-metadata-aso
description: Use when the user wants to improve an Apple app's App Store Optimization (ASO) by writing, optimizing, auditing, or localizing the store listing metadata and copy: app name, subtitle, keyword field, description, promotional text, what's new, and screenshot/app-preview captions. Covers high-level requests ("optimize my App Store listing", "help me rank for X") and specific ones ("write a 30-char subtitle", "pick keywords under 100 chars", "review my description for keyword stuffing"). Not for translating in-app UI strings or general code localization.
---

# Apple App Store Metadata & ASO

App Store Optimization (ASO) is how you write and structure a listing so people find the app and then install it. It does two jobs at once: ranking in search and browse, and turning those impressions into downloads. This skill is about the listing **metadata** that does that work. It explains what each field is, the limits on it, how Apple indexes and shows it, how to research the words that go in it, and how to write the copy.

> **Scope.** Apple App Store and App Store Connect only. Google Play works differently, and the clearest example is that Google indexes the long description for keywords while Apple doesn't, so advice written for Play will mislead you here.
>
> This skill stays on **metadata**: the text and creative you author in App Store Connect. Some things move rankings and installs but aren't metadata, so they're out of scope: ratings and reviews and when to prompt for them (that's onboarding), retention, crash and hang rates (that's engineering), and your off-store web presence. They matter, they just belong to other phases.

---

## Overview

Two goals, and most fields serve one of them, not both:

- **Discoverability (ranking):** Apple indexes only some of the text fields. You get found by putting the right keywords in those fields without wasting characters or repeating terms.
- **Conversion (impression to install):** Once someone sees the listing in search or on the product page, the visuals and the first lines of copy decide whether they tap **Get**. Keywords barely matter at this stage.

The keyword field is pure ranking input that no user ever sees. Screenshots sit at the conversion end: they're what convince someone to tap Get. Work out which job a field is doing before you write in it, because that decides how you write it.

---

## What Apple indexes vs. what it shows

A handful of facts drive most metadata decisions:

- **Only some text fields are indexed for search:** the app name, the subtitle, and the hidden keyword field. The developer name and in-app purchase / in-app event display names are also indexed, with less weight.
- **The description is not indexed for search.** People get this wrong all the time because Google Play does the opposite. On the App Store the description is there to convert, not to rank, so stuffing keywords into it does nothing for search and makes it harder to read.
- **Promotional text isn't indexed either,** but you can change it any time without shipping a new version, which makes it the right home for anything time-sensitive.
- **Apple builds the phrases for you.** You give the keyword field single words and it combines them into phrases. Don't spend characters on a term that already appears in the name or subtitle. Across all three fields, each word only needs to show up once.
- **Search results are visual first.** Apple shows the icon, name, subtitle, and the first one to three screenshots, or an autoplaying app preview. A lot of the install decision happens right there, before anyone opens the full product page.

---

## Workflow: writing or optimizing a listing

When you're asked to write a listing from scratch or improve an existing one, work in this order. Each step has a section below with the detail.

1. **Gather the inputs.** What the app does, its single core value, the target storefront and market, the main competitors, and whether it's new or established. A new app leans long-tail; an established one can fight for head terms.
2. **Research and validate keywords.** Brainstorm the landscape, score candidates with an ASO tool, and, if an Apple Search Ads account is available, validate the best ones with an exact-match campaign (otherwise flag them as unvalidated). End with a ranked list split into primary (one or two), secondary (a handful), and long-tail.
3. **Place keywords by field weight, highest first.** Title takes the brand plus the leading primary keyword. Subtitle takes the strongest secondary keywords and a value proposition, reusing no words from the title. The keyword field takes everything else as single words. If the storefront indexes more than one localization, use the extra slots for more keywords (see Localization).
4. **Write the conversion copy.** Description opens with the core benefit. Promotional text carries the current hook. The first two screenshots carry the value props.
5. **Run the self-check** below against every field.
6. **Output the listing** with a character count next to each field, and flag anything that still needs Apple Search Ads validation or a check against current Apple docs.
7. **Iterate after launch.** Change indexed fields in batches, wait the roughly two-week index-and-settle cycle, and test every 2 to 4 weeks. Refresh promotional text whenever you like, since it isn't indexed.

---

## Metadata structure (field reference)

Every field a listing can have, with its limits and how it behaves. These numbers and indexing rules change over time, so check the current App Store Connect UI or Apple's docs before you rely on an exact figure.

| Field | Limit | Visible to users? | Indexed for search? | Editable without a new app version? |
| ----- | ----- | ----------------- | ------------------- | ----------------------------------- |
| **App Name** | 30 chars | Yes (everywhere) | **Yes (highest weight)** | No (review required) |
| **Subtitle** | 30 chars | Yes (under the name) | **Yes (high weight)** | No (review required) |
| **Keyword field** | 100 chars | **No (hidden)** | **Yes** | No (review required) |
| **Promotional Text** | 170 chars | Yes (top of description) | No | **Yes (anytime, no review)** |
| **Description** | 4,000 chars | Yes (product page, truncated) | **No** | No (review required) |
| **What's New** (release notes) | 4,000 chars | Yes (product page) | No | No (tied to a version) |
| **App Icon** | 1024×1024 px PNG, no alpha | Yes (everywhere) | No | No (review required) |
| **Screenshots** | Up to 10 per device size | Yes (results + product page) | Debated (caption text) | No (review required) |
| **App Previews** (video) | Up to 3 per device, 15-30s | Yes (results + product page) | No | No (review required) |
| **Primary / Secondary Category** | 1 each | Yes | Influences browse ranking | No (review required) |
| **Support URL** | n/a | Yes | No | Yes |
| **Marketing URL** | n/a | Yes | No | Yes |
| **Privacy Policy URL** | n/a | Yes | No | Yes |
| **In-App Purchase / Event names** | per Apple limits | Yes | **Yes (lower weight)** | Reviewed separately |

### Text metadata

- **App Name**. Your most valuable field for ranking, and the name people remember you by. Usually `Brand` or `Brand: primary value/keyword`. 30 characters.
- **Subtitle**. A short value proposition under the name, and the second-strongest keyword field. It should complement the name and not reuse its words. 30 characters.
- **Keyword field**. Hidden, comma-separated, 100 characters total. Pure ranking input. Apple recombines the words into phrases, so list single terms and use the whole budget.
- **Promotional Text**. 170 characters above the description, editable any time without review. Good for launches, sales, seasonal messaging, or a fresh hook between version updates. Not indexed.
- **Description**. Up to 4,000 characters, written to convert. The first few lines show before the **more** cutoff and matter most. Not indexed for search on iOS. On Google Play the description is a primary ranking factor, which is why cross-platform advice gets this wrong.
- **What's New**. Up to 4,000 characters of release notes per version. It signals an app that's actively maintained. Not indexed.

### Visual assets

- **App Icon**. 1024×1024 PNG, no transparency. It appears on every surface, so it does a lot of the conversion work.
- **Screenshots**. Up to 10 per device size. The first one to three show in search results and carry most of the conversion weight. Supply assets for the current required device sizes, such as the largest iPhone display and iPad. Apple can scale down from the largest sizes for some older devices, but confirm the current requirements.
- **App Previews**. Up to 3 autoplaying videos per device size, 15 to 30 seconds each. Previews can start muted, so the poster frame has to stand on its own.

### Identity & classification

- **Categories**. One primary, which feeds category browse and charts, plus one optional secondary.
- **Age Rating**. Set through the content questionnaire. It affects eligibility and who sees the app.
- **URLs**. Support and Privacy Policy URLs are required; the Marketing URL is optional. All three can change without a new version.
- **Developer Name**. Carries some search weight, and consistent branding helps.

### Localization

Don't think of localizations as "the translations of my listing." Each **localization** is its own full set of metadata, with its own name (30), subtitle (30), keyword field (100), description, and screenshots. For ranking they behave like extra keyword slots.

The mechanism is cross-indexing: **a storefront indexes keywords from a defined set of localizations, not only the one that matches the country's main language.** Every localization in that set adds another indexed name, subtitle, and 100-character keyword field for the same storefront, and most apps leave those extra slots empty. So the play for a target market is:

- Find which localizations the storefront indexes. This is Apple's mapping and it changes over time, so confirm the current set before you rely on it.
- Fill each extra localization's keyword field first, because it's hidden. Use the keywords that didn't fit your primary fields, written in the language people actually search in for that storefront. Overflow there has no downside.
- Be careful with the extra localizations' name and subtitle, because those are visible. They show to anyone browsing in that localization, including in its own home storefront, so don't stuff them with off-language keywords if you also serve that market.
- Repeating a keyword across two indexed localizations may lift its ranking for that storefront, since it then appears in more than one indexed source. Repeating a word within a single localization does nothing, so treat this as a quirk worth testing, not a guarantee.

Beyond the keyword angle, genuinely localizing for a market you operate in lifts both ranking and conversion there: local search terms, idioms, and screenshots that fit the place.

### Custom Product Pages & A/B testing (a plus, not the foundation)

These sit on top of the core listing, so reach for them once the fundamentals are solid.

- **Custom Product Pages (CPP):** Up to 70 alternate product pages, each with its own screenshots, app preview, and promotional text, reachable by unique URLs. They began as a paid-traffic tool, but Apple now lets you assign keywords to a custom product page so it can surface in organic search too, which makes them a discoverability lever and not just a place to point ad campaigns.
- **Product Page Optimization (PPO):** Apple's built-in A/B testing. Run up to 3 treatments against the live page, varying the icon, screenshots, or app preview, and read the conversion lift in App Store Connect before you roll out a winner. Useful for proving which creative metadata works, but it's a complement to getting the indexed text fields right.

---

## Keyword research & validation

The fields above are only as good as the words you put in them. This is how to choose those words.

- **Go long-tail when the app is new.** A new app won't outrank established ones on a broad head term like "vegan". Target specific phrases like "raw vegan recipes" to win a smaller race first, then climb toward broader terms as the app gains weight.
- **Read tool scores as heuristics, not truth.** ASO tools (AppTweak, Astro, AppSprint, Sensor Tower) rate keywords on popularity and difficulty, usually on a 0-100 scale. A common rule of thumb is to aim for popularity around 30 or higher with difficulty under 50. Those scores come from the tools' own models, often inferred from ad-bidding patterns rather than real search counts, so they point you in a direction rather than settle anything.
- **Validate with Apple Search Ads when you can.** With an ASA account and a little budget, the strongest check is an exact-match campaign on a candidate keyword: real impressions and a healthy tap-through rate mean genuine demand worth putting in your metadata, and weak numbers mean the tool oversold it. Without ASA, treat tool-sourced keywords as unvalidated and flag them as candidates to watch after launch.
- **Brainstorm with an LLM, then verify.** Use a model to map the keyword landscape and surface terms you'd miss, but run the candidates through an ASO tool and ASA before they go in a field. Don't ship un-validated guesses.

---

## Best practices (per field)

> The examples below all use one made-up app, **Reps**, a home and gym workout tracker, so you can see the fields working together instead of in isolation. Character counts are in parentheses where the field has a hard limit. ❌ marks a bad example, ✅ a good one.

### App name

- Put your primary keyword at the very start. The name carries the most ranking weight, and leading with the keyword also helps the app surface in search autosuggest.
- Lead with brand only when the brand itself is the thing people search for. Otherwise pair brand with the primary keyword.
- Stay well inside 30 characters and don't stuff keywords. It reads as spam and risks rejection.
- Don't burn a word here that you also need in the subtitle or keyword field.

**Examples**
- ❌ `Reps` (4). Brand only. No keyword does any search work, and the bare word is generic.
- ❌ `Reps: #1 Best Workout App Free` (30). Wastes space on "App", and "#1", "Best", and "Free" invite a metadata rejection.
- ✅ `Home Workouts & Gym Log: Reps` (29). The primary keyword "Home Workouts" leads, "Gym Log" rides along, brand sits at the end, and nothing is wasted.

### Subtitle

- Use it for secondary keywords and for context the title doesn't have room for.
- Don't repeat the title's primary keyword. The solid reason is that Apple indexes each word once across name, subtitle, and keyword field, so a duplicate wastes the slot. (Some practitioners go further and say repeating it "confuses the algorithm" or dilutes the title's weight. The mechanism is shaky and contested, but the practical advice lands in the same place: don't repeat, spend the space on new terms.)
- Put the highest-value terms first.

**Examples**
- ❌ `Home Workout & Gym Tracker` (26). Every strong word here ("Home", "Workout", "Gym") is already in the title, so the subtitle adds almost no new search coverage.
- ✅ `Exercise tracker & progress` (27). Three new indexable words ("Exercise", "tracker", "progress"), none in the title, and it describes what Reps actually is, a tracker, instead of promising coaching it doesn't do.

### Keyword field

- Single words, comma-separated, with no space after the commas. Spaces waste characters, and Apple recombines the words into phrases on its own.
- No duplicates of words already in the name or subtitle. No plurals when the singular is present. No category names, "app", or "free".
- Don't use competitor trademarks. It risks rejection and legal trouble. Use the full 100 characters.

**Examples**
- ❌ `workout, workouts, exercise app, gym, free progress tracker` (59). Nearly every token is wasted: "workouts" and "gym" repeat the name, "exercise", "progress", and "tracker" repeat the subtitle, "workout" only pairs a singular with the plural that's already there, "app" and "free" aren't allowed, and the spaces after each comma cost characters too.
- ✅ `hiit,cardio,strength,training,weight,dumbbell,abs,core,bodyweight,routine,sets,muscle,pilates,squat` (99). All new single words, no overlap with the name or subtitle, no spaces, and nearly the whole budget used.

### Description

- Make the first few lines count, before the **more** cutoff. Lead with the core benefit.
- Write for people, not search. On iOS the description isn't indexed, so keep it scannable and benefit-led with short paragraphs or bullets. (If you also publish on Google Play, that store does index the description, so you may want a different version there.)

**Examples**
- ❌ `Reps is the best workout app for home workouts, gym workouts, fitness, exercise, hiit, cardio, and strength training workouts!` Keyword soup that does nothing for iOS ranking and reads like spam to the one person who matters, the user deciding whether to install.
- ✅ `Build a workout habit that actually sticks. Reps gives you guided home and gym routines, tracks every set, and shows your strength climbing week over week.` Opens with the benefit, names what the app does, and stays readable.

### Promotional text

- Use it for time-sensitive messaging like launches, sales, and seasonal pushes, since it updates without review.
- Put the strongest hook in the first 50 or so characters.
- Don't waste it restating the subtitle. It's free to change, so it's a low-risk place to test angles, even though it never affects ranking.

**Examples**
- ❌ `Reps is a fitness app for workouts and exercise.` (48). Restates the subtitle, says nothing timely, and squanders the one field you can update on a whim.
- ✅ `New in June: 20 follow-along HIIT sessions plus an Apple Watch app. Start your free 7-day trial and plan your first week in under 5 minutes.` (140). Timely, leads with the hook, and gives a concrete reason to act now.

### What's New

- Lead with the most meaningful change in plain language. It's a trust surface that signals an app someone is actually maintaining.
- Don't ship "Bug fixes and performance improvements" as the entire note release after release. Name what changed.
- It isn't indexed, so it's for users, not keywords. You can still mention a new feature that happens to support a keyword theme, just don't stuff it.

**Examples**
- ❌ `Bug fixes and performance improvements.` Generic, and as a standing note it tells a prospective user nothing.
- ✅ `You can now download workouts for offline gym sessions, and rest timers keep running on your lock screen. We also fixed a crash when syncing Apple Health.` Concrete, leads with the change people will care about.

### Screenshots & app previews

- The first two screenshots do most of the work in search results, so put your strongest value proposition there.
- Caption each one with 3-5 words of bold text and show the real UI behind it, not abstract marketing art.
- Whether Apple reads caption text for search is unsettled, so don't count on screenshots for ranking. Either way, make the captions communicate the app's value simply and directly in plain language. That's what converts, and it's the safe bet if the OCR signal turns out to be real.
- Keep the app preview's poster frame compelling and readable with the sound off.

**Examples**
- ❌ A first screenshot of the sign-in screen, or a raw UI shot with no caption. It asks the viewer to figure out the value themselves.
- ✅ First two screenshots each carrying one value prop in bold over a real screen: `Guided home workouts` and `Track every set`.

### App icon

- Keep it simple and legible at thumbnail size, where most people first see it. Fine detail and small text disappear.
- Use a distinct silhouette and color so it stands out in a results list, and keep it consistent with your screenshots so the listing feels like one thing.

### In-app purchase & event names

- These display names carry a little search weight, so name them with real terms ("Unlimited Workouts", "Pro Annual") rather than internal codes like `sku_premium_001`.
- Keep them honest and representative of what's sold. Misleading names are a rejection risk, and the keyword upside is small.

---

## Self-check

Run every drafted field through this before you call a listing done. It turns the rules above into a pass/fail list.

- [ ] **App name** is within 30 characters, the primary keyword is present and leads (unless the brand is itself the search term), and there's no "app", "free", pricing, or "#1/best" claim.
- [ ] **Subtitle** is within 30 characters, reuses no word from the app name, and leads with the highest-value terms.
- [ ] **Keyword field** is within 100 characters, comma-separated with no spaces, duplicates no word from the name or subtitle, drops plurals whose singular is present, avoids "app"/"free"/category names and competitor trademarks, and uses most of the budget.
- [ ] **Across name + subtitle + keyword field, each keyword appears once.**
- [ ] **Description** opens with the benefit in the first few lines and isn't keyword-stuffed (it isn't indexed on iOS).
- [ ] **Promotional text** is within 170 characters with the hook in the first ~50.
- [ ] **Screenshots** lead with the two strongest value props, captioned in 3-5 words over real UI.
- [ ] **What's New** leads with a real, specific change, not a bare "bug fixes" note.
- [ ] **In-app purchase / event display names** use real keyword terms, not internal codes.
- [ ] **Nothing risks a metadata rejection** (see "Metadata that gets rejected" below).
- [ ] **A character count is recorded for each limited field,** and anything unvalidated is flagged.

---

## Iteration & testing cadence

Metadata changes aren't instant, and changing too much at once makes the results unreadable.

- Expect roughly a week for Apple to index a change and about another week for rankings to settle. Wait a full cycle before you judge a change.
- Change indexed metadata in deliberate batches and test every 2 to 4 weeks rather than constantly. If you move five things at once and ranking shifts, you won't know which one did it.
- Promotional text is the exception. It isn't indexed and updates immediately, so you can refresh it without disturbing a running test on your other fields.

---

## Metadata that gets rejected

Apple's "Accurate Metadata" guideline (App Review Guideline 2.3) governs what you can put in these fields. Keep metadata truthful and relevant, or you risk a rejection and, separately, a ranking penalty. Common triggers:

- Keywords in the name or subtitle that aren't relevant to the app, or a keyword field padded with unrelated terms.
- Competitor names or trademarks you don't own.
- Pricing or promotional words in the app name, like "Free", "Sale", or "#1".
- Unsubstantiated superlatives ("best", "number one") with nothing behind them.
- Screenshots or previews that don't reflect what the app actually does.
- Placeholder text left in any field, or references to other platforms.

---

## Common mistakes

| Mistake | Fix |
| ------- | --- |
| Keyword-stuffing the **description** to rank on iOS | The App Store doesn't index the description. Write it to convert and keep keywords in the name, subtitle, and keyword field |
| Repeating the same word across name, subtitle, and keyword field | Use each word once. Apple combines tokens, so there's no point spending the limit twice |
| Spaces after commas in the keyword field | Remove them. They eat the 100-character budget for nothing |
| Using competitor brand names as keywords | Drop them. They invite rejection and trademark trouble || Trusting tool volume without checking | Validate with an Apple Search Ads exact-match campaign when you have one; otherwise flag the keywords as unvalidated |
| Changing metadata constantly | Index and settle takes ~2 weeks; batch changes and test every 2 to 4 weeks so you can read the result |
| Treating screenshots 4 to 10 as important | The first two drive most of the conversion in search results, so put the value there |

---

## Notes & verification

- Character limits, indexed fields, device-size requirements, CPP and PPO counts, and storefront localization behavior all change over time. When precision matters, check the current App Store Connect UI and Apple's official docs rather than trusting the numbers here.
- Ranking and conversion also lean on factors this skill leaves out because they aren't metadata: ratings and reviews, retention, crash and hang rates, and off-store presence.
- This skill is guidance only. It needs no API key and no network access.
