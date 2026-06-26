---
name: sleek-apple-store-metadata-aso
description: Use when the user wants to write, optimize, audit, or localize Apple App Store listing metadata for App Store Optimization (ASO) — app name, subtitle, keyword field, description, promotional text, what's new, and screenshot/app-preview captions. Covers high-level requests ("optimize my App Store listing", "help me rank for X") and specific ones ("write a 30-char subtitle", "pick keywords under 100 chars", "review my description for keyword stuffing").
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

The keyword field is pure ranking input that no user ever sees. Screenshots sell the app but count for nothing in search. So before you write anything in a field, work out which job it does, because that decides how you write it.

---

## What Apple indexes vs. what it shows

A handful of facts drive most metadata decisions:

- **Only some text fields are indexed for search:** the app name, the subtitle, and the hidden keyword field. The developer name and in-app purchase / in-app event display names are also indexed, with less weight.
- **The description is not indexed for search.** People get this wrong all the time because Google Play does the opposite. On the App Store the description is there to convert, not to rank, so stuffing keywords into it does nothing for search and makes it harder to read.
- **Promotional text isn't indexed either,** but you can change it any time without shipping a new version, which makes it the right home for anything time-sensitive.
- **Apple builds the phrases for you.** You give the keyword field single words and it combines them into phrases. Don't spend characters on a term that already appears in the name or subtitle. Across all three fields, each word only needs to show up once.
- **Search results are visual first.** Apple shows the icon, name, subtitle, and the first one to three screenshots, or an autoplaying app preview. A lot of the install decision happens right there, before anyone opens the full product page.

---

## Metadata structure (field reference)

Every field a listing can have, with its limits and how it behaves. These numbers and indexing rules change over time, so check the current App Store Connect UI or Apple's docs before you rely on an exact figure.

| Field | Limit | Visible to users? | Indexed for search? | Editable without a new app version? |
| ----- | ----- | ----------------- | ------------------- | ----------------------------------- |
| **App Name** | 30 chars | Yes — everywhere | **Yes (highest weight)** | No (review required) |
| **Subtitle** | 30 chars | Yes — under the name | **Yes (high weight)** | No (review required) |
| **Keyword field** | 100 chars | **No (hidden)** | **Yes** | No (review required) |
| **Promotional Text** | 170 chars | Yes — top of description | No | **Yes (anytime, no review)** |
| **Description** | 4,000 chars | Yes — product page (truncated) | **No** | No (review required) |
| **What's New** (release notes) | 4,000 chars | Yes — product page | No | No (tied to a version) |
| **App Icon** | 1024×1024 px PNG, no alpha | Yes — everywhere | No | No (review required) |
| **Screenshots** | Up to 10 per device size | Yes — results + product page | No | No (review required) |
| **App Previews** (video) | Up to 3 per device, 15–30s | Yes — results + product page | No | No (review required) |
| **Primary / Secondary Category** | 1 each | Yes | Influences browse ranking | No (review required) |
| **Support URL** | — | Yes | No | Yes |
| **Marketing URL** | — | Yes | No | Yes |
| **Privacy Policy URL** | — | Yes | No | Yes |
| **In-App Purchase / Event names** | per Apple limits | Yes | **Yes (lower weight)** | Reviewed separately |

### Text metadata

- **App Name** — Your most valuable field for ranking, and the name people remember you by. Usually `Brand` or `Brand: primary value/keyword`. 30 characters.
- **Subtitle** — A short value proposition under the name, and the second-strongest keyword field. It should complement the name and not reuse its words. 30 characters.
- **Keyword field** — Hidden, comma-separated, 100 characters total. Pure ranking input. Apple recombines the words into phrases, so list single terms and use the whole budget.
- **Promotional Text** — 170 characters above the description, editable any time without review. Good for launches, sales, seasonal messaging, or a fresh hook between version updates. Not indexed.
- **Description** — Up to 4,000 characters, written to convert. The first few lines show before the **more** cutoff and matter most. Not indexed for search on iOS. On Google Play the description is a primary ranking factor, which is why cross-platform advice gets this wrong.
- **What's New** — Up to 4,000 characters of release notes per version. It signals an app that's actively maintained. Not indexed.

### Visual assets

- **App Icon** — 1024×1024 PNG, no transparency. It appears on every surface, so it does a lot of the conversion work.
- **Screenshots** — Up to 10 per device size. The first one to three show in search results and carry most of the conversion weight. Supply assets for the current required device sizes, such as the largest iPhone display and iPad. Apple can scale down from the largest sizes for some older devices, but confirm the current requirements.
- **App Previews** — Up to 3 autoplaying videos per device size, 15 to 30 seconds each. Previews can start muted, so the poster frame has to stand on its own.

### Identity & classification

- **Categories** — One primary, which feeds category browse and charts, plus one optional secondary.
- **Age Rating** — Set through the content questionnaire. It affects eligibility and who sees the app.
- **URLs** — Support and Privacy Policy URLs are required; the Marketing URL is optional. All three can change without a new version.
- **Developer Name** — Carries some search weight, and consistent branding helps.

### Localization

Each **localization** is a full set of metadata of its own — name, subtitle, keyword field, description, screenshots, and so on — and it's one of the most underused levers in ASO.

The reason it matters for ranking is cross-indexing: a storefront pulls keywords from more than one localization. The classic example is the US storefront, which has historically indexed the **Spanish (Mexico)** name and subtitle alongside English (U.S.). The tactic that comes out of this:

- Fill the Spanish (Mexico) name and subtitle with **additional English keywords**, not a translation. You're buying a second set of title/subtitle slots aimed at the same US users, which is room for roughly 6–10 "plan B" terms you couldn't fit in your primary fields.
- Some practitioners report that repeating a stubborn keyword in the Spanish (Mexico) title lifts its US ranking, even though repeating it within a single localization wouldn't. Treat that as a tactic to test, not a guarantee.
- Apple changes which localizations a storefront indexes. Confirm the current pairings before you build a strategy on a specific one.

Beyond the hack, real localization beats translation. Local search terms, idioms, and culturally fitting screenshots tend to lift both ranking and conversion in that market.

### Custom Product Pages & A/B testing (a plus, not the foundation)

These are about testing and targeting metadata rather than the core copy, so treat them as a bonus once the fundamentals are solid.

- **Custom Product Pages (CPP):** Up to 35 alternate product pages, each with its own screenshots, app preview, and promotional text, reachable by unique URLs. They're for targeted marketing and ad traffic, not organic search, since they aren't separately indexed.
- **Product Page Optimization (PPO):** Apple's built-in A/B testing. Run up to 3 treatments against the live page, varying the icon, screenshots, or app preview, and read the conversion lift in App Store Connect before you roll out a winner. Useful for proving which creative metadata works, but it's a complement to getting the indexed text fields right.

---

## Keyword research & validation

The fields above are only as good as the words you put in them. This is how to choose those words.

- **Go long-tail when the app is new.** A new app won't outrank established ones on a broad head term like "vegan". Target specific phrases like "raw vegan recipes" to win a smaller race first, then climb toward broader terms as the app gains weight.
- **Read tool scores as heuristics, not truth.** ASO tools (AppTweak, Astro, AppSprint, Sensor Tower) rate keywords on popularity and difficulty, usually on a 0–100 scale. A common rule of thumb is to aim for popularity around 30 or higher with difficulty under 50. Those scores come from the tools' own models, often inferred from ad-bidding patterns rather than real search counts, so they point you in a direction rather than settle anything.
- **Validate with Apple Search Ads before committing.** The strongest check is to run an exact-match ASA campaign on a candidate keyword. Real impressions and a healthy tap-through rate mean genuine demand worth putting in your metadata. Weak numbers mean the tool oversold it.
- **Brainstorm with an LLM, then verify.** Use a model to map the keyword landscape and surface terms you'd miss, but run the candidates through an ASO tool and ASA before they go in a field. Don't ship un-validated guesses.

---

## Best practices (per field)

> **Status: corpus pending.** These are established field-level rules, safe to apply on any listing. More examples and house style will come from further corpus the user provides.

### App name

- Put your primary keyword at the very start. The name carries the most ranking weight, and leading with the keyword also helps the app surface in search autosuggest.
- Lead with brand only when the brand itself is the thing people search for. Otherwise pair brand with the primary keyword.
- Stay well inside 30 characters and don't stuff keywords. It reads as spam and risks rejection.
- Don't burn a word here that you also need in the subtitle or keyword field.

_Good / bad examples: to be added from corpus._

### Subtitle

- Use it for secondary keywords and for context the title doesn't have room for.
- Don't repeat the title's primary keyword. The solid reason is that Apple indexes each word once across name, subtitle, and keyword field, so a duplicate wastes the slot. (Some practitioners go further and say repeating it "confuses the algorithm" or dilutes the title's weight. The mechanism is shaky and contested, but the practical advice lands in the same place: don't repeat, spend the space on new terms.)
- Put the highest-value terms first.

_Good / bad examples: to be added from corpus._

### Keyword field

- Single words, comma-separated, with no space after the commas. Spaces waste characters, and Apple recombines the words into phrases on its own.
- No duplicates of words already in the name or subtitle. No plurals when the singular is present. No category names, "app", or "free".
- Don't use competitor trademarks. It risks rejection and legal trouble. Use the full 100 characters.

_Good / bad examples: to be added from corpus._

### Description

- Make the first few lines count, before the **more** cutoff. Lead with the core benefit.
- Write for people, not search. On iOS the description isn't indexed, so keep it scannable and benefit-led with short paragraphs or bullets. (If you also publish on Google Play, that store does index the description, so you may want a different version there.)

_Good / bad examples: to be added from corpus._

### Promotional text

- Use it for time-sensitive messaging like launches, sales, and seasonal pushes, since it updates without review.
- Put the strongest hook in the first 50 or so characters.

_Good / bad examples: to be added from corpus._

### What's New

- Show the app is being looked after. Call out real changes instead of a flat "bug fixes".

_Good / bad examples: to be added from corpus._

### Screenshots & app previews

- The first two screenshots do most of the work in search results, so put your strongest value proposition there.
- Caption each one with 3–5 words of bold text and show the real UI behind it, not abstract marketing art.
- Keep the app preview's poster frame compelling and readable with the sound off.

_Good / bad examples: to be added from corpus._

---

## Iteration & testing cadence

Metadata changes aren't instant, and changing too much at once makes the results unreadable.

- Expect roughly a week for Apple to index a change and about another week for rankings to settle. Wait a full cycle before you judge a change.
- Change indexed metadata in deliberate batches and test every 2 to 4 weeks rather than constantly. If you move five things at once and ranking shifts, you won't know which one did it.
- Promotional text is the exception. It isn't indexed and updates immediately, so you can refresh it without disturbing a running test on your other fields.

---

## Common mistakes

> Scaffold, to be expanded with corpus material. Established entries below.

| Mistake | Fix |
| ------- | --- |
| Keyword-stuffing the **description** to rank on iOS | The App Store doesn't index the description. Write it to convert and keep keywords in the name, subtitle, and keyword field |
| Repeating the same word across name, subtitle, and keyword field | Use each word once. Apple combines tokens, so there's no point spending the limit twice |
| Spaces after commas in the keyword field | Remove them. They eat the 100-character budget for nothing |
| Using competitor brand names as keywords | Drop them. They invite rejection and trademark trouble |
| Translating the Spanish (Mexico) fields instead of using the cross-index | The US store reads es-MX name/subtitle, so fill them with extra English keywords, not a translation |
| Trusting tool volume without checking | Validate candidates with an Apple Search Ads exact-match campaign before they go in your metadata |
| Changing metadata constantly | Index and settle takes ~2 weeks; batch changes and test every 2–4 weeks so you can read the result |
| Treating screenshots 4–10 as important | The first two drive most of the conversion in search results, so put the value there |
| _…more from corpus_ | |

---

## Notes & verification

- Character limits, indexed fields, device-size requirements, CPP and PPO counts, and storefront localization behavior all change over time. When precision matters, check the current App Store Connect UI and Apple's official docs rather than trusting the numbers here.
- Ranking and conversion also depend on things this skill doesn't cover, because they aren't metadata: ratings and reviews, retention, crash and hang rates, and off-store presence. Keep them in mind, but handle them in their own phases.
- This skill is guidance only. It needs no API key and no network access.
