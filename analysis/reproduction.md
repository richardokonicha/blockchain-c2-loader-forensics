# Safe Analysis / Reproduction Guide

## ⚠️ CRITICAL WARNING — DO NOT EXECUTE THE PAYLOAD

This loader grants arbitrary remote code execution. Running it will compromise your machine. This guide is for **static analysis only** in an isolated environment.

---

## What Has Been Verified (Static Only)

- File contents read via `git show 1b13415:file` (no checkout, no build)
- String patterns matched with `grep` against saved file copies
- No `npm run dev`, `npm run build`, `node`, `pnpm install`, or any execution performed on the payload
- The malicious commit objects have been fully purged (`git reflog expire --expire=now --all`, `git gc --prune=now --aggressive`)

---

## Recommended Safe Environment for Any Further Study

If a researcher needs to observe the loader behavior, use:

- **Air-gapped VM** with no network access to production or personal accounts
- **Snapshot before, snapshot after** — discard VM entirely after observation
- **No SSH keys, no browser sessions, no crypto wallets** mounted or accessible
- **Network isolation**: block all outbound except monitored sandbox gateway
- **Static analysis tools preferred** over execution: `grep`, `strings`, `python` text parsing

---

## What Was Not Done (Intentionally)

- No execution of `postcss.config.js`
- No `next dev` or `next build` from infected branch
- No network probing of the blockchain dead-drop wallet (`0xa322...`)
- No HTTP requests to `/0x/cls` or `/0x/ls` endpoints
- No interaction with the attacker-controlled C2 infrastructure
- The blockchain wallet's transaction history is public (`etherscan.io`) and can be audited without interaction; we did not probe it actively

---

## Evidence Preservation Note

The malicious commit (`1b13415`) was fully removed from all branches and loose objects to prevent accidental execution by any future clone. A metadata-only reference (`evidence/1b13415.diff`) preserves the commit hash, author, date, and message without executable payload content.
