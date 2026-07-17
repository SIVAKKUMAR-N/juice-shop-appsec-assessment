# Finding 006 – Directory Listing Enabled

## Severity

Medium

## Category

Security Misconfiguration

## CWE

CWE-548: Exposure of Information Through Directory Listing

## OWASP Top 10

A05:2021 – Security Misconfiguration

## Location

**File:** `server.ts`

---

## Description

The application enables directory listing using the `serve-index` middleware. When a user requests certain directories (such as `/ftp`), the server automatically generates an index page displaying all files within that directory. This allows attackers to enumerate publicly accessible files and directories, increasing the risk of sensitive information disclosure if confidential files are stored within these locations.

---

## Vulnerable Code

```ts
app.use('/ftp',
    serveIndexMiddleware,
    serveIndex('ftp', { icons: true }))
```

**Screenshot:**

`screenshots/finding-006/006-vuln-code.png`

---

## Root Cause

The application explicitly enables directory browsing by registering the `serve-index` middleware. Instead of denying requests to directory paths or serving only intended static resources, the server generates a listing of all files contained in the configured directory.

---

## Impact

- Disclosure of directory structure
- Enumeration of publicly accessible files
- Increased likelihood of sensitive information disclosure
- Facilitates reconnaissance for further attacks
- May expose backup files, logs, configuration files, or other unintended resources

---

## Evidence

**Detection Tool:** Semgrep

**Rule ID**

```text
javascript.express.security.audit.express-check-directory-listing.express-check-directory-listing
```

**Finding Summary**

Semgrep detected the use of the `serve-index` middleware, which enables directory indexing. Manual analysis confirmed that requests to configured paths (for example, `/ftp`) generate a directory listing instead of denying access or serving only intended files.

**Evidence Screenshot:**

`screenshots/finding-006/006-semgrep-finding.png`

---

## Secure Fix

```ts
import express from 'express'

app.use('/ftp', express.static('ftp'))
```

Alternatively, if directory browsing is not required, remove the `serve-index` middleware entirely.

**Screenshot:**

`screenshots/finding-006/006-secure-code.png`

---

## Why the Fix is Secure

Using `express.static()` serves only explicitly requested files and does not generate a directory listing. Users must know the exact filename to request a resource, preventing attackers from enumerating the contents of the directory. This reduces information disclosure and follows the principle of least privilege by exposing only intended resources.

---

## Recommendation

- Disable directory listing for all non-public directories.
- Remove unnecessary use of the `serve-index` middleware.
- Store sensitive files (backups, logs, configuration files, keys) outside publicly accessible directories.
- Restrict access to directories that require authentication or authorization.
- Periodically review exposed directories to ensure only intended files are publicly available.

---

## References

- CWE-548: Exposure of Information Through Directory Listing
- OWASP Top 10 2021 – A05: Security Misconfiguration
- OWASP Directory Listing Guidance
- Semgrep Rule: `javascript.express.security.audit.express-check-directory-listing.express-check-directory-listing`