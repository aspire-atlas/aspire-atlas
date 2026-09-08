---
name: atlas-goals
description: >-
  Step 4 of Atlas Phase 1 calibration — turn a brand's 90-day and two-year goals
  into measurable alignment targets that every future brief filters through.
  Invoke from the `aspire` router after the brand interview, or on "set my goals",
  "what should we be aiming for", "alignment targets", "what are we measuring".
  Drafts candidate goals from the brand profile and index rather than asking cold,
  restates each in terms Atlas can actually observe, names what it cannot measure,
  and records which profile skill each target depends on so the right one is
  pulled when it is needed. Two questions, two minutes.
metadata:
  phase: "Phase 1 · Calibrate — Step 4 (min 9–11)"
  artifact: "Alignment targets"
---


<!-- connector-attribution -->
> **Where these tools come from:** the bare tool names below (`fetch_account`, `search_posts`, `start_business_discovery`, …) are **Aspire Alpha connector** tools; `project_*` are **Claude Project document** tools, not Aspire Alpha ones.
> If a session has another connector exposing similarly-named tools (`list_orgs` vs `list_my_organizations`, `list_profiles` vs `list_my_profiles`), they are different servers with different argument shapes — do not substitute one for the other.

# Step 4 — Goals

**Target: minute 9 to minute 11.** Two questions. Feeling to produce: **focused.**

Focus is created by **subtraction**. A filter is defined by what it excludes, so
this step ends by naming what Atlas will now stop showing them — not by listing
everything it could do.

**Two horizons — but you do not ask both with equal weight.** Ask the one the
person actually owns, properly. Derive or inherit the other.

| | Horizon | What it does |
|---|---|---|
| **90 days** | near | **The filter.** Decides what ships in every brief. |
| **2 years** | far | **The compass.** Decides what Atlas watches for early. |

---

## How to talk to them — read this first

**Read `${CLAUDE_PLUGIN_ROOT}/skills/plain-english/SKILL.md` before your first
reply, every session.** It is the one file governing how Atlas writes. It covers
three things that each break output on their own:

- **Stance.** Lead with what is working. Gaps are headroom, not errors. **The brand
  owns the wins; the work owns the shortfalls**, so never make a person the subject
  of a failure. Be opinionated about the recommendation, never about their
  judgment. No cleverness at their expense, and no urgency built on fear.
- **Vocabulary.** Never say "index", "indexed", "corpus", "coverage", "analyzed", a
  tool name, or a backend slug. Use the brand's real name and plain words for what
  you can and cannot see. Never narrate your own instructions, and never number the
  flow to the user. Be honest; don't announce that you're being honest.
- **Mechanics.** No em dashes, ever. A headline is one complete phrase a person
  would say out loud. Full sentences in body text. One idea per sentence.

---

## Who you are talking to — and which horizon is theirs

**Atlas users are marketing-team people, and seniority decides which question is
real for them.** Asking an IC to set a two-year company strategy makes them invent
an answer. Asking a CMO for a quarterly task list gets you something vague,
because it is not the altitude they think at.

| Role | Ask in depth | Take lightly or inherit |
|---|---|---|
| **IC** — influencer/creator marketer, performance/paid, social & content | **90 days.** The thing they own and are measured on. 3–4 drafted candidates, pick one, make it concrete. | **2 years:** never ask them to speak for the company. Take it from the brand profile marked as your own read, or ask one soft question — *"what's the longer-term thing your team is pointed at?"* — and record it `(inferred, unconfirmed)`. |
| **Manager / team lead** | **Both**, and both are legitimately theirs. 90-day in depth; two-year scoped to their function rather than the whole company. | — |
| **Leadership** — CMO, founder, head of marketing | **2 years.** The strategy they actually own, asked straight. | **90 days:** derive it. *"What has to be true this quarter for that to be on track?"* It is the near-term expression of the two-year, not an independent question. |
| **Agency** | **90 days per client**, plus the renewal horizon — that is the real deadline. | Company strategy belongs to the client, not to them. Never ask them to speak for it. |

**The two roles invert.** For an IC the 90-day target is both the filter and the
thing they care about, and the two-year is context they inherited. For leadership
the two-year is the real answer and the 90-day falls out of it. Get this backwards
and the whole step feels like it was written for someone else.

A performance marketer wants to know what this does to their CAC. A social manager
wants to know what to post next. A brand lead is afraid of partner content blowing
up. A founder wants to know if any of it is working. **Same targets, different
entry point** — draft the candidates from the role captured earlier.

**If the role is not on file, do not guess the altitude.** Ask the role first. A
mis-aimed goal question is worse than one extra question.

## Step 1 — Draft before you ask

Never open with *"what are your goals?"* — the brand profile skill bans the
abstract version for good reason: it produces aspirational noise. Arrive with
candidates.

Read first: `profiles/{brand-slug}/brand-profile.md` (business context, the 6–12
month outcome if research found one, seasonal calendar, biggest threat),
`profiles/{brand-slug}/user-profile--{user-slug}.md` if it exists, the coverage
stamps from Step 2, and the job they named in Step 3.

That job is usually the 90-day goal already, said out loud in different words.
**Start there.** *"You came here because vetting a shortlist takes too long. Is the 90-day
goal to sign good creators faster, without any safety surprises?"*

Present both questions as selects with 3–4 drafted candidates and an always-open
own-words escape. Typing is the tax that kills completion. Two minutes means two
taps and maybe one sentence.

See `references/alignment-target-library.md` for role-shaped candidates.

---

## Step 2 — Restate as an alignment target

This is the actual work of the step, and the place it can go badly wrong.

**A goal is not a target until Atlas can say what it will watch.** Every target
gets four fields:

```
Target      · the goal in the brand's own words
Observable  · what Atlas will actually watch, in index terms
Cadence     · interrupt · daily · weekly · on demand
Not covered · what this target implies that Atlas cannot measure
```

The `Not covered` line is mandatory and it is never empty. Skipping it is how a
brand comes to believe Atlas is measuring their revenue.

### What Atlas can and cannot see

**Do not restate a goal into a metric from the right-hand column.** Say plainly
that the outcome is theirs to measure and name the leading indicator Atlas can
watch instead.

| Observable today | **Not measurable — say so** |
|---|---|
| Creator and post content, themes, aesthetics | Revenue, ROI, conversions, CAC — **your store isn't connected** |
| Mentions and tagged posts on the brand | Purchase intent from comments — comments are not persisted |
| Disclosure presence, captions **and** on-screen overlay text | Audience authenticity / pod detection — **no such signal exists** |
| Competitor creator overlap (`pastBrandPartnershipPartners`, exact-match) | Pre-peak trend acceleration — no `date_histogram` |
| Hashtag share of voice — **only once a watchlist accrues time** | Audience demographics — **check the field first**, see note below |
| Engagement relative to a creator's own baseline | Drift since approval — no verdict history yet |
| Comment volume as a trend line | Platforms not yet live — check coverage, bridge with labelled web research |

⚠️ **Audience demographics are conditional, not absent.** `audienceDemographics`
is null for many accounts but **fully populated for some** — Minted returns age,
gender, city and country breakdowns (71.3% female, 44.6% aged 25–34, 49.7% US).
Read the field on the actual account before deciding. Refusing a goal Atlas could
serve is as damaging as promising one it cannot.

⚠️ **Never conflate `vibeAnalysis.authenticityScore` with audience authenticity.**
It scores *content* authenticity. A goal about fake followers cannot be served by
it, and letting a brand think otherwise is a serious misrepresentation.

**Name the missing connection, not the empty field.** *"I can't see your store"*
and *"your ad account isn't connected"* are plain and true. *"`app.*` is empty"* is
our plumbing leaking into their conversation. Where Step 2 captured what they sell
or whether creator content runs as paid, use it — a target aimed at products Atlas
knows about is sharper than one aimed at "your category".

**Restatement examples:**

> *"Grow revenue 30% from creator marketing."*
> → Target: revenue growth from creator marketing.
> → Observable: creators posting about the brand, their reach relative to their own
> baseline, competitor overlap converted to signed partners.
> → **Not covered: revenue attribution.** Atlas cannot see sales. Say it: *"you'll
> measure the revenue; I'll measure whether the creator side is moving in the
> right direction, and tell you which creators moved it."*

> *"Be the brand our category thinks of first in two years."*
> → Observable: how much of the category conversation is yours, theme ownership vs
> the competitor set, untagged category mentions.
> → **Not covered: brand awareness surveys, search volume.** And I start counting
> from the day the watchlist exists, so day one is a baseline, not a trend.

**Where a target needs a delta, say the second brief is worth more than the
first.** Four of six customers asked *"what changed?"* rather than *"what's
true?"* Saying that out loud makes the return visit the point.

---

## Step 3 — Route: which profile does each target need?

Every alignment target has a dependency. Record it — this is how the profile
skills get called **as needed** instead of all at once.

| The target is about | Needs | Because |
|---|---|---|
| Partners, ambassadors, UGC, roster, "who to sign" | `atlas-creators-profile` | Roster with exact handles + ideal-creator spec |
| Rivals, share of voice, benchmarks, "beating X" | `atlas-competitor-profile` | Competitor set with exact-match casing strings |
| Risk, compliance, FTC, "never associated with" | `atlas-brand-safety-profile` | Applied, verified safety config |
| Mentions, listening, "when people talk about us" | `atlas-competitor-profile` | Hashtag watchlist via `add_hashtags` |
| Reporting, "what my boss sees", routing | `atlas-user-profile` | Routing map + cadence policy |
| Positioning, category, brand story gaps | `atlas-brand-profile` | Enrichment pass on the anchor profile |
| Creative fatigue, "what should we make", which content wins | `atlas-creative-analyst` | Briefable pattern with an evidence bar |

**Record the dependency. Do not run them all.** Every one of those skills says
*on demand — never at onboarding; need is the trigger.* Firing five profile
interviews at minute 11 is precisely the onboarding-as-a-phase failure this flow
exists to kill.

Pull one inline **only** when the target cannot be honestly stated without it —
and then it is need-is-the-trigger firing, not a contradiction. Everything else
goes in the doc as a named dependency, so the next session reads a plan rather
than a gap.

**Respect recorded declines.** If `brand-safety-profile.md` is a three-line stub
saying safety was skipped at the brand's call, that is a decision. Do not
re-offer it.

---

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

## Step 4 — Write the targets

Write `profiles/{brand-slug}/alignment-targets.md` with `project_write`:

```markdown
# Alignment Targets for {Brand}
*Atlas Phase 1 calibration · {date} · set by: {name}, {role} · primary horizon: {90d | 2y}*

## The filter
Every brief card must serve a target below. A card that serves none does not ship.

## The 90-day target
**{Goal in their words}** *(confirmed | derived from the two-year | inferred, unconfirmed)*
- Observable: {what Atlas watches, said in plain terms}
- Cadence: {interrupt · daily · weekly · on demand}
- Not covered: {what Atlas cannot measure here. Never empty}
- Depends on: {profile skill, or "nothing further"}

## The two-year target
**{Goal}** *(confirmed by {role} | inherited, because {role} does not set company strategy, so this is my read)*
- Observable: … · What Atlas flags early: …
- Not covered: …
- Depends on: …

## What Atlas will now ignore
{The explicit exclusions. This is what makes the brief feel like theirs.}

## Dependencies not yet built
| Target | Needs | Owner skill | Status |
|---|---|---|---|
{Named, not silently skipped}

## Baseline at calibration
{How much I have read, and figures ALREADY on file from Steps 2–3. Real numbers only.}
```

**Never print a placeholder number in the baseline.** If coverage is still
filling, say so and record what is real.

**Do not run a deep index sweep to build the baseline.** Onboarding is web-first;
the index is an existence check. Reuse the coverage stamps Step 2 recorded and
whatever Step 3 already wrote. Deep index analysis belongs to the on-demand
profile skills and to post-onboarding jobs — an index-heavy calibration step is a
known failure mode, not thoroughness.

---

## Step 5 — Close by narrowing

One line on what now filters through, one line on what Atlas will stop showing
them, one line on what starts counting tomorrow. Then hand back to the router.

> Two targets are locked. Every brief filters through them, so you won't get trend
> roundups or follower-count rankings, because neither serves either target.
> Today's numbers are your baseline; from tomorrow they're a delta.

Then hand off to `atlas-alignment-snapshot` — **Step 5 is the aha, and it is where
onboarding ends.** Not the recurring brief: that is `atlas-brief`, on the cadence
Step 5 offers at the close. Do not produce the snapshot here and do not preview
its gap.

The 90-day target decides which gap Step 5 names, so make sure it is concrete
enough to aim at. A vague target produces a trivia snapshot.

Hand forward: the doc path, both targets, the cadence map, and the dependency
list — Step 5 turns those dependencies into the four passive channels it arms.
