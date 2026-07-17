# Finding 001 – SQL Injection

## Severity

Critical

## Category

Injection

## CWE

CWE-89: Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection')

## OWASP Top 10

A03:2021 – Injection

## Location

`routes/search.ts`

## Description

The application constructs an SQL query by directly concatenating user-controlled input into the SQL statement. Because the input is not parameterized, an attacker can inject malicious SQL commands that may expose, modify, or delete sensitive data.

## Vulnerable Code

```ts
models.sequelize.query(
  `SELECT * FROM Products
   WHERE ((name LIKE '%${criteria}%'
   OR description LIKE '%${criteria}%')
   AND deletedAt IS NULL)
   ORDER BY name`
)
```

**Screenshot:** `screenshots/finding-001/001-vuln-code.png`

## Root Cause

The application directly concatenates user input into the SQL query instead of using parameterized queries. As a result, the database interprets user input as executable SQL rather than as data.

## Impact

- Unauthorized database access
- Sensitive data disclosure
- Authentication bypass (depending on the affected query)
- Data modification or deletion
- Potential full database compromise

## Evidence

### Semgrep Rule

```text
javascript.sequelize.security.audit.sequelize-injection-express.express-sequelize-injection
```

**Semgrep Output Screenshot:** `screenshots/finding-001/001-semgrep-finding.png`

## Recommended Secure Implementation

```ts
const searchTerm = `%${criteria}%`

models.sequelize.query(
  `SELECT * FROM Products
   WHERE (
     (name LIKE :searchTerm OR description LIKE :searchTerm)
     AND deletedAt IS NULL
   )
   ORDER BY name`,
  {
    replacements: {
      searchTerm
    }
  }
)
```

**Screenshot:** `screenshots/finding-001/001-secure-code.png`

## Why the Fix is Secure

The application now uses a parameterized query with Sequelize replacements instead of concatenating user input directly into the SQL statement. User input is treated as data rather than executable SQL, preventing attackers from injecting malicious SQL commands.

## Recommendation

- Use parameterized queries or prepared statements for all database operations.
- Never concatenate user input into SQL statements.
- Validate and sanitize user input where appropriate.
- Perform regular Static Application Security Testing (SAST) and code reviews to identify injection vulnerabilities early.

## References

- CWE-89: Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection')
- OWASP Top 10 2021 – A03: Injection
- OWASP SQL Injection Prevention Cheat Sheet
- Semgrep Rule: `javascript.sequelize.security.audit.sequelize-injection-express.express-sequelize-injection`