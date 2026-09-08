---
name: atlas-competitor-profile
description: Build a brand's competitor profile during Aspire Atlas onboarding — confirm the competitor set with exact handles and casing variants, capture why each competitor actually matters, and run a first competitive read (creator overlap, dominant themes, shared hashtag territory) from the Atlas index. Use this whenever a brand names competitors, asks "what are competitors doing," wants competitive intelligence, share of voice, or "creators who work with X but not us" set up during onboarding, or when the brief needs a competitor set and none exists. The competitor set is the routing key for the entire competitive-intelligence family — get it early.
---


<!-- connector-attribution -->
> **Where these tools come from:** the bare tool names below (`fetch_account`, `search_posts`, `start_business_discovery`, …) are **Aspire Alpha connector** tools; `project_*` are **Claude Project document** tools, not Aspire Alpha ones.
> If a session has another connector exposing similarly-named tools (`list_orgs` vs `list_my_organizations`, `list_profiles` vs `list_my_profiles`), they are different servers with different argument shapes — do not substitute one for the other.

# Atlas Competitor Profile

The competitor set is the routing key for every competitive question Atlas can answer — who they poach creators from, whose themes they're losing to, whose hashtag territory they share. And it carries the most dangerous data caveat in the product: partnership-history fields are **exact-match**. Query `Aspire` when the index says `ASPIRE` and you get a silent zero that reads identically to "no partners." This skill exists to get the exact strings from a human once, so every future answer is real.

## Test mode — block silently, stay in character

If `atlas-session-mode` set a test **or demo** mode, **the writes in this skill do not fire** —
but the step still runs in full. Make the decision, show the reasoning, present
whatever card the real flow presents. Only the final call is skipped, and a
customer never sees that call anyway.

**Never narrate the test.** No "would have applied X", no "blocked — test run", no
explaining what is and isn't real. That commentary is what ruins the run being
measured. Add the skipped call to the session ledger and carry on.

**Never claim a state change that didn't happen.** Phrase it as the decision —
*"here's what I'd switch on"* — which is what the real flow says at that point
anyway. Docs go to `sandbox/`, never `profiles/`.

See `${CLAUDE_PLUGIN_ROOT}/skills/atlas-session-mode/references/write-boundaries.md`.

---

## What this produces

One project doc: `profiles/{brand-slug}/competitor-profile.md` — the confirmed set with exact handles and match strings, why each competitor matters (in the brand's words), a first competitive read per competitor, and hashtag-watchlist recommendations.

## When this runs

**On demand — never at onboarding.** Build this profile the first time a competitor question shows up: "what are our rivals doing," a benchmark ask, a competitive report. Need is the trigger. Like every Atlas profile it's a living document: infer the set from research (provenance-marked), confirm with one click when the moment is natural, enrich silently as work surfaces answers.

## Operating principles

Same rules as every Atlas profile skill: pre-fill before asking; size the question count by what pre-fill left open — a confidently inferred set needs one confirm, a fuzzy one earns more digging; offer options, welcome prose — present every question (probes included) as a select with pre-drafted candidate answers plus an own-words escape, and extract structure from any prose given; read `profiles/{brand-slug}/` first (the brand profile supplies category and positioning — the seed for competitor inference); write back with `project_write`; never invent a number, and label anything the index can't yet answer as "coming" rather than improvising.

## Step 1 — Infer the set before asking

- **From the brand profile:** inferred category and positioning → candidate competitors.
- **From the index:** `search_posts` for `featuredBrands` co-occurring with the brand's category content; scan `pastBrandPartnershipPartners` values on creators in the category (this also harvests the *actual casing variants in the data* — record every variant you see).
- **From the web:** named competitors in press, "vs" searches, retail shelf-mates.

Rank candidates. You should arrive at the interview with a pre-filled set of 3–5.

## Step 2 — The interview

**Confirm 1 · The set.** Pre-filled:
> "Based on your content and category, we think your competitors are **{X, Y, Z}**. Right? Anyone missing, and anyone here you don't actually compete with?"

For every confirmed competitor, capture the **handle on each platform the index covers**, not just the name. The name fills the exact-match fields; the handle fills content queries. Both, always.

**Confirm 2 · Tiering** (only if the set is >3): "Which one matters most right now?" — one tap, sets the ranking the brief leads with.

**Deep probe:**

- **P1 · The last loss.** Competitor lists are cheap; mechanics are signal:
  > "Walk me through the last time {top competitor} actually cost you something, whether that was a creator who went with them, a shelf placement, or a customer segment. How did that play out?"

  Extract: which competitor is a real threat vs a name they track out of habit, *what kind* of loss matters (creators? distribution? attention?), and therefore which competitive questions belong in their brief. A brand that loses creators wants overlap lists; a brand that loses shelf placement wants share-of-voice; those are different briefs.

## Step 3 — First competitive read (Tier A only — answerable today)

For each confirmed competitor, run against the index and report only what returns real evidence:

- **Creator overlap** — creators with the competitor in `pastBrandPartnershipPartners` but no history with this brand. Query **every casing variant** captured in Step 1. This ends in a target list — the highest-intent output onboarding can produce.
- **Dominant themes & aesthetics** in the competitor's creator content right now (normalize aesthetic-tag casing before counting — `Modern` and `modern` are one theme).
- **Shared hashtag territory** — hashtags both brands' content lives in. Where a share-of-voice comparison would need a hashtag watchlist, recommend the specific `add_hashtags` calls and offer to make them; week-over-week SOV starts counting only once the watchlist exists, so say that.

Two query rules that prevent confidently wrong answers: exclude the brand's own posts when searching competitor names in text (their own marketing mentions competitors — filter `author.username`), and a zero result on an exact-match field means *check casing variants* before it means "none."

## Step 4 — Write the profile

`profiles/{brand-slug}/competitor-profile.md`:

```markdown
# Competitor Profile for {Brand}
*Atlas onboarding · {date} · set size: {N} · primary: {competitor}*

## The set
| Competitor | IG handle | Exact spelling seen in your posts | Tier | Why they matter (brand's words) |
|---|---|---|---|---|

## What losing looks like
{From P1. The kind of loss that hurts, and what that implies the brief should watch}

## First read
### {Competitor 1}
- Creator overlap: {N creators found who worked with them and never with us} {links}
- Dominant themes/aesthetics: …
- Shared hashtag territory: …
{repeat per competitor; where a read returned nothing, say "nothing I can see" rather than "none exist"}

## Watchlist recommendations
{Specific hashtags to track for how much of the conversation is theirs, and whether they were added}

## Limits & gaps
{Casing variants covered; competitors I cannot read yet (coming);
the conversation share needs a watchlist and time, so "I'll start counting once
the watchlist is added"}

## What this unlocks
{e.g., weekly overlap-target list; competitor theme watch; conversation share once the watchlist matures}
```

## Step 5 — Close

Lead the close with the most surprising thing the first read found — a real number with links ("14 creators have worked with {competitor} and never with you") is the aha this profile can deliver on day one. Be equally plain about what starts counting tomorrow (deltas, SOV) — the second brief is worth more than the first, and saying so makes the return visit the point. If invoked by a hub, return the doc path, the confirmed set with exact strings, and the watchlist state.
