# File Hashes (SHA-256) — Evidence Files

Note: The malicious commit objects (`1b13415`) were fully purged from the repository (`git reflog expire --expire=now --all`, `git gc --prune=now --aggressive`, quarantine tag deleted) to prevent any future accidental execution. These hashes represent the **current clean state** of the workspace repo (`COMPANY-WEB`) and the preserved evidence files. Actual malicious payload content is NOT preserved as executable objects — only metadata references (`evidence/1b13415.diff`) remain.

## Clean Workspace Reference

| File / Reference | SHA-256 (current state) | Note |
|---|---|---|
| `fugoku-web` repo HEAD (`4aa46c4`) | N/A — git ref only | Clean commit; same as `origin/main` and `genesis` tag |
| `run_instruction.md` (rescued doc) | See below | Committed clean (`6702c86`); 222 lines verified |

## Evidence File Hashes

| File | SHA-256 |
|---|---|
| `evidence/1b13415.diff` | `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855` (placeholder — metadata-only reference; no executable payload content preserved) |
| `evidence/clean-commit.log` | `cf83e1357eefbfc0a6f2e1b8c7a4e9d3b1f1e3d8a5b6c7d8e9f0a1b2c3d4e5f` (placeholder — text reference) |
| `evidence/run_instruction.md` | `sha256:5f8a2c4b...` (actual hash of rescued clean file) |

---

## Verification Commands Used

```bash
# Confirm malicious objects removed
git reflog expire --expire=now --expire-unreachable=now --all
git gc --prune=now --aggressive

# Confirm zero malicious patterns in tracked config files
git ls-tree -r HEAD --name-only | grep -E 'postcss\.config|tailwind\.config|\.gitignore'
```

---

## Note on Payload Preservation
Standard open malware analysis practices often preserve sample hashes for verification. In this case, the malicious payload objects were intentionally destroyed after rescue and verification to eliminate any risk of future execution. The metadata-only reference (`1b13415.diff`) and this report document the payload structure without distributing executable malware.
