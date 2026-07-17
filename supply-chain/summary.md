# Supply Chain Security Assessment Summary

## Purpose

This document summarizes the software supply chain security assessment performed on the OWASP Juice Shop application. The assessment covered both application dependencies and the container image to identify known vulnerabilities and evaluate software supply chain risks.

---

# Assessment Scope

The following components were assessed:

- Source code dependencies
- Docker container image
- Software Bill of Materials (SBOM)
- Third-party packages
- Operating system packages
- Node.js runtime

---

# Assessment Activities

## Source Code Assessment

The application dependencies were reviewed using:

- package.json
- npm audit

The assessment identified multiple vulnerable third-party packages requiring updates or replacement.

---

## Container Image Assessment

The Docker image was analyzed using:

- Syft
- SPDX SBOM
- Grype

The assessment identified vulnerabilities affecting:

- npm packages
- Node.js runtime
- Operating system libraries

---

# Key Findings

## Source Code

- Multiple vulnerable npm dependencies were identified.
- Several packages have security updates available.
- Continuous dependency monitoring is recommended.

---

## Container Image

The Grype scan identified:

- **109 total vulnerabilities**
- **8 Critical**
- **51 High**
- **37 Medium**
- **6 Low**
- **7 Negligible**

Most vulnerabilities originated from:

- Outdated npm packages
- Node.js runtime
- Operating system packages

---

# Overall Risk Assessment

The overall software supply chain risk is assessed as **High** due to the presence of multiple Critical and High severity vulnerabilities across application dependencies and the container image.

It is important to note that OWASP Juice Shop is intentionally vulnerable for security training purposes. Therefore, these findings are expected and do not represent a production-ready application.

---

# Recommendations

The assessment recommends:

- Regular dependency updates
- Continuous dependency scanning
- Periodic container image scanning
- Updating base images
- Automated SBOM generation
- Integrating supply chain security checks into the CI/CD pipeline

---

# Conclusion

The software supply chain assessment successfully identified vulnerabilities across both application dependencies and the container image. Regular vulnerability scanning, timely updates, and automated security controls are essential for reducing software supply chain risk and maintaining a secure software development lifecycle.