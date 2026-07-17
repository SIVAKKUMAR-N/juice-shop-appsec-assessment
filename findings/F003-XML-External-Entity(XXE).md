# Finding 003 – XML External Entity (XXE) Injection

## Severity

High

## Category

Security Misconfiguration

## CWE

CWE-611: Improper Restriction of XML External Entity Reference

## OWASP Top 10

A05:2021 – Security Misconfiguration

## Location

`routes/fileUpload.ts`

## Description

The application processes user-uploaded XML files using the `libxml.parseXml()` function with XML entity expansion enabled (`noent: true`). Because the uploaded XML content is fully controlled by the user, an attacker can supply malicious XML containing external entities. This may allow disclosure of sensitive local files, server-side request forgery (SSRF), or denial-of-service attacks depending on the parser configuration and deployment environment.

## Vulnerable Code

```ts
const data = file.buffer.toString()

const sandbox = { libxml, data }
vm.createContext(sandbox)

const xmlDoc = vm.runInContext(
  'libxml.parseXml(data, { noblanks: true, noent: true, nocdata: true })',
  sandbox,
  { timeout: 2000 }
)
```

**Screenshot:** `screenshots/finding-003/003-vuln-code.png`

## Root Cause

The application parses user-controlled XML while enabling XML entity expansion (`noent: true`). As a result, external entities defined within attacker-supplied XML may be resolved by the XML parser, allowing access to local files or other internal resources.

## Impact

- Disclosure of sensitive local files (e.g., `/etc/passwd`, `C:\Windows\win.ini`)
- Server-Side Request Forgery (SSRF)
- Information disclosure
- Denial of Service (e.g., XML entity expansion attacks)
- Potential compromise of internal systems

## Evidence

### Semgrep Rule

```text
javascript.express.security.audit.express-libxml-vm-noent.express-libxml-vm-noent
```

**Semgrep Output Screenshot:** `screenshots/finding-003/003-semgrep-finding.png`

## Recommended Secure Implementation

```ts
const xmlDoc = libxml.parseXml(data, {
  noblanks: true,
  noent: false,
  nocdata: true
})
```

**Screenshot:** `screenshots/finding-003/003-secure-code.png`

## Why the Fix is Secure

The parser no longer expands XML entities because `noent` is disabled. This prevents attacker-controlled XML from resolving external entities, mitigating XXE attacks. If XML parsing is required, the parser should also disable DTD processing and external entity resolution wherever possible.

## Recommendation

- Disable XML entity expansion (`noent: false`).
- Disable DTD processing unless explicitly required.
- Use securely configured XML parsers for all untrusted XML.
- Validate uploaded XML before processing.
- Prefer safer data formats such as JSON when XML support is unnecessary.

## References

- CWE-611: Improper Restriction of XML External Entity Reference
- OWASP Top 10 2021 – A05: Security Misconfiguration
- OWASP XML External Entity Prevention Cheat Sheet
- Semgrep Rule: `javascript.express.security.audit.express-libxml-vm-noent.express-libxml-vm-noent`