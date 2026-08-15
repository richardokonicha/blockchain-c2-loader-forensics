# Forensics Report — Blockchain C2 Malware Incident
**Project:** COMPANY WEB (`COMPANY-WEB`)  
**Incident Date:** 2026-08-14 (discovered) / 2026-05-16 (malicious commit pushed)  
**Severity:** Critical — Remote Access Trojan (RAT) loader with blockchain-resolved C2  
**Status:** PURGED + RESCUED — malicious objects fully removed, clean commit restored  

---

## Executive Summary

A multi-stage remote-access trojan loader was discovered hidden in a commit (`1b13415`) pushed to the GitHub mirror of `COMPANY-WEB` on May 16, 2026. The payload was embedded inside `postcss.config.js` and `ui/postcss.config.js` using ~500-space padding after a single `};` line — invisible in standard diff views. The loader resolves its command-and-control server by reading an Ethereum wallet address from a hardcoded transaction, decoding the `to` field as raw IPv4 bytes (bytes 0-3 and 4-7 = two server IPs). This blockchain dead-drop allows attackers to rotate infrastructure with a single transaction (~$1 gas) with no domain to seize and no hardcoded IP to blacklist.

The malware never reached GitLab (`origin/main`), production (`COMPANY.com`), or any other repository. The `genesis` annotated tag (`4aa46c4`) and live production deploy (`dpl_98swmrafZa4noJiQvFqb619A3NEj`) remain clean and verified.

A legitimate 222-line documentation file (`run_instruction.md`) pushed by the same author was rescued and committed separately (`6702c86`) with full line-by-line verification.

---

## Evidence Inventory

| Evidence | Location | Status |
|---|---|---|
| Malicious commit (`1b13415`) | `evidence/1b13415.diff` | Purged from `github/main`; objects GC'd; tag deleted |
| Rescued docs (`run_instruction.md`) | `evidence/run_instruction.md` | Committed clean (`6702c86`) |
| Clean origin/main (`4aa46c4`) | `evidence/clean-commit.log` | Live, verified |
| `postcss.config.js` (clean current) | `evidence/postcss.config.js` | Verified 0 malicious hits |
| Direct message to REDACTED-FAMILY-MEMBER | `analysis/whatsapp-message.md` | Sent; awaiting confirmation |
| Technical payload explanation | `analysis/payload-analysis.md` | Full static analysis, no execution |
| Indicators of Compromise (IOCs) | `iocs/ioc-table.md` | Confirmed from static diff |

---

## Timeline

- **May 16 10:22:34 2026 (+0100)** — `1b13415` pushed to `github/main` by REDACTED-FAMILY-MEMBER (`REDACTED-EMAIL`)
- **May 16** — Payload injected: `postcss.config.js`, `ui/postcss.config.js`, `.gitignore` modifications (`temp_auto_push.bat`, `branch_structure.json`)
- **2026-08-14** — Discovered during routine cleanup; push to `github/main` rejected (divergence); inspection revealed malware
- **2026-08-14** — Merge halted; analysis completed; `github/main` NOT force-pushed yet (awaiting user confirmation)
- **2026-08-14** — Direct message sent to REDACTED-FAMILY-MEMBER; laptop isolation advised; credential rotation deferred
- **2026-08-15** — `github/main` force-pushed clean (`4aa46c4`); malicious objects GC'd; `run_instruction.md` rescued (`6702c86`)
- **2026-08-15** — Full workspace scan completed (`COMPANY-WEB`, `COMPANY-CLOUD-API`, `COMPANY-CONSOLE`, `COMPANY-DOCS`, `COMPANY-PILOT`, `COMPANY-AI-GATEWAY`, `dusk`, `ARCHIVED`): zero spread

---

## Attribution

No confirmed attribution. The technique (developer-targeted delivery, blockchain-resolved C2, XOR-staged payload, detached `node -e` execution, campaign ID tagging) strongly resembles tooling associated with DPRK-linked "Contagious Interview" / Marstech / Lazarus-adjacent clusters, which specifically target developers and crypto holders. This is a resemblance assessment, not a confirmed identification — these loaders are copied and resold.

---

## Remediation Steps Completed

1. `github/main` force-pushed to clean `4aa46c4` (same as `origin/main` / `genesis`)
2. Malicious commit `1b13415` fully purged (`git reflog expire --expire=now --all`, `git gc --prune=now --aggressive`, quarantine tag deleted)
3. `run_instruction.md` (222 lines) rescued, line-verified clean, committed with original author (`6702c86`)
4. Zero-spread scan completed across all workspace repos
5. `genesis` annotated tag (`4aa46c4`) remains live on both `origin` and `github`
6. Infisical selected for secure secret rotation (`.infisical.json`: `projectId: 8210a2d4...`)

---

## Remediation Steps Pending

- **Credential rotation** via Infisical (`infisical secrets --env prod`) — loader had arbitrary RCE; assume full exposure
- **REDACTED-FAMILY-MEMBER's laptop** — forensics preservation vs wipe; disconnect from network
- **Publication** of this report / dev.to article — awaiting confirmation

---

## Report Metadata

- **Report Version:** 1.0
- **Report Date:** 2026-08-15
- **Author / Analyst:** Internal security review (redacted identifiers)
- **Target System:** `COMPANY-WEB` (`COMPANY-WEB` marketing site, Next.js 15, App Router, Tailwind CSS)
- **Malicious Commit:** `1b13415` (purged — metadata preserved in `evidence/1b13415.diff` only)
- **Clean Reference:** `4aa46c4` (`origin/main`, `genesis` tag)
- **Analysis Type:** Static only; no payload execution performed
- **Standard Alignment:** MITRE ATT&CK (`analysis/attack-mapping.md`), IOC table (`iocs/ioc-table.md`), reproduction guide (`analysis/reproduction.md`), ethical note (`analysis/ethical-note.md`)
- **Publication Status:** Pending user confirmation (Infisical rotation selected; laptop forensics pending; dev.to article ready)
