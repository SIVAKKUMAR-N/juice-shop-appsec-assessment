# Finding 008 – Path Traversal via User-Controlled File Download

## Severity

High

## Category

Path Traversal

## CWE

CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')

## OWASP Top 10

A01:2021 – Broken Access Control

## Location

**File:** `routes/fileServer.ts`

## Description

The application allows users to request files from the `ftp/` directory. Although it validates file extensions and removes poison null bytes, these checks do not prevent directory traversal. The application fails to verify that the canonicalized file path remains within the intended `ftp/` directory before passing it to `res.sendFile()`. As a result, an attacker may use directory traversal sequences (e.g., `../`) to access files outside the authorized directory.

## Vulnerable Code

```ts
function verify(file: string, res: Response, next: NextFunction) {
  if (
    file &&
    (endsWithAllowlistedFileType(file) ||
      file === 'incident-support.kdbx')
  ) {
    file = security.cutOffPoisonNullByte(file)

    res.sendFile(path.resolve('ftp/', file))
  } else {
    res.status(403)
    next(new Error('Only .md and .pdf files are allowed!'))
  }
}
```

**Screenshot:** `screenshots/finding-008/008-vuln-code.png`

---

## Root Cause

The application accepts a filename from user input and constructs the file path using `path.resolve()`. While file extensions are validated, the application does not verify that the resolved path remains within the intended `ftp/` directory. Consequently, directory traversal sequences such as `../` can cause the resolved path to escape the intended directory and access arbitrary files on the server.

---

## Impact

- Unauthorized access to server files
- Disclosure of sensitive information
- Exposure of application source code and configuration files
- Leakage of credentials or cryptographic material
- Increased attack surface for further compromise

---

## Evidence

**Detection Tool:** Semgrep

**Rule ID**

```text
javascript.express.security.audit.express-res-sendfile.express-res-sendfile
```

**Finding Summary**

Semgrep detected that user-controlled input is passed to `res.sendFile()` after being resolved using `path.resolve()`. Although the application validates file extensions, it does not verify that the canonicalized path remains within the intended `ftp/` directory. This may allow attackers to perform directory traversal and read arbitrary files from the server.

**Evidence Screenshot:**

`screenshots/finding-008/008-semgrep-finding.png`

---

## Secure Fix

```ts
const baseDir = path.resolve('ftp')
const requestedFile = path.resolve(baseDir, file)

if (!requestedFile.startsWith(baseDir + path.sep)) {
  return res.status(403).send('Access denied')
}

res.sendFile(requestedFile)
```

**Screenshot:** `screenshots/finding-008/008-secure-code.png`

---

## Why the Fix is Secure

The fix canonicalizes both the base directory and the requested file path before serving the file. It verifies that the resolved path begins with the expected base directory, preventing attackers from escaping the `ftp/` directory using traversal sequences such as `../`. This ensures that only authorized files within the intended directory can be accessed.

---

## Recommendation

- Validate the canonical path before serving files.
- Ensure resolved file paths remain within the intended base directory.
- Do not rely solely on file extension validation.
- Prefer allowlisting specific filenames instead of accepting arbitrary file paths.
- Log and monitor repeated directory traversal attempts.

---

## References

- CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- OWASP Top 10 2021 – A01: Broken Access Control
- OWASP Path Traversal Cheat Sheet
- Semgrep Rule: `javascript.express.security.audit.express-res-sendfile.express-res-sendfile`