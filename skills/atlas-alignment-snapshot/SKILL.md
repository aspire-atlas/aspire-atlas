---
name: atlas-alignment-snapshot
description: >-
  Step 5 of Atlas Phase 1 calibration — the aha. Builds the Alignment Snapshot:
  what the brand SAYS, what creators SHOW, what consumers RESPOND to, the gap
  between them named in one sentence, and three aligned creators with evidence.
  Invoke from the `aspire` router after alignment targets are set, or on "show me
  the snapshot", "what's my alignment", "run the mirror", "show me the gap".
  Ends Phase 1 by arming the four always-on passive channels and going quiet.
  Never a report — a mirror the brand has never been held up to before.
metadata:
  phase: "Phase 1 · Calibrate — Step 5 (min 11–15), then hand off to Phase 2"
  artifact: "Alignment Snapshot"
---


<!-- connector-attribution -->
> **Where these tools come from:** the bare tool names below (`fetch_account`, `search_posts`, `start_business_discovery`, …) are **Aspire Alpha connector** tools.
> If a session has another connector exposing similarly-named tools (`list_orgs` vs `list_my_organizations`, `list_profiles` vs `list_my_profiles`), they are different servers with different argument shapes — do not substitute one for the other.

# Step 5 — The Aha

**Target: minute 11 to minute 15.** Feeling to produce: **the wow moment.**

This is the end of onboarding. **Not the recurring brief** — that is
`atlas-brief`'s job, on a cadence this step offers at the close. Today's job is a
mirror: the first time this brand has seen itself, its creators, and its audience
in one frame.

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

## Why this is the aha and not a report

Atlas's whole thesis is that it aligns **Brand · Creators · Consumers**. The three
panels are that thesis, instantiated on their own data:

| Panel | Thesis | The question it answers |
|---|---|---|
| **SAY** | Brand | What do you claim to be? |
| **SHOW** | Creators (Content) | What does the market actually show of you? |
| **RESPOND** | Consumers | What do people actually react to? |

A brand can see any one of these already. **None of them can see all three at
once** — which is why the gap between them is the thing worth paying for, and why
this lands as a mirror rather than a dashboard.

**The gap is the aha. The panels are only the setup.** If you produce three tidy
panels and no gap sentence, this step has failed even if every number is right.

See `references/say-show-respond.md` for how to build each panel from real fields.

---

## Step 1 — Build the three panels

Everything here runs on **targeted index reads** scoped to this one question —
`fetch_account`, `search_posts`, `search_creators`, `search_creator_marketplace`.
This is **not** the deep sweep the profile skills own. Do not build a creators,
competitor or safety profile to produce this snapshot.

Read `profiles/{brand-slug}/brand-profile.md` and `alignment-targets.md` first.
The 90-day target decides which gap is worth naming — a snapshot that names a gap
nobody is trying to close is trivia, not an aha.

**Every claim carries a link.** A number without a post behind it is exactly the
kind of thing this product exists to replace.

### When SHOW comes back empty

Common, and not a failure — many brands arrive with zero creators posting about
them on a covered platform. **Bridge with clearly labelled web research.** Search
for creator activity on platforms not yet in the index, name the platform and
label it web-sourced, and let the panel be real rather than blank.

Yough is the worked case: zero third-party Instagram posts, while its actual
creator advocacy was TikTok-native in-aisle discovery videos. The Instagram-only
read would have said "nobody is showing you," which was **false** — the activity
was simply somewhere Atlas could not yet see. Labelled web research is what keeps
the mirror honest.

If web research also finds nothing, say so plainly and make the absence the
finding — then fill the panel with category whitespace: who is already making
content this brand's buyer watches, without mentioning them.

---

## Step 2 — Name the gap

One sentence. Concrete, evidenced, and **surprising but true.**

> You say high-protein and clean-label; creators show you as an in-aisle Target
> discovery; and your audience responds ~3× harder to retail news and community
> prompts than to product shots.

That sentence does three things at once: it proves Atlas read all three panels,
it tells them something they did not know, and it implies the action without
prescribing it.

**Rules that keep it credible:**

- **The gap must be falsifiable.** If the brand can't check it against something
  they already know, it reads as horoscope. Link the evidence.
- **Never manufacture a gap.** If all three panels genuinely align, say *that* —
  "your story, your creators and your audience are pointing the same direction;
  here's the one place they diverge" is a real and rarer finding.
- **Do not stack gaps.** One named gap lands; three named gaps is a report.
- **A gap is not a criticism, and this is the rule most often broken.** Name the
  misalignment, not the mistake. These are marketers looking at their own creative
  choices, often with their leadership reading over their shoulder.

  > ❌ *"You market chemistry. Your creators and your audience both talk about
  > routine."* — reads as *you got this wrong.*
  >
  > ✅ *"Your audience has already told you what they want more of, the routine.
  > Your creators are making it. There's clear room to meet them there."* — same
  > facts, reads as an opening.

  **Test it before you write it down:** read the sentence as the person whose work
  it describes. If it makes them look careless, rewrite it. The goal is a page they
  would forward.

---

## Step 3 — Three aligned creators

Three, not ten. **Aligned means aligned across all three panels:** they already
make content in the brand's territory (SAY), they are already showing it or
showing the category (SHOW), and their audience reacts the way this brand's
audience reacts (RESPOND).

**Rank by over-performance relative to each creator's own baseline** — a small
creator's breakout post is a stronger signal than a big creator's average one.
Raw follower counts mislead, and zero of 155 customer asks ever requested a
follower range.

Each creator gets: handle **exactly as the index holds it**, why they are aligned
in one line, the evidence post with a link, and whether their content has been
analyzed.

⚠️ **An unanalyzed creator must never read as safe.** If the content behind a
recommendation has no analysis, say so on the row. A verdict-less creator
presented as fine is the worst failure this snapshot can produce.

---

## Step 3b — Three to five supporting insights

**One headline gap, then the insights that sit under it.** The gap is what lands;
the insights are what makes it credible and actionable.

The 1K brief specifies "three to five insights that are specific, evidence-backed,
and tied to a decision." That does not contradict the one-gap rule — it completes
it. Lead with the gap, then carry three to five findings beneath it. Each one must:

- be **specific** — a number and a name, never a category
- be **evidence-backed** — a link to the post behind it
- **end in a decision** — if a finding implies no choice, cut it

**Do not pad to five.** Three real insights beat five where two are filler, and a
filler insight is the fastest way to make the other four look invented.

If a finding is interesting but you cannot tie it to a decision, it belongs in the
"what Atlas could not see" section as an open question instead.

---

## Step 3c — The recommended first action

**This is where onboarding used to stop, and stopping here is the thing the brief
rules out.** A diagnosis that ends without a next move produces the reaction the
1K brief explicitly does not want — *"that is a good analysis"* — rather than the
one it does: *"can it start running this for us?"*

So the snapshot ends in **one recommended action**, not a menu.

```
Action      · what to do, concretely, this week
Why this    · which insight above it follows from
Who         · the creators or assets involved, named
Measure     · what would tell you it worked, and by when
Baseline    · the number it moves against, from today's read
```

**Where the pattern comes from.** If the action needs a repeatable content pattern
— and it usually does — pull `atlas-creative-analyst` for it rather than inventing
one here. It carries the evidence bar. If nothing clears that bar, **recommend the
test instead of the pattern**; a test is a legitimate first action and a fabricated
pattern is not.

**Rules that keep it honest:**

- **One action.** A list is a menu, and a menu is the customer doing the
  prioritisation Atlas was supposed to do.
- **It must follow from an insight above.** If you cannot point at the finding it
  came from, it is a guess dressed as a recommendation.
- **Name the measurement before the action.** The brief's rule is that every
  activation has a measurement plan attached — define it up front, not after.
- **Only recommend what the brand can actually do.** Partnership-ad activation,
  affiliate terms and lifecycle automation **do not exist yet.** Do not recommend
  launching a partnership ad. Recommend the thing that is real today — brief these
  three creators on this pattern, screen this roster against the standard, put
  this content in front of the buyer — and say plainly that Atlas will carry more
  of it when the paid loop lands.
- **Never claim Atlas will execute it.** Today the brand executes; Atlas
  recommends, measures and remembers. Say so.

**Worked shape:**

> **Do this:** brief @creator-a, @creator-b and @creator-c on the routine angle
> rather than the ingredient angle. Same product, different opening.
>
> **Why:** your audience saves routine content 4× more than ingredient content,
> and none of your current partners lead with it.
>
> **Measure:** saves per post against your 30-day median of 38. Three posts is
> enough to see it.
>
> **I'll be watching:** whether the gap narrows. That's the next snapshot.

The last line matters — it is what makes the return visit the point rather than a
follow-up.

---

## Step 4 — Write the snapshot

Write `profiles/{brand-slug}/alignment-snapshot-{date}.md`. Dated, because the
next one is a comparison — this is the baseline everything after measures from.

```markdown
# Alignment Snapshot for {Brand}
*Atlas Phase 1 · {date} · I've read {N} of your {actual} posts and gone through {M} of them properly*

## The gap
**{One sentence.}**
{2–3 lines of evidence with links.}

## What sits under it
{3–5 insights. Each: specific, linked, ending in a decision. Never padded.}

## SAY · what you claim
{Positioning, themes, own hashtags and cadence, taken from their content and site, with links}

## SHOW · what the market shows
{Creator content about the brand, or the labelled web-sourced bridge, or category
whitespace where nothing exists. Name the platform for anything web-sourced.}

## RESPOND · what people react to
{Engagement relative to the brand's own baseline; which posts over-perform and
what they have in common; comment volume as trend line where likes are withheld}

## Three aligned creators
| Creator | Handle (exact) | Why aligned | Evidence | Read closely? |
|---|---|---|---|---|

## Recommended first action
- **Do this:** … | **Why:** {which insight} | **Who:** {named creators/assets}
- **Measure:** {metric, baseline, by when}. Define it before the action, not after.
- **Atlas executes:** nothing yet. The brand acts; Atlas measures and remembers.

## What this is measured against
{The 90-day target from alignment-targets.md this snapshot speaks to}

## What Atlas could not see
{What I could not read, what I found on the web instead, platforms not yet live,
and how much I have not gone through yet. Never empty.}
```

**Denominate coverage honestly.** Report analyzed against *actual* media count,
not against posts-indexed — the second flatters by more than 2× on real brands.

---

## Step 4b — Publish the mirror as a visual artifact

The written snapshot is the record. **The page is what they actually look at**, come
back to, and show their team — so publishing is part of Step 4, not an optional
extra.

**Read
`${CLAUDE_PLUGIN_ROOT}/skills/atlas-alignment-snapshot/references/publishing-the-snapshot.md`
before building the page.** It carries the bar the page must clear, the
one-artifact-per-brand rule that keeps the URL stable, and which capabilities are
worth declaring.

⚠️ **In demo mode, publishing is not optional.** The page *is* the demo — a link
they can open on their phone in the taxi is worth more than anything said out loud.
This is the **one** action demo mode permits: an outbound artifact carrying the
fixture brand's name, not a state write, so it does not contradict the block below
— see
`${CLAUDE_PLUGIN_ROOT}/skills/atlas-session-mode/references/write-boundaries.md`.
It never carries a real brand's name.

---

## Test and demo modes — block silently, stay in character

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

## Step 5 — Arm the four channels, then go quiet

Phase 1 ends with Atlas already running, not with a promise. Present **one
confirmable card** with the four always-on channels, then stop talking.

| Channel | What it does | Needs |
|---|---|---|
| **1 · Social listening** | Hashtags, narratives and mentions mapped to the brand's ontology as they emerge | Hashtag watchlist (`add_hashtags`) |
| **2 · Brand safety** | Continuous monitoring of anyone near the brand; risks surfaced before they go public | Applied safety config (`set_brand_safety_config`) |
| **3 · Competitive analysis** | Who rivals are signing and which messages they're claiming, before it shows in sales data | Competitor set with exact-match casing strings |
| **4 · Industry benchmarking** | Where the brand sits against category norms, refreshed near-continuously | Confirmed category |

**Only arm what the plumbing supports.** For each channel, either configure its
prerequisite now — this is need-is-the-trigger firing, so pull the owning profile
skill inline if that is what it takes — or show the row as **"waiting on {X}"**
and say what it needs. A channel presented as on with nothing behind it is a lie
on a calendar, and it is the exact failure that costs the trust these fifteen
minutes just built.

---

## Step 6 — Offer what you can actually deliver

**A brief producer now exists** (`atlas-brief`), so the brief can finally be offered
for real instead of promised. Offer three things, as an offer — not an
announcement:

> Four channels are watching, and you've got one thing to do this week.
>
> Three things I can set up before I go:
>
> **This page, kept current.** Same link, so you can send it to anyone. Next time
> I look, it shows whether the gap moved.
>
> **A weekly brief.** Three to five things that actually changed, each with the
> post behind it. Its own link, always current.
>
> **In Slack, if you want it.** I'll post the headline and the link wherever your
> team already talks.

### Arming each one

| They say yes to | Do this |
|---|---|
| The page | Nothing extra — Step 4b already published it. Confirm the link. |
| The weekly brief | Create the schedule with the **remote scheduled-task tools** (never in-session cron). The task fires into a fresh session, so name the brand, the profile path and the brief page URL in it. |
| Slack | Ask **which channel**, and confirm the exact text before the first post. Never guess a destination. |

**Default to weekly, not daily.** For a brand with a few hundred posts there is
rarely enough new signal to justify a daily interruption, and a daily brief that
keeps saying "quiet" trains people to ignore it. Offer weekly, honour daily if they
ask for it, and say why.

**Create nothing unasked.** A schedule nobody agreed to is the same overreach as
arming a channel with nothing behind it.

### Still not on offer

- **A daily brief by default** — available on request, not the recommendation.
- **A self-refreshing page** — it updates when Atlas republishes it. Say "I'll keep
  it current", never "it updates live".
- **Anything in Aspire workflow** — outreach, briefs and rights are still the
  brand's to execute. Atlas recommends, measures and remembers.

**Then stop.** No summary of the summary, no upsell, no next-steps list. The
feeling to leave them with is that something is now working on their behalf — and
everything you said would happen actually will.

---

## Handing off to Phase 2

Phase 1 is complete. **Phase 2 is partly built:** `atlas-brief` produces the
recurring brief — weekly by default — and the schedule armed above fires into it.
**Still missing:** the dialog→context→smarter-brief habit loop that makes each
edition sharper from the conversation around it. That is the retention engine and
the next thing to build.

Do not write an edition here — `atlas-brief` owns that. Do not promise an insight
you have not produced, and do not imply the brief learns from this session yet.

Return to the router: the snapshot path, **the published artifact URL**, the named
gap, the three creators, the channels actually armed, and the channels still
waiting — with what each needs.
