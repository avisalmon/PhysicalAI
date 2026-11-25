# Contributing

## Rule #1 — This repository is PUBLIC

Everything committed here is visible to the entire world and served as a public web page.

### Never commit

- Intel confidential, Intel internal, or Intel proprietary information of any kind
  (documents, roadmaps, product names, code, architecture details, internal URLs, hostnames, email threads, meeting notes).
- Secrets: API keys, tokens, passwords, certificates, private keys, connection strings, `.env` files.
- Customer, partner, or personal data (PII).
- Third‑party copyrighted material without a compatible license.

### Where non-public material goes

Only inside the gitignored `private/` folder (or `confidential/`, `internal/`). These folders are **never** tracked by git and never pushed. Treat them as local scratch space only.

### Before every commit

1. Run `git status` and `git diff --cached` and read what you are about to publish.
2. Ask: *"Would I be comfortable with this on the front page of the internet?"* If not, don't commit it.
3. If something confidential was committed by mistake, **do not just delete it in a new commit** — it stays in history. Rewrite history and rotate any exposed secrets immediately.

## Content guidelines

- Write for a general technical audience; cite public sources.
- Prefer open datasets, open-source tools, and publicly available papers.
- Keep course material vendor-neutral unless the public documentation is being referenced.
