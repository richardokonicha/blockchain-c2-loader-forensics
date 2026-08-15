# MITRE ATT&CK Mapping

This analysis is static only; no execution was performed. Technique assignments are based on observed behavior in the payload code, not runtime confirmation.

| ATT&CK ID | Technique Name | Observation / Evidence |
|---|---|---|
| T1071.001 | Application Layer Protocol: Web Protocols | C2 fetch over `http://ip>:443` (plain HTTP on TLS port) |
| T1059.007 | Command and Scripting Interpreter: JavaScript / Node.js | `spawn("node", ["-e", ...], ...)` — detached Node child |
| T1059.004 | Command and Scripting Interpreter: Unix Shell | Not directly observed; loader is Node-native |
| T1055.012 | Process Hollowing | Not observed; loader uses detached spawn (not injection) |
| T1071.004 | Application Layer Protocol: DNS / Blockchain | ETH wallet used as C2 dead-drop (non-standard protocol abuse) |
| T1027 | Obfuscated Files or Information | `\u00xx` escapes, `sfL()` shuffle, padding trick, XOR encryption |
| T1036.005 | Masquerade: Match Legitimate Name or Location | `postcss.config.js` (legitimate build config) as host file |
| T1083 | File and Directory Discovery | Not directly observed in loader; stage 4 capability unknown |
| T1531 | Account Access Removal / Persistence | Not observed; loader provides RCE, not persistence directly |
| T1566.002 | Phishing: Spearphishing Link / Attachment | Delivery via fake coding test / repo share (inferred from infection vector) |
| T1596 | Search Open Technical Databases: Blockchain / Crypto | Multi-RPC lookup (`1rpc.io`, `drpc.org`, `publicnode.com`) blends with developer traffic |
| T1070.001 | Indicator Removal: File Deletion | Not confirmed; malicious `.gitignore` additions (`temp_auto_push.bat`) suggest cleanup artifacts |
| T1204.002 | User Execution: Malicious File / Project Build | Trigger: `next dev` or `next build` (PostCSS auto-load) — no user interaction needed |

---

## Notes on Attribution
No confirmed MITRE group mapping. Behavioral resemblance to DPRK-linked developer-targeting clusters (`Contagious Interview` / Marstech) is noted but not assigned as a confirmed ATT&CK group association.
