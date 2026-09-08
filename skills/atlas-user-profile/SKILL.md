---
name: atlas-user-profile
description: Build a profile of the person operating Atlas for a brand — their role, what they own day-to-day, team shape and routing (who needs to see what), the tools they use today and where those fail, and their cadence preferences (interrupt vs digest). Use this during Aspire Atlas onboarding whenever a new user starts working with Atlas, says "get to know me," "ask me about my role," or "until you know enough about me," or whenever Atlas output needs to be scoped to a person (what THEY see vs what their boss sees) and no user profile exists yet. The v0 stub (role, brand, first ask) is written by /aspire at first contact; this skill enriches it lazily — 1–2 questions at natural moments across sessions, never a batch interview.
---


<!-- connector-attribution -->
> **Where these tools come from:** `project_*` are **Claude Project document** tools, not Aspire Alpha ones.
> If a session has another connector exposing similarly-named tools (`list_orgs` vs `list_my_organizations`, `list_profiles` vs `list_my_profiles`), they are different servers with different argument shapes — do not substitute one for the other.

# Atlas User Profile

Atlas customers told us the same thing four out of four times: **nobody wants *a* brief — everybody wants routing.** "What should the influencer team see versus the brand team versus the ads team?" Generic output is experienced as spam. Scope is not a filter on the brief; it is the brief's identity. This skill captures the person so everything Atlas produces afterward can be scoped to them — and it is also the "know me too" half of onboarding: the brand profile says what the company is; this says who Atlas is actually talking to.

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

One project doc: `profiles/{brand-slug}/user-profile--{user-slug}.md` (per person — a brand can have several). Plus a short list of extracted config values: routing map, cadence policy, and the friction Atlas is being hired to remove.

## Operating principles

Same rules as every Atlas profile skill: pre-fill before asking (here pre-fill is thin, so make questions cheap instead — multiple choice where the answer space is known); size the question count by what research and sibling profiles left open, and favor deep mechanic probes (questions that name their actual tools and ask how things work) over stacks of fact questions; offer options, welcome prose — present every question as a select with pre-drafted candidate answers plus an own-words escape, decompose mechanics into 2–3 quick selects rather than demanding typed essays, and when they do type, extract every structured value the prose holds; check `profiles/{brand-slug}/` in the project first and never re-ask what a sibling profile holds; write the result back with `project_write`.

## Step 1 — Pre-fill what little you can

From the session: name, email domain, how they've phrased things so far (a CMO asks different questions than a coordinator — note the register). From the project: prior sessions with this person, the brand profile if it exists. From the web, lightly: only if they're publicly findable (company team page, LinkedIn surface facts). Don't be creepy; a wrong guess about a company is a correction, a wrong guess about a person is off-putting. When unsure, ask.

## Step 2 — The question library (drawn from opportunistically)

/aspire already captured the three starting facts (role, brand, first ask) — never re-ask them. The items below are the enrichment library: draw 1–2 per session at natural moments ("while that runs — quick one"), only when the answer changes what Atlas does next. Anything answered organically during work gets extracted silently.

**Confirms / cheap asks:**
1. **Role & relationship read-back.** "You're {guess — e.g., head of marketing at {brand}, running the creator program yourself}?" If nothing to guess: one multiple-choice ask — owner/CMO · runs the program hands-on · agency working for the brand · exploring. This fork changes everything downstream (an agency needs client-facing outputs; a founder needs decisions, not decks).
2. **Team shape.** Who else touches this — solo, small team, or multiple functions (organic / paid / email / brand)? Multiple choice; this seeds the routing probe.

**Deep probes:**

- **P1 · Today's workflow & friction.** Never fire this cold — without a lead-in it lands as odd and salesy. It must follow directly from their earlier answers and reference them explicitly: the role they confirmed, the team shape they picked, the work they said they own. Deliver it as two light selects. First, *where the work lives today*: "For the {work they just described — e.g., creator vetting and reporting} — where does that happen today?" (options: in Aspire · another platform · spreadsheets / manual · a mix). Then, *where it hurts*, with options drafted from whatever the first answer implies, plus the own-words escape. **Rule: never name an Aspire competitor unless Aspire is named first** — when tools are listed, Aspire leads.

  For you, not for them: this is the highest-yield answer in the interview — it ranks the job families, positions Atlas against the incumbent, and defines "better" in their terms. Never say any of that to the user; telling someone their answer is valuable is pressure, not warmth.

- **P2 · Routing & cadence mechanics.** Not "how often do you want emails?" — mechanics:
  > "When something needs action, such as a risky partner post or a competitor move, walk me through who needs to know, in what form, and how fast. And what do you check daily versus what does your boss ask you for?"

  One answer yields four config values: the routing map (who sees what), interrupt-vs-digest policy per topic (risk interrupts; trends digest — but let *their* answer set it), the reporting artifact their boss expects, and how fast "fast" is for them.

**Don't ask:** job title as a form field (get it from the read-back), "what are your goals" in the abstract (P1 surfaces the real one), or anything the brand profile already answered.

## Step 3 — Write the profile

`profiles/{brand-slug}/user-profile--{user-slug}.md`:

```markdown
# User Profile for {Name} ({Brand})
*Atlas onboarding · {date} · role: {role} · relationship: {in-house / agency / owner}*

## Who they are
{Role, what they personally own day-to-day, register/tone to use with them}

## Team & routing map
| Audience | What they should see | Form | Cadence |
|---|---|---|---|
| {this user} | … | … | daily / weekly / interrupt |
| {boss / other teams} | … | … | … |

## Tools today & the friction
{Current stack; what's not working, in their words. This is what Atlas is hired to fix}

## Cadence policy
{Interrupts: … · Digest: … · The artifact their boss expects: …}

## How they define success
{What has to be true in 90 days for this person to call Atlas worth it}

## Config extracted
{The structured values pulled from their prose answers, listed explicitly}
```

## Step 4 — Close

Reflect back what you extracted ("from what you told me, risk alerts interrupt you directly, trends go in a weekly digest, and your CMO gets the Monday summary — correct?"). A corrected extraction is a win, not a failure. Point to the doc, and note which profile skill makes sense next given what they said — the friction they named usually names it.
