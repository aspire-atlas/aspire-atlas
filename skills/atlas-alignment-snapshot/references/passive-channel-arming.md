# Arming the four passive channels

Phase 1 ends with Atlas running. One confirmable card, four rows, then silence.

**The governing rule: only promise what the plumbing supports.** Each row is
either configured now or labelled *waiting on {X}*. There is no third state.

---

## Channel 1 · Social listening

*Hashtags, narratives and mentions mapped to the brand's ontology as they emerge.*

- **Needs:** a hashtag watchlist on the profile.
- **Arm with:** `add_hashtags`, then `list_hashtags` to verify it's active
  (adding a hashtag activates it immediately — there is no separate
  make-it-live step).
- **Owner if a deeper set is needed:** `atlas-competitor-profile`.
- **Say out loud:** share-of-voice **starts counting from the day the watchlist
  exists.** Day one is a baseline, not a trend. Never show an SOV number as though
  it has history.

## Channel 2 · Brand safety

*Continuous monitoring of anyone near the brand. Risks surfaced before they go public.*

- **Needs:** an applied, verified safety config.
- **Arm with:** `get_brand_safety_config` → `set_brand_safety_config`.
- **Owner:** `atlas-brand-safety-profile` — pull it inline if the brand cares and
  no config exists.
- **Two absolutes:** get an explicit yes before applying (this config blocks and
  flags creators, it must never change silently), and **an unenriched post must
  never read as safe.**
- **Respect a recorded decline.** A stub saying safety was skipped at the brand's
  call is a decision. Show the row as deliberately off, not as a gap.

## Channel 3 · Competitive analysis

*Who rivals are signing, which messages they're claiming, before it shows in sales data.*

- **Needs:** a competitor set with **exact-match casing strings**, not just names.
- **Owner:** `atlas-competitor-profile`.
- **Say out loud:** a casing mismatch returns a silent zero that reads exactly
  like "no partners". Getting the exact strings is the difference between an
  answer and a confident zero.

## Channel 4 · Industry benchmarking

*Where the brand sits against category norms, refreshed near-continuously.*

- **Needs:** a confirmed category. Usually already in `brand-profile.md`.
- **Cheapest to arm** — often needs nothing new.
- **Say out loud** what the norm is computed over. A benchmark whose peer set is
  unstated is unfalsifiable, and unfalsifiable numbers are what the incumbents
  sell.

---

## Scheduling

Use the **remote scheduled-task tools**, never in-session cron — an in-session
schedule dies with the session and the user's channel silently never runs.

✅ **A brief schedule is safe to create — `atlas-brief` is a real producer.** Use
the remote scheduled-task tools so it survives the session, and name the brand,
the profile path and the brief page URL in the task. **Weekly is the default.**
What must never be scheduled is anything with no producer behind it: a trigger
firing into nothing is worse than no trigger.

**What you can schedule honestly:** a weekly re-read that updates the snapshot page
and reports whether the gap moved. That has a real producer — this skill — and a
real output. Offer it, and create it only if they say yes.

State the real cadence they agreed to — weekly unless they asked for daily — and
never imply the brief learns from their questions yet. It reports change; the
loop that makes it sharper is not built.

---

## The card

Four rows, honest states, then stop:

```
Four channels, from today:

  ✓ Social listening      12 tags live · SOV counts from today
  ✓ Brand safety          Standard posture applied · verified on {post}
  ⏳ Competitive analysis  waiting on: exact competitor strings
  ✓ Industry benchmarking high-protein food · vs {N} accounts

Two things I can set up: this page kept current at the same
link, and a weekly look that updates it and tells you if
the gap moved.
```

Then nothing. No recap, no next steps, no upsell. The last impression should be
that something is now working on their behalf.
