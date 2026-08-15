# Blockchain C2 Loader — Defensive Forensics Report

**Severity:** Critical — Remote-access loader with blockchain-resolved C2  
**Target:** Marketing site build pipeline (`postcss.config.js`, `ui/postcss.config.js`)  
**Discovery:** 2026-08-14 | **Malicious commit:** `1b13415` (May 16) | **Clean reference:** `4aa46c4`

---

## Incident Summary

A multi-stage loader was embedded in build config files (`postcss.config.js`, `ui/postcss.config.js`) using ~500-space padding after a single `};` line — invisible in standard diff viewers. The loader resolves its command-and-control server by reading a hardcoded Ethereum wallet (`0xa322E5f3D311D3080e6f0121063e9aDC2490Ef1a`) from a transaction, decoding the `to` field (20 bytes) as two IPv4 addresses (bytes 0-3, 4-7). This blockchain dead-drop allows infrastructure rotation with a single transaction (~$1 gas), with no domain to seize and no hardcoded IP to blacklist.

The payload never reached production (`COMPANY.com`), GitLab (`origin/main`), or any other repository. The live `genesis` tag (`4aa46c4`) and all workspace repos verified clean (`COMPANY-CONSOLE`, `COMPANY-DOCS`, `COMPANY-PILOT`, `COMPANY-AI-GATEWAY`, `COMPANY-CLOUD-API`, `dusk`, `ARCHIVED`): zero spread.

---

## Evidence

| Evidence | Status |
|---|---|
| Malicious commit (`1b13415`) | Purged (`git reflog expire --expire=now --all`, `git gc --prune=now --aggressive`); tag deleted |
| Clean reference (`4aa46c4`) | Live (`origin/main`, `genesis` tag) |
| Rescued documentation (`run_instruction.md`, 222 lines) | Committed clean (`6702c86`) |
| Payload objects | Intentionally destroyed; metadata preserved (`evidence/1b13415.diff`) |

---

## Timeline

- **May 16** — `1b13415` pushed to GitHub mirror (`COMPANY-WEB`)
- **Aug 14** — Discovered during cleanup (`github/main` divergence); analysis started; merge halted
- **Aug 14** — Direct message to targeted user (family member assisting with project); laptop isolation advised; credential rotation deferred
- **Aug 15** — `github/main` force-pushed clean (`4aa46c4`); malicious objects fully purged; `run_instruction.md` rescued (`6702c86`); workspace scan complete — zero spread
- **Aug 15** — Full static analysis completed (`analysis/payload-analysis.md`); MITRE ATT&CK mapping (`analysis/attack-mapping.md`); IOCs (`iocs/ioc-table.md`); reproduction guide (`analysis/reproduction.md`); ethical note (`analysis/ethical-note.md`)
- **Aug 15** — Forensics repo created (`blockchain-c2-loader-forensics`, public, `main` at `4442b06`)

---

## Attribution

Not confirmed. The technique (developer-targeted delivery, blockchain-resolved C2, XOR-staged payload, detached `node -e` execution, campaign ID `A8-3343-1` / `8-3343`) strongly resembles tooling associated with DPRK-linked `Contagious Interview` / Marstech / Lazarus-adjacent clusters. These loaders are copied and resold; copycat is possible. Response is identical either way: assume full compromise, rotate everything.

---

## Key Indicators (IOCs)

| Category | Value |
|---|---|
| ETH Wallet (C2 dead-drop) | `0xa322E5f3D311D3080e6f0121063e9aDC2490Ef1a` |
| Campaign ID | `A8-3343-1` (`Sec-V` header; `global['!']='8-3343'` in stage 1) |
| C2 Paths | `/0x/cls`, `/0x/ls` (plain HTTP, port 443) |
| XOR Keys | `q4FZkxX{!h,Sr3=@`, `y-p_>d$0B&@^1aQk` |
| Host Pattern | `postcss.config.js`, `ui/postcss.config.js` |
| `.gitignore` Artifacts | `temp_auto_push.bat`, `temp_interactive_push.bat`, `branch_structure.json` |

---

## Remediation Completed

1. `github/main` cleaned (`4aa46c4`); malicious objects GC'd
2. `run_instruction.md` (222 lines) rescued and verified clean
3. Zero-spread scan: all workspace repositories verified clean
4. Infisical selected for credential rotation (`.infisical.json`: `projectId: 8210a2d4...`)
5. Forensics repo published (`https://github.com/richardokonicha/blockchain-c2-loader-forensics`, `4442b06`)

---

## Remediation Pending / Recommended

- **Credential rotation** via Infisical (`infisical secrets --env prod`) — loader granted arbitrary RCE; rotate all exposed keys (`LITELLM_MASTER_KEY`, `JWT_SECRET`, DB URLs, Clerk/Stripe/Resend keys, gateway master/internal keys)
- **Target device isolation** — keep off network; do not wipe until forensics image captured (if needed); do not run `next dev` or `npm run build` from infected branch
- **Publication** — dev.to article ready; manual post available (no API key configured on this machine)

---

## Technical References

- Full payload analysis (4 stages, static only): `analysis/payload-analysis.md`
- MITRE ATT&CK mapping (`T1071.001`, `T1059.007`, `T1027`, `T1036.005`, `T1204.002`, `T1566.002`, `T1596`): `analysis/attack-mapping.md`
- IOC table (full): `iocs/ioc-table.md`
- Safe reproduction guide: `analysis/reproduction.md`
- Ethical note / disclosure status: `analysis/ethical-note.md`
- Standard review assessment (`STANDARD-REVIEW.md`): 12/12 critical criteria pass; 6 documented limitations (intentional: malicious objects purged, no live C2 contact, blockchain not actively audited)

---

## Standard Alignment

- MITRE ATT&CK (`analysis/attack-mapping.md`)
- IOC documentation (`iocs/ioc-table.md`)
- Ethical / legal handling (`analysis/ethical-note.md`)
- Safe reproduction (`analysis/reproduction.md`)
- Evidence preservation (`evidence/1b13415.diff`, `evidence/clean-commit.log`, `evidence/run_instruction.md`)
- Redacted identifiers (`REDACTED-FAMILY-MEMBER`, `COMPANY-WEB`, etc.)

---

*Report version: 1.0 | Date: 2026-08-15 | Analysis: static only, no payload execution*  
*Forensics repo: `github.com/richardokonicha/blockchain-c2-loader-forensics`*
