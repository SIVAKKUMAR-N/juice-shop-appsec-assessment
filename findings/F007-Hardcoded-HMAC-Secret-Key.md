# Finding 007 – Hardcoded HMAC Secret Key

## Severity

High

## Category

Cryptographic Failures

## CWE

CWE-798: Use of Hard-coded Credentials

## OWASP Top 10

A02:2021 – Cryptographic Failures

## Location

**File:** `lib/insecurity.ts`

## Description

The application uses a hardcoded secret key to generate HMAC (Hash-based Message Authentication Code) values. Since the secret is embedded directly in the source code, anyone with access to the codebase can obtain the key and generate valid HMAC values. Hardcoded cryptographic secrets violate secure key management practices and may allow attackers to forge signed data if the HMAC is used for authentication, session validation, API request signing, or integrity verification.

## Vulnerable Code

```ts
export const hmac = (data: string) =>
  crypto
    .createHmac('sha256', 'pa4qacea4VK9t9nGv7yZtwmj')
    .update(data)
    .digest('hex')
```

**Screenshot:** `screenshots/finding-007/007-vuln-code.png`

---

## Root Cause

The HMAC secret key is hardcoded directly into the application's source code instead of being securely managed through environment variables or a dedicated secrets management solution. If the application's source code is exposed through source control leaks, backups, or server compromise, the secret key is immediately compromised.

---

## Impact

- Disclosure of cryptographic secrets
- Forgery of valid HMAC signatures
- Potential authentication bypass
- Tampering with signed data
- Loss of data integrity and trust

---

## Evidence

**Detection Tool:** Semgrep

**Rule ID**

```text
javascript.lang.security.audit.hardcoded-hmac-key.hardcoded-hmac-key
```

**Finding Summary**

Semgrep detected a hardcoded secret key passed directly to the `crypto.createHmac()` function. Manual analysis confirmed that the cryptographic key is embedded in the application source code rather than being loaded securely at runtime.

**Evidence Screenshot:**

`screenshots/finding-007/007-semgrep-finding.png`

---

## Secure Fix

```ts
export const hmac = (data: string) =>
  crypto
    .createHmac('sha256', process.env.HMAC_SECRET!)
    .update(data)
    .digest('hex')
```

Example:

```env
HMAC_SECRET=9d2cfb7c0d6b9c8c9c9f7f7f6e1c8ab5c1f2d3e4f5a6b7c8d9e0f1a2b3c4d5e
```

**Screenshot:** `screenshots/finding-007/007-secure-code.png`

---

## Why the Fix is Secure

The HMAC secret is removed from the source code and loaded from an environment variable at runtime. This prevents the secret from being exposed through the application's source repository and allows the key to be rotated without modifying the application's code. Secure secret management significantly reduces the risk of key disclosure and unauthorized generation of valid HMAC signatures.

---

## Recommendation

- Remove all hardcoded cryptographic secrets from the source code.
- Store HMAC keys in environment variables or a secure secrets management service.
- Rotate compromised or previously hardcoded keys immediately.
- Restrict access to cryptographic keys using the principle of least privilege.
- Implement periodic key rotation as part of the application's security policy.

---

## References

- CWE-798: Use of Hard-coded Credentials
- OWASP Top 10 2021 – A02: Cryptographic Failures
- OWASP Cryptographic Storage Cheat Sheet
- Semgrep Rule: `javascript.lang.security.audit.hardcoded-hmac-key.hardcoded-hmac-key`