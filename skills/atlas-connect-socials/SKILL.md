---
name: atlas-connect-socials
description: >-
  Step 2 of Atlas Phase 1 calibration — get the brand's content into Atlas so
  every later question is pre-filled instead of blank. Invoke from the `aspire`
  router for a brand with no content connected yet, or on "connect my
  Instagram", "connect socials", "link my account", "add my Instagram". Never
  links an account itself — hands off to Aspire's own connect flow on the
  website for full auth, and takes the handle-only fast path when the user
  would rather not do that right now. Starts ingestion immediately and hands
  off to the brand interview while content is still loading — the interview
  is what fills the wait.
metadata:
  phase: "Phase 1 · Calibrate — Step 2 (min 2–5)"
  artifact: "Content corpus"
---


<!-- connector-attribution -->
> **Where these tools come from:** the bare tool names below (`fetch_account`, `fetch_posts`, `search_posts`, `start_business_discovery`, …) are **Aspire Alpha connector** tools, all on its **public** surface — this skill never calls an admin-surface tool (`link_channel`, `list_channels`, `unlink_channel`), and never should; a brand user does not have them.
> If a session has another connector exposing similarly-named tools (`list_orgs` vs `list_my_organizations`, `list_profiles` vs `list_my_profiles`), they are different servers with different argument shapes — do not substitute one for the other.

# Step 2 — Connect socials

**Target: minute 2 to minute 5.** The user follows the connect hand-off or
just gives a handle. Atlas starts reading immediately. **No dead time** —
ingestion runs in the background while Step 3 happens in the foreground.

Feeling to produce: *"it's already working."*

---

## What is actually wired today

State this, do not ask about it.

| Platform | Status |
|---|---|
| **Instagram** | Live — the only network Atlas reads today. |
| TikTok | Not wired. |
| YouTube | Not wired. |
| Shopify | Not wired. |

Any other network is refused on the product's side, with a stable error — this
skill never places or handles that call, and never should (see below). Do not
offer them, and do not ask "which platforms do you use?" — asking implies a
choice that does not exist. If their program lives on TikTok, they need to
hear that **now**, not after they finish.

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

## The ask

One message, three things — and all three are about **scope**, which is why they
belong here rather than in the interview. The interview stays focused on who they
are and what they need.

> I read your Instagram content. That's what I can see today. I can't see your
> store or your ad account. So when I talk about what's working, I mean what earns
> attention, not what sells.
>
> **What's your Instagram handle?** I'll start reading straight away.
>
> **And what are you actually selling through this?** A couple of product names is
> plenty.
>
> **Is any of this creator content running as paid today?** Yes, no, or not sure.

### Why exactly these two, and nothing else

Each one changes a recommendation **now**. That is the whole test for asking.

| Question | What it changes today |
|---|---|
| What you're selling | Which creator content counts as on-topic, and what the whitespace search looks for. Without it, "your category" is a guess. |
| Creator content in paid | Whether a pattern found later is worth testing with spend behind it, or only organically. It changes the shape of the recommended action. |

**Do not ask for anything else.** Margins, commission ranges, inventory,
attribution rules and ads objectives are all in the vision brief and none of them
change what Atlas can do today. Record them as **known-absent** in the brand
profile — a named gap, not a blank field — and ask properly when there is something
to act on. Asking now implies a capability that does not exist.

**All three are skippable.** "Not sure" is a real answer to the paid question and
the most common one. Take it and move.

### Lead with scope, once, without apology

The scope line is a fact, not a caveat. Say it once, plainly, and never return to
it — no roadmap talk, no "coming soon". If a later answer would have needed the
store or the ad account, name it at that moment instead.

**This skill never links an account, and never calls a tool that does.**
Aspire Atlas handles auth, linking and which identifiers get stored — once, in
code, on the website. [ADR 0046](../../../../../docs/adr/0046-atlas-onboarding-as-an-in-repo-cowork-plugin.md)
draws this line for the whole plugin: Atlas recommends, the brand executes,
and identity is engineering's layer. This skill's only two moves for the real
connect are **hand off** — tell the brand to connect their Instagram from the
Aspire website whenever they want the deeper numbers — and **verify** — check
`fetch_account` / `fetch_posts` for the handle they gave, which reports real
content regardless of which path put it there. Never name a tool, an auth
flow, or a linking mechanic to the user, and never imply this skill is the
thing doing the connecting.

**The handle-only fast path needs none of that.** A brand who just gives a
handle gets read immediately via `start_business_discovery` — no hand-off, no
wait on the website, no auth screen. The hand-off above is only for the brand
who wants the deeper, owner-only numbers (reach, watch time, saves, audience
demographics) later. **Do not lecture them about owner auth up front** — name
that limit the first time a question actually needs it, not at minute three.

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

## What this step actually does

1. `fetch_account({ handle })` first. `{ found: false }` is a **normal
   result** — it means nothing has been fetched for that handle yet, not
   that the brand does not exist.
2. **Verify it's them before reporting anything.** `fetch_account` echoes
   `username`, `name`, `biography`, `followersCount`. Confirm the username is
   the handle they gave and the name and bio are plausibly their brand. A
   typo, a homonym, or a personal account instead of the business one all
   surface here — and all of them poison every downstream step silently. If
   it doesn't look like them, say so and ask rather than proceeding on a
   near-match.
3. **If found, check for content next — an account record is not content.**
   `fetch_posts({ handle })` is the real test. Posts present: report real
   coverage — how many, oldest and newest date. That is the corpus, stated
   honestly. No posts yet: that is its own state, not a failure and not the
   same sentence as "not connected" — see the table below.
4. If not found at all, or found with no posts yet: `start_business_discovery({
   handle, asProfile, postLimit })` — the handle-only fast path, and the
   thing that turns either state into real content.

### Two states, two messages — never the same sentence

| State | Say something like |
|---|---|
| `fetch_account` → `found: false` | *"I don't have anything from @handle yet. I'll start reading now."* |
| `fetch_account` → `found: true`, `fetch_posts` → no posts | *"Your Instagram's connected. I can't see any of your posts yet, so I'll keep checking. In the meantime, tell me about the brand."* (the `plain-english` worked example, almost verbatim, so reuse its register) |

Both are normal. Both start ingestion via `start_business_discovery` if it
hasn't already run. Neither is a reason to stall Step 3 — hand off to the
interview either way, per "No dead time" below. What must never happen is the
two collapsing into one sentence, or either reading as an error.

---

## Starting ingestion — the parts that go wrong

**Always attribute the run.** Pass `asProfile` (the brand's profile slug from the
router's Step 0). An unattributed run returns `attributed: false` and **never
shows up in that brand's insights cohort** — the ingestion works and the brand
gets no credit for it. This is a silent, expensive omission.

**`postLimit` is a post count, not a date range.** Default 25, maximum 100. There
is no date parameter.

> ⚠️ **Engineering note.** The flow card promises *"ingests 90 days of content."*
> The API cannot express that. It only takes "up to N posts, max 100." For a brand posting
> five times a week, 100 posts is roughly 20 weeks; for a daily poster it is
> ~14 weeks; for a brand posting twice a day it is under two months. Either add a
> date-window parameter to `start_business_discovery`, or change the promise to a
> post count. **Do not say "90 days" in the product until the API can honour it.**
> Say what you actually fetched: *"reading your last 100 posts, back to March 18."*

Then poll `get_job_status({ jobId })` — and know what the statuses mean:

- `completed` means the pipeline settled the data in Postgres. It **does not mean
  the posts are searchable yet.** Search indexing is a separate async pipeline
  downstream, so there is a real (normally short) lag.
- An empty `search_posts` result right after `completed` is that lag, **not a
  bug** and **not an empty brand.** Retry shortly rather than reporting zero.
- `completed` also does not mean *analyzed*. On a prior 50-post run, only 34% of
  posts had `analyzedAt` set at completion, and coverage did not move over a
  90-second re-poll.

On failure, `start_business_discovery` returns a stable `code`. Handle them
differently — they are not all the same message:

| `code` | What to do |
|---|---|
| `rate-limited` | Back off and retry. Say a queue is busy, not that it failed. |
| `unavailable` | Transient. Retry once, then move on to Step 3 and revisit. |
| `unauthorized` | They are not signed in. Point at sign-in, do not retry. |
| `not-found` | The named org or profile does not resolve. Re-read Step 0 state. |
| `forbidden` | They do not belong to that org, or to the profile's org. Stop. |
| `invalid-input` | The named profile is not in the named org. Fix the pairing. |
| `internal-error` | Report it plainly. Do not retry in a loop. |

---

## No dead time — the whole point of this step

**Do not make anyone watch a progress bar.** The moment ingestion is started:

1. Say in one line what is loading and roughly how much — real numbers only.
2. Hand off to `atlas-brand-interview` immediately.

The interview is what fills the wait. By the time Step 3 is done, the corpus is
there and Step 4 has something to stand on. Never show a bare spinner, and never
show a percentage you did not read from `get_job_status`.

---

## Handing off

Close by passing forward, so Step 3 does not re-derive any of it:

- `organizationId` and `profileSlug`
- the Instagram handle, and whether it is **linked** or **handle-only**
- coverage stamps: posts indexed, posts analyzed, oldest and newest observation
- the ingestion `jobId`, if one is in flight
- anything the user said in passing about the brand — it belongs in the Brand
  Context file and they should not have to say it twice
