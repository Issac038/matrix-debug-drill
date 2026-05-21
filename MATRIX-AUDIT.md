# MATRIX AUDIT

This audit documents the three environment failures discovered during the LU 2.7 matrix debugging assignment and the fixes applied to resolve them.

---

## Failure 1

- Combination: windows-latest, Node 18/20/22
- Step: Application path resolution in `src/fileUtils.js`
- Log evidence:
  - `Error: ENOENT: no such file or directory, open 'C:\workspace\matrix-debug-drill/src/configs/default.json'`
- Category: OS / Path compatibility
- Fix applied:
  - Replaced string concatenation with `path.join(__dirname, 'configs', configName + '.json')`.
  - This ensures the correct path separator is used on Windows and Unix systems, making file loading cross-platform.

---

## Failure 2

- Combination: windows-latest, Node 18/20/22
- Step: Unit test assertion in `src/fileUtils.test.js`
- Log evidence:
  - `Expected: "line one\nline two\nline three\n"
    Received: "line one\r\nline two\r\nline three\r\n"`
- Category: OS / Runtime behavior
- Fix applied:
  - Normalized CRLF to LF before comparison in the test using `content.replace(/\r\n/g, '\n')`.
  - This makes the test robust across Windows and Linux line-ending conventions.

---

## Failure 3

- Combination: ubuntu-latest, Node 22
- Step: Crypto API usage in `src/cryptoUtils.js`
- Log evidence:
  - `Error: crypto.createCipher is not a function`
- Category: Runtime version
- Fix applied:
  - Replaced deprecated `crypto.createCipher` and `crypto.createDecipher` with `crypto.createCipheriv` and `crypto.createDecipheriv`.
  - Added explicit IV generation and prepending to the encrypted payload so decryption remains compatible and secure.

---

## Workflow Update

- File: `.github/workflows/ci.yml`
- Change: Set `fail-fast: false` and added experimental `node: 24` on `ubuntu-latest`.
- Rationale: A compatibility matrix must complete all combinations so failures are visible, and Node 24 is evaluated without blocking production delivery.
