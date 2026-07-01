# MATRIX AUDIT

This audit records the matrix combinations that failed during the compatibility run and the fixes applied to resolve them.

| Combination | Failing step | Exact error line from the log | Failure type | Fix applied |
| --- | --- | --- | --- | --- |
| windows-latest + Node 18 | File loading in `src/fileUtils.js` | `Error: ENOENT: no such file or directory, open 'C:\workspace\matrix-debug-drill/src/configs/default.json'` | OS-specific | Replaced string-based path construction with `path.join(...)` so config files resolve correctly on Windows. |
| windows-latest + Node 20 | File loading in `src/fileUtils.js` | `Error: ENOENT: no such file or directory, open 'C:\workspace\matrix-debug-drill/src/configs/default.json'` | OS-specific | Replaced string-based path construction with `path.join(...)` so config files resolve correctly on Windows. |
| windows-latest + Node 22 | File reading assertion in `src/fileUtils.test.js` | `Received: "line one\r\nline two\r\nline three\r\n"` | OS-specific | Normalized CRLF to LF before comparing file content so tests pass across Windows and Ubuntu. |
| ubuntu-latest + Node 22 | Crypto API usage in `src/cryptoUtils.js` | `TypeError: crypto.createCipher is not a function` | Runtime version | Replaced deprecated `crypto.createCipher` / `crypto.createDecipher` with `crypto.createCipheriv` / `crypto.createDecipheriv`. |

## Workflow update

- File: `.github/workflows/ci.yml`
- Change: kept `fail-fast: false`, added the experimental Node 24 entry with `continue-on-error`, and preserved full matrix visibility for all supported runtimes.
- Rationale: compatibility matrices should expose every environment issue instead of stopping at the first one, and the experimental Node 24 job should not block the main matrix result.
