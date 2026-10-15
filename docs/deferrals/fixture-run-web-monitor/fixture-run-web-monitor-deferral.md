---
type: deferral
title: Web monitor for a running fixture
description: A browser page over a running hypothesis fixture stays deferred. The live view is a clack log of the campaign files.
status: draft
---

# Web monitor for a running fixture

The disposable launcher serves saved runs at `127.0.0.1` port `8766` from `monitor.py` and `monitor.html`. Its `--cli` flag already prints status from those same files and does not start the server. The hypothesis runner keeps a `@clack/prompts` log and does not carry the page, the session token, or the two tabs.

## Revisit when

- [ ] A maintainer cannot tell which cell is running, its exit, its elapsed time, and its hint from the `@clack/prompts` log while a campaign is in progress.
- [ ] Someone asks for a second view after that log has been used on a real campaign.
