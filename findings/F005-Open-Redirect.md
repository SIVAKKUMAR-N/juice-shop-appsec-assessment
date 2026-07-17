# Finding 005 – Open Redirect due to Improper URL Validation

## Severity

Medium

## Category

Broken Access Control

## CWE

CWE-601: URL Redirection to Untrusted Site ('Open Redirect')

## OWASP Top 10

A01:2021 – Broken Access Control

## Location

**Validation:** `lib/insecurity.ts`

**Redirect Endpoint:** `routes/redirect.ts`

## Description

The application allows users to specify a redirect destination through the `to` query parameter. Although the application validates the supplied URL against an allowlist, the validation uses `String.includes()`, which only checks whether an allowed domain appears anywhere within the URL. An attacker can craft a malicious URL containing an allowlisted domain as part of the string and bypass the validation, causing users to be redirected to an attacker-controlled website.

## Vulnerable Code

```ts
export const isRedirectAllowed = (url: string) => {
  let allowed = false

  for (const allowedUrl of redirectAllowlist) {
    allowed = allowed || url.includes(allowedUrl)
  }

  return allowed
}
```

```ts
if (security.isRedirectAllowed(toUrl)) {
    res.redirect(toUrl)
}
```

**Screenshot:** `screenshots/finding-005/005-vuln-code.png`

---

## Root Cause

The application validates redirect destinations using `String.includes()` instead of verifying the actual destination's origin or hostname. As a result, any URL containing an allowlisted domain as part of its string can bypass the validation.

---

## Impact

- Open Redirect
- Phishing attacks
- Social engineering
- Redirection to attacker-controlled websites
- Loss of user trust

---

## Evidence

**Detection Tool:** Semgrep

**Rule ID**

```text
javascript.express.security.audit.express-open-redirect.express-open-redirect
```

**Finding Summary**

Semgrep identified a potential Open Redirect because user-controlled input is passed to `res.redirect()`. Manual code review confirmed that the validation relies on `String.includes()`, allowing attacker-controlled URLs containing an allowlisted domain to bypass the security check.

**Evidence Screenshot:**

`screenshots/finding-005/005-semgrep-finding.png`

---

## Secure Fix

```ts
export const isRedirectAllowed = (url: string) => {
  try {
    const parsed = new URL(url)

    return redirectAllowlist.some(
      allowed => parsed.origin === new URL(allowed).origin
    )
  } catch {
    return false
  }
}
```

**Screenshot:** `screenshots/finding-005/005-secure-code.png`

---

## Why the Fix is Secure

The fix parses the supplied URL and compares its origin against the trusted allowlist instead of performing string matching. This ensures that only URLs belonging to trusted domains are accepted and prevents attackers from bypassing validation by embedding an allowlisted domain within a malicious URL.

---

## Recommendation

- Validate redirect destinations using the parsed URL origin or hostname.
- Avoid using string matching methods such as `includes()` for URL validation.
- Restrict redirects to trusted domains maintained in an allowlist.
- Consider using internal route identifiers instead of accepting arbitrary redirect URLs.

---

## References

- CWE-601: URL Redirection to Untrusted Site ('Open Redirect')
- OWASP Top 10 2021 – A01: Broken Access Control
- OWASP Unvalidated Redirects and Forwards Cheat Sheet
- Semgrep Rule: `javascript.express.security.audit.express-open-redirect.express-open-redirect`