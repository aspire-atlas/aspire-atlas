---
name: atlas-brand-profile
description: Build or refresh a brand's core profile during Aspire Atlas onboarding — brand summary, derived content context, business context, and voice & content operations. Use this whenever a brand starts Atlas onboarding, asks to "set up my brand," "build a brand profile," "tell Atlas about us," asks for a marketing strategy or brand context for a specific brand, or whenever any other Atlas profile skill (creators, competitor, brand-safety, user) needs brand context that doesn't exist yet. This is the anchor profile — but it builds in the BACKGROUND (research-only quick pass, no interview) and enriches incrementally across sessions; it is never an upfront onboarding step.
---


<!-- connector-attribution -->
> **Where these tools come from:** the bare tool names below (`fetch_account`, `search_posts`, `start_business_discovery`, …) are **Aspire Alpha connector** tools; `project_*` are **Claude Project document** tools, not Aspire Alpha ones.
> If a session has another connector exposing similarly-named tools (`list_orgs` vs `list_my_organizations`, `list_profiles` vs `list_my_profiles`), they are different servers with different argument shapes — do not substitute one for the other.

# Atlas Brand Profile

The load-bearing idea: **the profile is exhaust from doing work, not a prerequisite for it.** Every question you ever ask should be one you already have a guess for — Atlas showing its work, never handing out a form.

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

## When this runs (two modes)

- **Background quick pass (the default, at first contact via /aspire):** web research + index existence check only, NO interview, no user interruption. **The doc's "what I used" line must say "the web" alone on this path**, and it stays that way until the user has actually told you something that landed in the doc. One confirmed answer to a single library question is enough to make it "the web and what you told me"; a formal interview is not required, and there usually never is one. Get this wrong in either direction and the doc misstates where it came from. Write the doc as a research-only v0 with inferences provenance-marked *(inferred, unconfirmed)*. This runs while the user's first job is being delivered — they never wait on it.
- **Incremental enrichment (forever after):** the doc completes slowly across sessions. The interview content below is a question *library*, not a script — draw at most 1–2 questions into a session, at natural moments, only when the answer changes what Atlas does next. When the user says something in the course of any work that answers a library question, update the doc silently and mark it confirmed. Never run the library as a batch interview.

## What this produces

One project doc: `profiles/{brand-slug}/brand-profile.md` — the anchor record every other Atlas profile skill reads. Substantive sections: Brand Summary, Brand Context, Business Context, Voice & Content Operations, plus an honest limits section and a "what this unlocks" close. The Brand Summary is a short prose paragraph — what the brand is, what it stands for, how it differentiates, what it sells and where — assembled from research and from things learned naturally during work. No framework labels (no "golden circle," no why/how/what scaffolding).

## Operating principles (shared across all Atlas profile skills)

1. **Pre-fill before you ask.** Read the brand through the web first (with a quick existence check against the Atlas index). Never ask for a fact you can infer — confirming a guess is fast and demonstrates competence; asking cold is slow and demonstrates nothing.
2. **Research sets the question budget.** How many questions you ask — and how deep you go — depends on what pre-fill actually answered. A brand whose website, press, and index coverage tell the story clearly needs only a few one-click confirms plus a deep probe or two; a brand research can't read earns more questions. Never ask to fill a template — ask because research left a gap that changes what Atlas recommends. And prefer mechanic questions over fact questions: a deep probe names the brand's actual entities, asks for sequence ("walk me through..."), and asks how something works, not whether it exists. "Standard or Strict?" is plumbing — necessary, but not discovery.
3. **Offer options, welcome prose.** Typing is the tax that kills onboarding completion — never make an open question the only way in. Present every question, including deep probes, as a single/multi-select with pre-drafted candidate answers (your research is what makes plausible options possible) plus an always-available "in your own words" choice. Decompose a "walk me through" mechanic into 2–3 quick selects rather than demanding an essay. When the brand does type, treat it as a gift: extract every structured value the prose holds — one good answer should yield 3–4 profile fields, and nothing gets asked twice.
4. **Honesty is the product.** Never present a partial answer as complete, never invent a number, and never let unanalyzed content read as safe. Label inferences *(inferred)* and web-sourced material as web-sourced. If something can't be answered yet, say so plainly when it comes up — don't improvise it.
5. **Profiles persist in the Claude Project.** Before asking anything, `project_search` for `profiles/{brand-slug}/` — read sibling profiles and prior work on this brand, and never re-ask what a sibling already holds. Write your output back with `project_write`.
6. **Provenance.** Any claim about their content should be checkable — link real posts.

## Step 1 — Locate the brand

Identify the brand and its social handles (ask only if genuinely ambiguous). Slugify for paths (`Aspire` → `aspire`). Check the project for existing profiles and prior strategy work on this brand; anything found is pre-fill material and must not be re-asked.

## Step 2 — Pre-fill from the web (Atlas index: existence check only)

**Onboarding runs on web research, not index analysis.** Deep index work — themes, cadence, top performers, hashtag structure — belongs to the later profile skills and to post-onboarding jobs; during onboarding it is slow and the web already answers this profile's questions. Use the Aspire Alpha MCP for exactly one thing here: a **quick existence check** (`fetch_account` on the brand's handle) to record whether the brand is in the index. If it's absent, kick off `start_business_discovery` in the background so coverage fills while onboarding proceeds — don't wait on it, don't analyze it.

**Web search does the real work:** their site (about page), press, retail presence, reviews, founder interviews, and a surface read of their social presence. You're collecting who they are in their own published words, plus business facts (retail partners, launches, funding).

Compute a working read before asking anything: inferred category, positioning guess, business-context guesses, and a draft brand summary from their own published words.

## Step 3 — The question library (drawn from opportunistically — never a batch interview)

In background mode, skip this step entirely. In enrichment mode, pick the 1–2 highest-value open items for the current moment. Lead with confirms so the brand sees Atlas already did work before it's asked for anything.

**Confirms (pre-filled, one-click):**
1. **Category & positioning read-back.** "Here's what your presence says you are: {read}. Right?"

**Strategic facts: treat as known, don't ask.** The brand's purpose, positioning, and business direction are higher-management knowledge — assume they exist and are settled; your job is to find them in research (site, press, founder interviews) and in what users say naturally during work, and summarize them into the profile marked *(inferred)*. Confirm one only when two conditions BOTH hold: (1) the current work actually depends on getting it right, and (2) the user's role means they'd credibly know the answer — a founder/CMO can confirm positioning; don't ask a coordinator to speak for company strategy (for them, keep the research read and move on). When both hold, confirm it inline as one light select at a natural moment, never as a dedicated question block.

**Deep probes (adapt wording to what pre-fill surfaced; add or drop probes based on what research already answered). Present each probe as 2–3 quick selects with drafted options plus an own-words escape — the "walk me through" below is the mechanic to capture, not the literal delivery format:**

- **P1 · Business mechanics.** Anchor on the most consequential thing research surfaced, then ask for the mechanism, not the goal:
  > "The next 6–12 months seem to hinge on {e.g., renewing the big retail placement / the spring launch}. Walk me through how that actually gets decided. Who evaluates you, on what numbers, and when?"

  Yields: the real outcome variable, the deadline, the metric that matters, the threat. If research surfaced nothing, ask the open form: "What's the one outcome the next 6–12 months has to produce? And what actually determines whether it happens?"

- **P2 · Content operations.** 
  > "Walk me through how a piece of content gets made today. Who shoots it, who approves it, and how long from idea to posted?"

  Yields: in-house production capacity, voice ownership, approval chain, and how ambitious a content calendar can realistically be. This is the question that decides whether recommendations are $5k moves or $500k moves — you can also ask scale directly as a quick order-of-magnitude confirm if it hasn't emerged.

**Never ask:** their category cold (infer it), audience as a free-text description (they'll answer aspirationally and the field isn't searchable), follower/engagement thresholds (zero of 111 customer asks ever requested these), brand guidelines as pasted prose (you already read their content), which platforms (there's no choice to offer).

## Step 4 — Write the profile

Write `profiles/{brand-slug}/brand-profile.md` using this structure:

```markdown
# Brand Profile for {Brand}
*Atlas onboarding · {date} · handle: @{handle} · what I used: {the web / the web and what you told me} · what I can see: {N posts / nothing yet, just started reading}*

## Brand Summary
{One short prose paragraph: what the brand is, what it stands for, how it differentiates, what it sells and where. Assembled from their own published words and anything learned during work. Mark strategic claims *(inferred)* until a role-appropriate user confirms one in the natural course of work, never via a dedicated question.}

## Brand Context  *(found on the web at onboarding; numbers from your own posts land here on first refresh)*
- Positioning & inferred category: …
- Content presence at a glance: platforms, apparent themes, rough cadence (from public view, with links)
- Notable moments: launches, press, viral moments, with sources
- What I can see: {N posts I can read / just started reading, so the numbers will fill in}

## Business Context
- The 6–12 month outcome and its mechanics: {from P1. Who decides, on what, when}
- Scale & constraints: …
- Seasonal calendar / upcoming moments: …
- Retail & channel relationships: …
- Biggest threat: …

## Voice & Content Operations
- Tone of voice: … | Content pillars: …
- Production capacity & approval chain: {from P2}
- Owned channels: …

## Limits & gaps
{Data gaps about the BRAND, framed neutrally: what's not yet measured, what came from the web rather than their own posts, unanswered questions, fields that need owner auth. Never frame this section as Atlas product limitations or roadmap. "I found the TikTok side on the web for now" belongs here; "Atlas doesn't support TikTok yet, coming soon" does not. Questions about how much Atlas can see get answered in conversation if the user raises them, never volunteered in the doc.}

## What this unlocks
{2–3 brief questions Atlas can now answer for this brand, each ending in an action.
e.g., "where is {category} showing up organically in creator content not tagged for it?"}
```

Every derived claim gets a post link where possible. Every gap is named, not papered over.

## Step 5 — Close honestly

Present the profile with a one-line summary. Tell them what got measurably better because they answered (the config extracted from their prose), what Atlas still can't see, and that the profile compounds — the next profile skill (creators, competitor, brand safety) starts from here instead of from zero. If a hub/super-agent invoked this skill, return the doc path and the extracted structured fields rather than re-presenting everything.
