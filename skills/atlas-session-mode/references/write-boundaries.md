# Write boundaries outside a real run

**This file governs all three non-real modes** — `test-existing-org`,
`test-flow-check` and `demo`. Only `real` may change live state.

**Rule: in either test mode, no call may change state anywhere outside
`sandbox/`.** Reads are unrestricted for an existing-org test (that is the point);
every write is blocked and reported.

**Rule: in `demo` mode, nothing is written at all** — not `profiles/`, not
`sandbox/`. Reads come from `references/demo-fixture.md`, never from a live
account. There is exactly **one** permitted outbound action, and it is mandatory:
publishing the snapshot page, which carries the fixture brand's name. It is an
artifact, not a state write. **It must never carry a real brand's name.**

When in doubt about an unlisted tool: **if its name starts with set/create/update/
delete/add/remove/link/unlink/grant/revoke/invite/disable/start, treat it as a
write and block it.**

## Contents

- **Aspire Alpha**
- **Aspire Alpha Admin**
- **Aspire Alpha Debugging**
- **Project docs**
- **Scheduled tasks**
- **How to handle a blocked write**

---

---

## Aspire Alpha

**Reads — allowed**

`list_my_organizations` · `list_my_profiles` · `fetch_account` · `fetch_posts` ·
`search_posts` · `search_creators` · `search_creator_marketplace` ·
`get_brand_safety_config` · `list_hashtags` · `list_available_hashtags` · `get_job_status`

**Writes — blocked**

| Tool | What it would have changed |
|---|---|
| `set_brand_safety_config` | The brand's live screening posture |
| `add_hashtags` | What the brand is really watching |
| `remove_hashtags` | Same, destructively |
| `start_business_discovery` | Queues real ingestion and attributes it to the brand's insights cohort |
| `lookup_creators` | Same: a miss starts real discovery against Meta/TikTok and writes a Sighting to the brand |
| `lookup_posts` | Same, and a tiktok miss bills a paid per-post vendor call |

⚠️ **These three are the easy ones to get wrong.** They read like a fetch, but
they cost pipeline work and land in a real brand's cohort. Blocked in both test
modes. For an existing-org test, work with whatever is already there and say
what coverage is actually available. `lookup_creators`/`lookup_posts` became
reachable here when ASP-1532 promoted them from the Debugging surface — every
lookup is attributed by design, so there is no "just looking" mode.

---

## Aspire Alpha Admin

**Reads — allowed for an existing-org test**, and only within the one org the
persona belongs to: `get_org` · `get_profile` · `list_profiles` ·
`list_org_memberships`

No skill in this plugin calls `list_channels` any more (ASP-1467 removed the
router's use of it — the customer-reachable path now reads connection state
from `fetch_account` / `fetch_posts` on the public surface instead), so it is
no longer listed here at all — not because it became a write, but because
nothing in this plugin has a reason to call it, in any mode.

Never call `list_orgs`, `list_users`, `list_api_tokens`, `list_service_accounts`
or `list_linkable_accounts` in an impersonated session — a marketer at the brand
could not see any of it, and surfacing it breaks the persona and leaks other
customers.

**Writes — all blocked**

`create_org` · `update_org` · `delete_org` · `create_profile` · `update_profile` ·
`delete_profile` · `link_channel` · `unlink_channel` · `invite_member` ·
`grant_org_membership` · `revoke_org_membership` · `set_user_role` ·
`create_service_account` · `disable_service_account` ·
`set_service_account_rate_limit` · `create_api_token` · `revoke_api_token`

---

## Aspire Alpha Debugging

**Reads — allowed:** `get_agent_prompt` · `get_agent_prompt_history` ·
`get_agent_brand_config` · `get_agent_data_page` · `list_agent_types` ·
`get_insight` · `list_insights` · `search_insights` ·
`list_profile_marketplace_searches` · `run_agent_preview`

**Writes — all blocked:** `set_agent_prompt` · `append_insight` ·
`update_insight` · `delete_insight` · `create_profile_marketplace_search` ·
`update_profile_marketplace_search` · `disable_profile_marketplace_search` ·
`remove_profile_marketplace_search`

---

## Project docs

| | Real | Test | **Demo** |
|---|---|---|---|
| `project_write` to `profiles/…` | allowed | **blocked** | **blocked** |
| `project_write` to `sandbox/…` | n/a | allowed | **blocked** — demo writes nothing |
| `project_read` / `project_search` on `profiles/…` | allowed | **blocked** — a test must not inherit real state | **blocked** — the fixture is the only source |
| `project_delete` | allowed | only inside `sandbox/` | **blocked** |
| publish the snapshot page | allowed | **blocked** | **required** — fixture brand name only |

`project_*` are Claude Project document tools, not Aspire Alpha ones.

---

## Scheduled tasks

**Blocked in test mode, and in demo mode.** A scheduled task outlives the session,
so a run that arms one leaves something firing at a real brand next week — and a
demo has no real brand to fire at. When Step 5 would arm the passive channels or
create the brief schedule, report what it would have armed and stop.

In a **real** run both are allowed: `atlas-brief` is a real producer, so a brief
schedule fires into something.

---

## How to handle a blocked write

**Silently.** Add it to the ledger and carry on in character. Do not announce it,
do not explain it, do not substitute a "would have" sentence for the step.

The step still runs — make the decision, show the reasoning a customer would see,
present whatever card the real flow presents. Only the final call is skipped, and
a customer would not have seen that call anyway.

**Never claim a state change that did not happen.** "Four channels are running" is
false in a test; "here's what I'd switch on for you" is what the real flow says at
that moment regardless.

Everything in the ledger surfaces once, in the debrief, after the run ends. See
the SKILL for the debrief format.
