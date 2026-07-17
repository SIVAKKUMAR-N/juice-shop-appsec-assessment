# Finding 004 – Eval Injection through User-Controlled Username

## Severity

High

## Category

Injection

## CWE

CWE-95: Improper Neutralization of Directives in Dynamically Evaluated Code ('Eval Injection')

## OWASP Top 10

A03:2021 – Injection

## Location

**File:** `routes/userProfile.ts`

## Description

The application evaluates user-controlled data using JavaScript's `eval()` function. A username matching the `#{...}` pattern is processed and the contents are executed as JavaScript. Since usernames originate from user input and are stored in the database, an attacker can inject JavaScript code that is executed when the user's profile is rendered.

## Vulnerable Code

```ts
let username = user.username

if (username?.match(/#{(.*)}/) !== null) {
    const code = username.substring(2, username.length - 1)
    username = eval(code)
}
```

**Screenshot:** `screenshots/finding-004/004-vuln-code.png`

---

## Root Cause

The application treats user-controlled input as executable JavaScript instead of plain text. After extracting the contents between `#{` and `}`, the application passes the resulting string directly to `eval()`, allowing dynamic code execution.

---

## Impact

- Execution of attacker-controlled JavaScript
- Server-side code injection
- Potential information disclosure
- Application compromise
- Loss of application integrity

---

## Evidence

**Detection Tool:** Semgrep

**Rule ID**

```text
javascript.browser.security.eval-detected.eval-detected
```

**Finding Summary**

Semgrep detected the use of `eval()`. Manual analysis confirmed that the evaluated string originates from the user's username, which is attacker-controlled data stored in the database. The application extracts the expression enclosed within `#{...}` and executes it using `eval()`.

**Evidence Screenshot:**

`screenshots/finding-004/004-semgrep-finding.png`

---

## Secure Fix

```ts
let username = user.username
// additional defense
if (username) {
    username = entities.encode(username)
}
```

**Screenshot:** `screenshots/finding-004/004-secure-code.png`

---

## Why the Fix is Secure

The fix removes the use of `eval()` entirely and treats the username as plain text. HTML encoding ensures that user-supplied content is displayed safely without being interpreted or executed as JavaScript. Separating user data from executable code eliminates the possibility of Eval Injection.

---

## Recommendation

- Remove all uses of `eval()` for processing user input.
- Treat usernames as data rather than executable code.
- Encode user-controlled output before rendering it in HTML.
- Use secure template engines instead of dynamic code evaluation.

---

## References

- CWE-95: Improper Neutralization of Directives in Dynamically Evaluated Code
- OWASP Top 10 2021 – A03: Injection
- OWASP Injection Prevention Cheat Sheet
- Semgrep Rule: `javascript.browser.security.eval-detected.eval-detected`