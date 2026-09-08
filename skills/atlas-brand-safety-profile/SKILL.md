---
name: atlas-brand-safety-profile
description: Build a brand's safety standard during Aspire Atlas onboarding and actually apply it — interview for posture and real red lines, write the brand-safety standard document, configure the GARM category ceilings via the Aspire Alpha MCP (set_brand_safety_config), and verify the config changes screening output. Use this whenever a brand mentions brand safety, vetting, screening creators, risky content, GARM, disclosure/FTC compliance, or "what we won't be associated with" during onboarding, or when a vetting or monitoring job is picked and no safety profile exists yet.
---


<!-- connector-attribution -->
> **Where these tools come from:** the bare tool names below (`fetch_account`, `search_posts`, `start_business_discovery`, …) are **Aspire Alpha connector** tools; `project_*` are **Claude Project document** tools, not Aspire Alpha ones.
> If a session has another connector exposing similarly-named tools (`list_orgs` vs `list_my_organizations`, `list_profiles` vs `list_my_profiles`), they are different servers with different argument shapes — do not substitute one for the other.

# Atlas Brand Safety Profile

Brand safety is asked twice by every customer, and the second ask is the valuable one: first a **pre-partnership gate** ("is this creator safe to sign?"), then **post-signing drift** ("has anyone I already work with moved into new risk?"). This skill sets up the standard both run on — and it doesn't stop at a document. A safety config that never changes a screening outcome is decorative, so this skill ends by **applying the config and proving it flips a flag.**

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

1. A project doc: `profiles/{brand-slug}/brand-safety-profile.md` — the standard: posture, per-category ceilings with rationale, brand-specific red lines, disclosure rules.
2. **Applied config** via `set_brand_safety_config`, with a recorded verification that the ceiling actually changes output.

## When this runs

**On demand — never at onboarding.** Build this profile the first time safety actually enters the work: a vetting ask, a risky post, a monitoring request, or the brand raising it. Need is the trigger. Step 0's care-gate still applies at that moment; and like every Atlas profile this is a living document — category defaults first, 1–2 library questions at natural moments, silent enrichment as work surfaces answers.

## Operating principles

Same rules as every Atlas profile skill: pre-fill before asking; size the question count by what pre-fill left open — category defaults and sibling profiles usually answer most of it, so ask only what changes the config; offer options, welcome prose — present every question (probes included) as a select with pre-drafted candidate answers plus an own-words escape, and extract structure from any prose given; read sibling profiles in `profiles/{brand-slug}/` first (the brand profile supplies category — which sets posture defaults; the creators profile may already hold the disclosure hashtag); write back with `project_write`. And the two safety-specific absolutes: **an unenriched post must never read as safe** (a `pass` with no analysis behind it is the highest-severity failure this product can emit), and the config must be **live, not decorative**.

## Step 0 — First, find out if they care at all

Not every brand manages brand safety, and interviewing one that doesn't is wasted trust. Before anything else, read the signals already available: did they pick a vetting or monitoring job, or mention risk, screening, or "content we can't be near"? Does a sibling profile hold an escalation chain (a brand that described one cares by definition)? Is their category one where safety is structural (kids/family, health, regulated products)? If the signals clearly say they care, proceed. If the signals are absent or ambiguous, spend **one cheap question**:

> "Is brand safety something you actively manage, meaning creators you'd never sign and content you can't be associated with? Or is it not a real concern for you today?"

**If they don't care: skip the rest of this skill.** Don't apply config, don't push the interview. Write a three-line stub to `profiles/{brand-slug}/brand-safety-profile.md` recording that safety was deliberately skipped on {date} at the brand's call (so the hub and future sessions read a decision, not a gap), note that category-default posture can be switched on anytime, and stop. If they care — even mildly — continue below, and let the strength of their answer set how deep the interview goes.

## Step 1 — Pre-fill

- `get_brand_safety_config` — the current state, if any.
- **Category defaults** from the brand profile: an alcohol brand loosens drugs/alcohol-adjacent categories; a kids/family brand tightens debated-social-issues; a supplements brand watches health claims. Surface the **3–4 GARM categories that actually matter for their category** — never present twelve sliders.
- `search_posts` over their own and partner content: any existing flags, and the most frequent branded hashtag on partner content (pre-fills the disclosure question).
- Project docs: prior safety standards for this or similar brands are format and content precedent.

## Step 2 — The interview

**Confirm 1 · Posture preset.** "How strict should we be — **Standard · Strict · Custom**?" with the 3–4 category callouts for *their* category inline ("for a family food brand we'd tighten {X} and {Y} by default — sound right?"). Custom opens the full category list; the default path never shows it.

**Confirm 2 · Disclosure & required hashtag** (skip if the creators profile captured it). Pre-filled:
> "Partners seem to use {#brandpartner}. Is that required? And what's your disclosure rule: #ad, #sponsored, both, or your own?"

Cheapest credible win in the product: deterministic, checkable, and Atlas reads disclosures burned into the video frame, which caption-only tools miss.

**Deep probes:**

- **P1 · The wince.** Real red lines come from incidents, not category checklists:
  > "Tell me about the last time creator content made you wince, or caused a real problem. Walk me through what happened and what you did about it."

  Extract: the categories they *actually* fear (often not the GARM defaults — e.g., "creators making health claims we can't back"), true severity ("annoyed" vs "retailer called us"), and any brand-specific red lines the GARM taxonomy doesn't name. Write those as explicit rules in the standard.

- **P2 · The escalation chain** — *only if no creators profile exists*; otherwise reuse its answer. "When something breaches the standard — who spots it, who decides, how fast?"

## Step 3 — Draft the standard, confirm, then apply

Draft `profiles/{brand-slug}/brand-safety-profile.md`:

```markdown
# Brand Safety Standard for {Brand}
*Atlas onboarding · {date} · posture: {Standard/Strict/Custom} · config version: {v}*

## Posture & rationale
{One paragraph: who this brand is and why this posture}

## Category ceilings
| GARM category | Ceiling | Why this level for this brand |
|---|---|---|
{Only categories that deviate from the preset need a row with rationale}

## Brand-specific red lines
{From P1. Rules the GARM taxonomy doesn't cover, in plain language. Each one must
be testable against a real post, so write it so a specific post can pass or fail it}

## Disclosure & compliance
- Required disclosure: … · Required partner hashtag: …
- Checked in captions AND on-screen overlay text

## Escalation
{Who spots · who decides · speed. From P2 or the creators profile}

## Applied config record
- Applied: {date} · version: {v}
- Verified: {post link}. The verdict moved from {x} to {y} under your new standard ✓
- Use this form instead when no post sits on the boundary: Not verified yet. The
  closest evidence I found is {post link}, and it does not sit on the boundary.

## Known limits
{Share of relevant content I have not gone through yet; platforms I cannot read yet, bridged via web research; defects to caveat}
```

Show the brand the draft standard and get an explicit yes **before** applying — this config will block and flag creators; it should never change silently. Then call `set_brand_safety_config`.

## Step 4 — Verify the config is live

Screen a real post (a recent partner post, or a borderline one found in Step 1) against the new config. The test: **does the new ceiling change the outcome relative to the default?** Tighten a category and the flag must flip on content that sits at the old boundary. If you can't find a post that demonstrates the boundary, say so and show the nearest evidence — never claim verification you didn't perform. Record the result in the doc's Applied Config section.

While verifying, surface the analyzed-vs-not split for their roster/context: "of your {N} partners, {M} have analyzed content; the rest can't be called either way yet." That sentence prevents the worst failure mode this feature has.

## Step 5 — Close

One line on what's now standing guard ("every partner post now screens against this standard; breaches at {severity} interrupt {person} per your escalation chain") and one line on what's coming vs not yet (drift monitoring over time, if not yet shipped, is labeled "coming" — not improvised). If invoked by a hub, return the doc path, config version, and verification result.
