# Matrix Debug Drill Solution

This repository contains the reference solution for the LU 2.7 matrix debugging assignment. It includes fixes for cross-platform build failures, an updated GitHub Actions matrix workflow, and complete diagnostic documentation.

## Matrix Architecture

The GitHub Actions workflow runs the following combinations:

| OS | Node Version | Production Supported | Experimental | Fix Applied |
|---|---|---|---|---|
| ubuntu-latest | 18 | Yes | No | Path handling and crypto compatibility |
| ubuntu-latest | 20 | Yes | No | Path handling and crypto compatibility |
| ubuntu-latest | 22 | Yes | No | Path handling and crypto compatibility |
| windows-latest | 18 | Yes | No | Path handling and line ending normalization |
| windows-latest | 20 | Yes | No | Path handling and line ending normalization |
| windows-latest | 22 | Yes | No | Path handling and line ending normalization |
| ubuntu-latest | 24 | No | Yes | Experimental Node 24 validation with continue-on-error |

## What Changed

- `src/fileUtils.js`
  - Replaced hard-coded path concatenation with `path.join` for cross-platform path generation.
  - Added comments explaining why the fix is version- and OS-compatible.
- `src/fileUtils.test.js`
  - Normalized `CRLF` line endings to `LF` in test assertions.
  - Ensures tests pass consistently across Windows and Unix environments.
- `src/cryptoUtils.js`
  - Replaced deprecated `crypto.createCipher`/`createDecipher` usage with `crypto.createCipheriv`/`createDecipheriv`.
  - Added explicit IV generation and prefixing so decryption remains deterministic and secure.
- `.github/workflows/ci.yml`
  - Set `fail-fast: false` to ensure all matrix combinations run to completion.
  - Added Node `24` as an experimental combination with `continue-on-error` enabled.

## How to Validate

Run the test suite locally:

```bash
npm test
```

This solution is designed to support the production matrix while also surfacing an experimental Node 24 environment without blocking delivery.
