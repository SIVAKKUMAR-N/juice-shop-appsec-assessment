# Finding 002 – Hardcoded RSA Private Key

## Severity

Critical

## Category

Cryptographic Failures

## CWE

CWE-798: Use of Hard-coded Credentials

## OWASP Top 10

A02:2021 – Cryptographic Failures

## Location

**File:** `lib/insecurity.ts`

## Description

The application stores an RSA private key directly in the source code. If an attacker gains access to the application's source code or repository, the private key can be extracted and used to forge JWTs, compromising the application's authentication mechanism.

## Vulnerable Code

```ts
const privateKey = '-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----'
```

**Screenshot:** `screenshots/finding-002/002-vuln-code.png`

## Root Cause

A cryptographic private key is embedded directly in the application's source code instead of being loaded securely from an external secret management mechanism. Hardcoding sensitive secrets increases the risk of accidental disclosure through source code repositories, backups, or leaked artifacts.

## Impact

- Forging valid JWTs
- Authentication bypass
- Privilege escalation
- Exposure of sensitive cryptographic material
- Loss of integrity and trust in the authentication system

## Evidence

# Semgrep Rule

```text
javascript.jsonwebtoken.security.jwt-hardcode.hardcoded-jwt-secret
```

**Semgrep Output Screenshot:** `screenshots/finding-002/002-semgrep-finding.png`

**Finding Summary**

Semgrep detected a hardcoded RSA private key embedded directly in the application's source code. Manual verification confirmed that the private key is stored as a string literal in `lib/insecurity.ts`, making it accessible to anyone with source code access.

**Evidence Screenshot:**

`screenshots/finding-002/002-semgrep-finding.png`

## Secure Fix

```ts
const privateKey = fs.readFileSync(
  process.env.JWT_PRIVATE_KEY_PATH!,
  'utf8'
)
```

**Screenshot:** `screenshots/finding-002/002-secure-code.png`

## Why the Fix is Secure

The private key is no longer embedded in the application's source code. Instead, it is loaded at runtime from a secure location specified by an environment variable. This approach significantly reduces the risk of exposing cryptographic secrets through the source repository and enables integration with secure secret management solutions such as AWS Secrets Manager, Azure Key Vault, or HashiCorp Vault.

## Recommendation

- Remove all hardcoded cryptographic secrets from the source code.
- Store private keys in a secure secret management solution or protected key store.
- Rotate the exposed RSA private key immediately.
- Enable automated secret scanning in the CI/CD pipeline to prevent future commits containing sensitive credentials.

## References

- CWE-798: Use of Hard-coded Credentials
- OWASP Top 10 2021 – A02: Cryptographic Failures
- OWASP Secrets Management Cheat Sheet
- Semgrep Rule: `javascript.jsonwebtoken.security.jwt-hardcode.hardcoded-jwt-secret`