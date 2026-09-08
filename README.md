# Atlas by Aspire

**The content performance operating system for consumer brands.** Understand what
works, then put it to work.

Atlas is a Cowork plugin. Type **`/aspire`** after installing it and answer a short
set of questions — Atlas reads what your creators have posted and what your
audience did with it, and ends the first run with an alignment snapshot and a
recommended first action. Everything after that routes from the same command.

## Install

1. In Cowork, open **Customize** in the sidebar, then **Plugins**.
2. Select **Add marketplace** and enter:

   ```text
   aspire-alpha/aspire-atlas
   ```

3. Select **Browse plugins**, find **Atlas by Aspire**, and click **Install**. Cowork
   will ask you to sign in to Aspire — that's the connector Atlas reads your data
   through.
4. Start a new chat and run `/aspire`.

## What has to be true first

Installing this plugin is enough to get started — it bundles its own connection to
Aspire, so there is nothing to find or configure by hand first. The first time Atlas
needs it, Cowork will prompt you to authorize that connection; approve it once and
you're through.

One thing is still outside Atlas's control:

- **Aspire has already linked at least one of your social channels.** Atlas
  cannot link a channel on your behalf yet — that's real content-connection
  work Aspire's team currently does for you. If nothing is linked, Atlas will
  tell you so plainly instead of guessing, and will walk you through handing
  off to Aspire's own connect flow, or take just your handle as a fast path
  while that's pending.

A **Claude Project** is also required — Atlas writes your brand profile there as
you answer questions, and reads it back so a second session picks up where the
first left off instead of starting over.

If a channel isn't linked yet, talk to your Aspire contact — the plugin will say
what's missing rather than fail silently, but it can't link a channel on its own.

## What Atlas covers today

This release carries **Phase 1 — Calibrate**: a question-led first run (who your
brand is, what your creators and audience are actually doing, what you're
trying to achieve) ending in an alignment snapshot and one recommended next
step — plus the on-demand skills that go deeper on a brand, a user, a creator,
a competitor, brand safety, or a creative pattern once a real question comes
up.

Not everything Atlas will eventually do is built yet. Partnership ads,
affiliate, and lifecycle automation are roadmap, not shipped — nothing in this
release will claim otherwise.

## Licence

[PolyForm Internal Use License 1.0.0](./LICENSE) — you can run this for your
own business's internal use; you may not redistribute it.

## Support

Reach out through your usual Aspire point of contact.
