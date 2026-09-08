---
name: atlas-brief
description: >-
  Produce the recurring Atlas brief — three to five things worth knowing since
  last time, each traceable to the post behind it — publish it as a page at a
  stable link, and offer to send it to Slack. Invoke on "/atlas-brief", "run my
  brief", "what's new", "what changed", "catch me up", "today's brief", "weekly
  brief", "send me the brief", or when a scheduled task fires to refresh a brand's
  brief. Reads the brand's profile docs, alignment targets and last edition, then
  reports only what materially changed. A quiet week produces a short quiet brief,
  never manufactured filler.
metadata:
  phase: "Phase 2 · Passive Intelligence — the recurring loop"
  artifact: "The brief, at a stable per-brand link"
---


<!-- connector-attribution -->
> **Where these tools come from:** the **remote scheduled-task tools** referenced below are
> session-level tools, not Aspire Alpha ones; anything reading brand state goes through the
> **Aspire Alpha connector**, and `project_*` are **Claude Project document** tools.

# The brief

The recurring output. **Three to five things worth knowing, each traceable to the
post that produced it.** Conversational, not a report.

This is the retention engine: the brief earns its place by being right, repeatedly.
Everything below serves that, and the single fastest way to lose it is a brief that
pads itself out on a quiet week.

## How to talk to them — read this first

**Read `${CLAUDE_PLUGIN_ROOT}/skills/plain-english/SKILL.md` before writing a
word.** No "index", no "corpus", no "coverage", no tool names, no step numbers, and
never narrate your own instructions. Lead with what is working; gaps are headroom,
not errors. **The brand owns the wins; the work owns the shortfalls**, so work that
underperformed is never "your weakest". No em dashes, ever, and every headline is
one complete phrase.

---

## Step 1 — Read state before looking for news

In order:

1. `profiles/{brand-slug}/alignment-targets.md` — **the filter.** A brief item that
   serves no target does not ship. That is the contract Step 4 set.
2. The most recent `alignment-snapshot-*.md` — the named gap. *Has it moved?* That
   is usually the strongest item available.
3. `profiles/{brand-slug}/brief-{date}.md` — the last edition. **Never repeat an
   item they have already seen** unless it materially changed, and then lead with
   what changed rather than restating it.
4. Any `state.md` — declined work, applied configs, watchlists.

Then look for what is new: mentions, roster activity, competitor moves, content
that over-performed, safety flags.

---

## Step 2 — Compose it

Full rules in `references/brief-composition.md`. The essentials:

- **Three to five items. Never more.** Six is a report and nobody reads it twice.
- **Every item carries its evidence link.** A claim without a post behind it is the
  thing Atlas exists to replace.
- **Every item ends in a decision or a question**, not a fact. *"A competitor
  signed four creators in your niche this week. Want to see who's left?"* beats a
  count.
- **Lead with the one that matters most**, not the newest.
- **Say what changed, not what is true.** Four of six customers asked "what
  changed?" rather than "what's true?" — the delta is the product.

### The quiet week

**If nothing material happened, say so in two lines and stop.**

> It was a quiet week. Nothing moved that needs you. The gap is where it was.
> There are no new mentions and the roster is steady. I'll be back Monday.

A brief that manufactures insight to fill space will lose the morning habit faster
than silence would. **A short honest brief is a successful brief.** Never pad with
things that were true last week, restated metrics, or "no change" rows dressed as
findings.

---

## Step 3 — Publish it as a page

**The brief is a page, not a wall of chat text.** Publish with the Artifact tool.

**Load `artifact-design` first** — always. There is a worked example at
`${CLAUDE_PLUGIN_ROOT}/skills/atlas-brief/references/brief-example.html`.

### One page per brand, republished

**Same URL every edition.** Record it in `alignment-targets.md` alongside the
snapshot URL. They bookmark it once and it is always current — that is what makes
it a habit rather than a document they lose.

The page carries:

- **Today's items** — the current edition, at the top, dated
- **What this is measured against** — the targets, so relevance is visible
- **A short history** — what previous editions surfaced and what changed since.
  This is the compounding part: the third edition is more valuable than the first
  because the trend is visible on the same page.
- **The stable link and the date**, plainly. Someone opening it cold should know
  immediately how fresh it is.

Design it to be **scanned in thirty seconds on a phone** — this gets read in a
queue, not at a desk. That makes it a different object from the snapshot: the
snapshot is an essay, the brief is a scan.

---

## Step 4 — Offer Slack

**Sending is handled through the user's own Slack connection, not by this plugin.**
The plugin declares no Slack server. Use whatever Slack tool the session has; if
there is none, say Slack isn't connected and offer the link instead.

Full rules in `references/slack-delivery.md`. The three that matter:

1. **Never post without explicit confirmation of the destination and the text.**
   A post to a shared channel is visible to colleagues and cannot be recalled. Show
   exactly what will go, name exactly where, wait for a yes.
2. **Ask where. Never guess a channel.** A brand's competitive intelligence landing
   in the wrong channel is a real harm, not an inconvenience.
3. **Send a short message plus the link, not the whole brief.** Three or four
   lines and the headline item. The page is the detail; Slack is the nudge.

> Want this in Slack? Tell me the channel or DM and I'll post the headline with the
> link.

**Never post in test or demo mode.** Slack is externally visible, which makes it a
write in every sense that matters. Say what would have been sent, to where, and
stop.

---

## Step 5 — Write the record

Write `profiles/{brand-slug}/brief-{date}.md` — the edition, so the next one can
avoid repeating it and can measure what changed. In test or demo mode, `sandbox/`.

```markdown
# Brief for {Brand} on {date}
*edition {n} · measured against: {targets} · page: {artifact URL}*

## Items
{Each: the finding, the evidence link, the decision or question it ends in, and
which target it serves}

## Sent
{Slack destination if posted, or "not sent"}

## Not included
{What was considered and cut, and why. This stops the next edition from
re-surfacing it as if new}
```

That last section does real work. Without it, edition three re-reports what edition
two decided was not worth saying.

---

## Step 6 — Close short

The brief is the whole output. Do not summarise it in chat after publishing it —
give the headline, the link, and stop.

> Three things this week. The gap narrowed, because @mornings.with.dez posted the routine
> angle and it's your best-saved creator post yet. Full brief: {link}
>
> Want it in Slack?

## Test mode

If `atlas-session-mode` set a test or demo mode: docs to `sandbox/`, **no Slack
post**, and never narrate the test. See
`${CLAUDE_PLUGIN_ROOT}/skills/atlas-session-mode/references/write-boundaries.md`.

## Scheduling

Created only when the customer asks for it, via the **remote scheduled-task tools**
— never in-session cron, which dies with the session and silently never fires.

The task fires into a fresh session, so it must stand alone: name the brand, the
profile path, and the brief page URL to republish.

**Default to weekly.** Daily is available and some brands want it, but for a brand
with a few hundred posts there is rarely enough new signal to justify a daily
interruption — and a daily brief that keeps saying "quiet" trains people to ignore
it. Offer weekly, honour daily if they ask, and say why.
