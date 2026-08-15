# Indicators of Compromise (IOCs)

## Confirmed Malware Indicators

| Category | Indicator | Context |
|---|---|---|
| ETH Wallet (C2 dead-drop) | `0xa322E5f3D311D3080e6f0121063e9aDC2490Ef1a` | Hardcoded in loader; queried via 4 public RPC endpoints |
| Campaign ID | `A8-3343-1` / `8-3343` | Header `Sec-V`; also `global['!']='8-3343'` in stage 1 |
| C2 Paths | `/0x/cls`, `/0x/ls` | Plain HTTP on port 443 |
| XOR Decryption Keys | `q4FZkxX{!h,Sr3=@`, `y-p_>d$0B&@^1aQk` | Stage 4 payload decryption |
| Obfuscation Signature | `sfL("rmcej%otb%",2857687)` | Fisher-Yates shuffle; reconstructs `require`/`object`/`module` |
| Padding Pattern | `};` followed by 500+ spaces, then payload | Delivery trick in `postcss.config.js`, `ui/postcss.config.js` |
| Host File Pattern | `postcss.config.js` injected with off-screen payload | Ideal host (auto-loaded by Next.js / Tailwind builds) |
| `.gitignore` Artifacts | `temp_auto_push.bat`, `temp_interactive_push.bat`, `branch_structure.json` | Hidden artifacts added by malicious commit |
| User-Agent Spoof | Chrome 131.0 (`Mozilla/5.0 ... Chrome/131.0.0.0`) | Stage 4 fetch header |
| Persistence Method | `spawn("node", ["-e", ...], {detached:true, stdio:"ignore", windowsHide:true}).unref()` | Detached child survives parent exit |

## Affected / Targeted Assets

| Asset | Status |
|---|---|
| `COMPANY-WEB` GitHub mirror (`GITHUB-MIRROR`) | PURGED (`github/main` → clean `4aa46c4`) |
| `COMPANY-WEB` GitLab (`origin/main`) | CLEAN — never infected |
| Production site (`COMPANY.com`) | CLEAN — built from `origin` code |
| `COMPANY-WEB` workspace repos (`COMPANY-CONSOLE`, `COMPANY-DOCS`, etc.) | ZERO SPREAD — verified clean |
| `run_instruction.md` (legitimate doc) | RESCUED — committed clean (`6702c86`) |

## References
- Blockchain dead-drop wallet: `https://etherscan.io/address/0xa322E5f3D311D3080e6f0121063e9aDC2490Ef1a` (public audit trail of C2 rotation)
- Campaign ID header `Sec-V`: tracking mechanism for deployment counting
