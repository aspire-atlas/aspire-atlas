---
name: atlas-creators-profile
description: Build a brand's creator profile during Aspire Atlas onboarding — confirm the existing partner roster detected from the Atlas index (exact handles), snapshot each partner, capture the escalation chain for off-brand content, and define the brand's ideal-creator spec for discovery. Use this whenever a brand mentions its creators, partners, ambassadors, or UGC program during onboarding, asks "who should we work with," wants creator discovery or vetting set up, or when the brand-safety or competitor skills need to know who the brand's partners are. This skill is the biggest fork in Atlas onboarding — run it for every brand, roster or not.
---


<!-- connector-attribution -->
> **Where these tools come from:** the bare tool names below (`fetch_account`, `search_posts`, `start_business_discovery`, …) are **Aspire Alpha connector** tools; `project_*` are **Claude Project document** tools, not Aspire Alpha ones.
> If a session has another connector exposing similarly-named tools (`list_orgs` vs `list_my_organizations`, `list_profiles` vs `list_my_profiles`), they are different servers with different argument shapes — do not substitute one for the other.

# Atlas Creators Profile

Whether a brand has a partner roster is **the single biggest fork in Atlas onboarding.** A brand with a roster gets monitoring, drift detection, and disclosure checking — the recurring, highest-retention jobs. A brand without one gets discovery and vetting. Same product, different brief. This skill detects which brand this is, confirms it, and captures both sides: who they work with today, and who they should work with next.

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

One project doc: `profiles/{brand-slug}/creators-profile.md` — the confirmed roster with per-creator snapshots, the escalation/cadence config extracted from one deep probe, and an ideal-creator spec written in intent and life-stage terms (not follower bands).

## When this runs

**On demand — never at onboarding.** Build this profile the first time a question actually needs it: a creator discovery ask, a vetting request, "who's talking about us." Need is the trigger, never completeness. Like every Atlas profile, it's a living document: start with what research answers (provenance-marked), draw at most 1–2 library questions at natural moments, and enrich silently whenever work surfaces an answer.

## Operating principles

Same rules as every Atlas profile skill: pre-fill from the Aspire Alpha MCP and the project before asking anything; size the question count by what pre-fill left open — a well-detected roster needs only confirmation plus a deep probe or two, a brand the index can't read earns more questions; offer options, welcome prose — present every question (probes included) as a select with pre-drafted candidate answers plus an own-words escape, and extract structure from any prose given; read `profiles/{brand-slug}/` first (the brand profile supplies category, themes, and business context — don't re-ask); write back with `project_write`; state the index's current platform coverage out loud (it's expanding — check what's live, and bridge uncovered platforms with web research, labeled as such); never let an unanalyzed creator read as safe.

## Step 1 — Detect the roster before asking about it

- `search_posts` for posts mentioning or tagging the brand's handle; look for creators doing it **regularly** — repeat mentions are the partner signal, a single tag is noise.
- `search_creators` / `fetch_account` on each candidate: themes, partnership history (`pastBrandPartnershipPartners` — note it's exact-match and often null), whether their content has been analyzed, and the safety verdict if it has.
- **Capture handles exactly as they appear in the index.** Exact-match fields return a silent zero on a casing mismatch — `Aspire` vs `ASPIRE` reads identically to "no partners." Getting exact strings is not a nicety; it is the difference between a real answer and a confident zero.
- Check the project for prior shortlists and creator work on this brand.

## Step 2 — The interview

**Confirm 1 · The roster.** Pre-filled:
> "We see {N} creators tagging or mentioning you regularly: {list with handles}. Are these your partners? Anyone missing?"

Additions from the brand: get the **handle**, not just the name. If they name someone not in the index, queue ingestion (`start_business_discovery`) and say the snapshot will follow — never fake one.

**Confirm 2 · Required partner hashtag** (only if a roster exists and the brand-safety profile hasn't captured it): pre-fill from the most frequent branded hashtag on partner content. This one field unlocks the cheapest credible win in the product — the disclosure-gap check.

**Deep probes:**

- **P1 · The escalation chain** *(roster brands)* — this is the highest-yield question in Atlas onboarding:
  > "I can see {N} creators tagging you regularly. Walk me through what happens when one of them posts something off-brand. Who spots it, who decides, and how fast does that need to happen?"

  One answer yields four config values: roster confirmation pressure-test, interrupt-vs-digest policy, severity threshold, and who-sees-what routing. If a user profile already captured routing, skip the routing part and probe only speed and severity.

- **P2 · The ideal creator.** Pre-fill this one from evidence before asking: search the index for creator posts mentioning or tagging the brand, and rank them by engagement **relative to each creator's own baseline** (a small creator's breakout post is a stronger signal than a big creator's average one — raw likes mislead). Describe the top 1–3 — who they are, what the post was, how far it over-performed, what themes/aesthetic carried it — and ask the brand to confirm and explain:
  > "This post by @{creator} about you did {N}× their usual engagement ({link}). Is this the kind of partnership that works for you? What made it work from your side? And describe the customer moment you want this content to live in."

  The brand reacting to real posts yields a sharper spec than the brand generalizing from memory — and it's another moment where Atlas shows its work. **Fallback** when the index finds no brand mentions (common for brands whose creator activity lives on a platform not yet indexed): ask the open form — "Tell me about the partner (or the piece of creator content) that worked *better than you expected* — what made it work?" — and note that the evidence-first version kicks in as coverage fills.

  Extract the spec in the terms discovery actually searches on: **intent and life-stage segments** ("2027 brides," "dads cooking for their family," "new-apartment movers"), content themes, aesthetics, energy — and dealbreakers. Not one of 111 real customer asks ever requested a follower-count range or engagement-rate floor; if the brand volunteers one, record it, but never ask for it.

**No-roster fork:** skip Confirm 2 and P1, run P2 plus one probe on how they'd vet ("walk me through how you'd decide someone is safe to sign — what would you check, and who signs off?"). Say plainly that their Atlas brief will be discovery-led until partners exist.

## Step 3 — Write the profile

`profiles/{brand-slug}/creators-profile.md`:

```markdown
# Creators Profile for {Brand}
*Atlas onboarding · {date} · mode: {roster / discovery-led} · roster size: {N}*

## Confirmed roster
| Creator | Handle (exact) | Themes | Read closely? | Safety verdict | Notes |
|---|---|---|---|---|---|

## Not yet read closely
{Creators Atlas can't call either way. The honesty card. Coming up: …}

## Escalation & cadence config (from the brand's own words)
- Who spots · who decides · required speed: …
- Interrupt vs digest policy: … · Severity threshold: …
- Required partner hashtag / disclosure: …

## Ideal-creator spec
- Intent & life-stage segments: …
- Content themes & aesthetic: … · Energy / voice: …
- Proven-it-works evidence: {the partner or post they cited, and why}
- Dealbreakers: …

## Limits & gaps
{Platforms I cannot read yet, bridged via web research; share of the roster I have not gone through; caveats on how much I could see}

## What this unlocks
{Roster mode: disclosure-gap check, 24h go-live tracking, sentiment early-warning.
Discovery mode: whitespace search, semantic shortlists, screening gate.}
```

The "Not yet read closely" section is load-bearing: a verdict-less creator listed as fine is the worst failure mode this skill has. Show what Atlas doesn't know as visibly as what it does.

## Step 4 — Close

Summarize the fork out loud ("you're a roster brand — the monitoring jobs are yours from day one") and name the first concrete thing Atlas will catch — for roster brands that's usually a disclosure gap, which is deterministic and checkable. If invoked by a hub, return the doc path, the mode, and the extracted config values.
