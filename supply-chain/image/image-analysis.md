# Container Image Analysis

## Purpose

This document analyzes the OWASP Juice Shop Docker image to identify software components, generate a Software Bill of Materials (SBOM), and detect known vulnerabilities present in the container image.

---

# Assessment Method

The container image was analyzed using the following tools:

- Syft – Generated the Software Bill of Materials (SBOM)
- SPDX – Generated a standardized SBOM for interoperability and compliance
- Grype – Scanned the SBOM for known vulnerabilities

---

# Generated Artifacts

| Artifact | Purpose |
|----------|---------|
| juice-shop-sbom.json | Native Syft Software Bill of Materials |
| juice-shop-sbom-spdx.json | SPDX-formatted Software Bill of Materials |
| juice-shop-vulnerabilities.json | Grype vulnerability scan results |

---

# Vulnerability Summary

| Severity | Count |
|----------|------:|
| Critical | 8 |
| High | 51 |
| Medium | 37 |
| Low | 6 |
| Negligible | 7 |
| **Total** | **109** |

---

# Fix Availability

| Status | Count |
|--------|------:|
| Fix Available | 93 |
| No Fix Available | 16 |

---

# Key Findings

The vulnerability scan identified security issues across multiple layers of the container image.

### npm Packages

Examples include:

- jsonwebtoken
- lodash
- sanitize-html
- express-jwt
- multer
- socket.io
- tar
- ws
- crypto-js
- moment

Several of these packages contain Critical or High severity vulnerabilities and should be updated to patched versions where possible.

---

### Operating System Packages

The scan also identified vulnerabilities in operating system packages, including:

- libc6
- libssl3t64
- zlib1g

These vulnerabilities originate from the base image and require updating the underlying operating system packages or rebuilding the image using a newer base image.

---

### Runtime Components

The Node.js runtime (24.15.0) was also found to contain known vulnerabilities, indicating that the runtime should be upgraded to a patched release.

---

# Risk Assessment

The container image presents a **High** software supply chain risk due to the presence of multiple Critical and High severity vulnerabilities across application dependencies, runtime components, and operating system libraries.

It is important to note that OWASP Juice Shop is intentionally vulnerable for security training. Therefore, these findings are expected and should not be considered representative of a production-ready application.

---

# Conclusion

The container image assessment identified **109 known vulnerabilities**, affecting third-party npm packages, the Node.js runtime, and operating system libraries. Regular image scanning, dependency updates, and rebuilding container images with patched base images are essential to reduce software supply chain risk in production environments.