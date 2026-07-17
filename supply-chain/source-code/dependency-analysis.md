# Source Dependency Analysis

## Purpose

This document analyzes the third-party dependencies used by OWASP Juice Shop to understand the application's software supply chain and identify potential security risks associated with external packages.

---

# Package Manager

- Package Manager: npm

---

# Dependency Summary

| Category | Count |
|----------|------:|
| Runtime Dependencies | 67 |
| Development Dependencies | 60 |
| Total Declared Dependencies | 127 |

---

# Key Runtime Dependencies

The application relies on several important runtime libraries to provide its core functionality.

| Package | Purpose |
|----------|----------|
| express | Web application framework |
| sqlite3 | SQLite database driver |
| sequelize | Object Relational Mapper (ORM) |
| jsonwebtoken | JWT creation and verification |
| express-jwt | JWT authentication middleware |
| helmet | HTTP security headers |
| express-rate-limit | API rate limiting |
| multer | File upload handling |
| socket.io | Real-time communication |
| libxmljs2 | XML parsing |
| sanitize-html | HTML sanitization |
| serve-index | Directory listing functionality |

---

# Key Development Dependencies

The project also uses several development dependencies for testing, code quality, packaging, and TypeScript development.

Examples include:

- TypeScript
- Cypress
- ESLint
- Mocha
- Supertest
- Sinon
- CycloneDX npm
- Concurrently

---

# Security-Relevant Dependencies

Several packages directly contribute to the application's security posture.

| Package | Security Function |
|----------|-------------------|
| helmet | Adds HTTP security headers |
| express-rate-limit | Mitigates brute-force and DoS attacks |
| jsonwebtoken | JWT generation and verification |
| express-jwt | JWT authentication middleware |
| sanitize-html | Prevents Cross-Site Scripting (XSS) |
| sanitize-filename | Prevents malicious file names |
| cors | Controls Cross-Origin Resource Sharing |

---

# Observations

- The project declares **127 third-party dependencies**, increasing the application's software supply chain attack surface.
- Multiple packages implement critical security functionality such as authentication, input sanitization, and rate limiting.
- The repository does **not include a `package-lock.json` file**, preventing verification of the exact dependency versions from the repository alone.
- Dependency vulnerability analysis will be performed using **npm audit** on the installed project dependencies.
- Since OWASP Juice Shop is intentionally vulnerable for security training, some dependencies may be outdated or intentionally insecure.

---

# Next Steps

The dependency inventory will be analyzed using **npm audit** to identify:

- Vulnerable packages
- Known CVEs
- Severity levels
- Recommended package updates
- Dependency remediation recommendations