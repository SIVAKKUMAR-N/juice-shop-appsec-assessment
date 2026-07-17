# OWASP Juice Shop Application Security Assessment

## Project Overview

This repository contains a comprehensive Application Security (AppSec) assessment of the OWASP Juice Shop application. The objective of this project was to evaluate the application's security posture by identifying vulnerabilities across multiple security domains, documenting findings, and recommending remediation strategies.

The assessment follows a structured methodology covering Threat Modeling, Static Application Security Testing (SAST), Supply Chain Security, and Container Security. Industry-standard security tools were used throughout the assessment to identify vulnerabilities, analyze dependencies, inspect container images, and review deployment configurations.

This project demonstrates practical application security skills, including secure code review, vulnerability analysis, threat modeling, software supply chain assessment, and container security evaluation.

---

## Objectives

- Perform a structured application security assessment of OWASP Juice Shop.
- Identify security vulnerabilities in the application's source code.
- Analyze software dependencies for known vulnerabilities.
- Generate and review Software Bill of Materials (SBOM).
- Assess container image security and Docker configuration.
- Document findings with remediation recommendations.
- Produce a professional security assessment report suitable for a real-world AppSec engagement.

---

## Assessment Scope

The assessment focused on evaluating the security of the OWASP Juice Shop application across multiple security domains. The following areas were included in the assessment:

- Threat Modeling
- Static Application Security Testing (SAST)
- Secure Code Review
- Software Dependency Analysis
- Software Bill of Materials (SBOM) Generation
- Container Image Security Assessment
- Dockerfile Security Review
- Runtime Container Configuration Review

The assessment was performed on the application's source code and official Docker image without modifying the application's functionality.

---

## Assessment Methodology

The security assessment followed a structured methodology consisting of four phases:

### 1. Threat Modeling

A threat model was developed to understand the application's architecture, identify critical assets, analyze trust boundaries, and evaluate potential threats using the STRIDE methodology.

### 2. Static Application Security Assessment

The source code was reviewed using Semgrep to identify common security vulnerabilities, followed by manual validation of the findings. Each confirmed vulnerability was documented with its impact, affected code, and remediation recommendations.

### 3. Supply Chain Security Assessment

Software dependencies were analyzed using **npm audit** to identify vulnerable packages. A Software Bill of Materials (SBOM) was generated using **Syft**, and the container image was scanned for known vulnerabilities using **Grype**.

### 4. Container Security Assessment

The application's Dockerfile and container image configuration were reviewed to evaluate container security best practices, including base image selection, privilege management, image metadata, runtime configuration, and overall container hardening.

---

## Tools Used

| Tool | Purpose |
|------|---------|
| **Semgrep** | Static Application Security Testing (SAST) and secure code analysis |
| **Docker** | Build, run, inspect, and analyze application containers |
| **Syft** | Generate Software Bill of Materials (SBOM) for the container image |
| **Grype** | Scan the SBOM for known vulnerabilities (CVEs) |
| **npm audit** | Identify known vulnerabilities in Node.js dependencies |
| **Git** | Version control and project management |
| **GitHub** | Repository hosting and documentation |
| **Markdown** | Security assessment documentation and reporting |

---

## Assessment Workflow

The assessment was performed using the following workflow:

```text
Threat Modeling
        │
        ▼
Static Application Security Testing (Semgrep)
        │
        ▼
Manual Code Review & Finding Validation
        │
        ▼
Software Dependency Analysis (npm audit)
        │
        ▼
SBOM Generation (Syft)
        │
        ▼
Container Vulnerability Scanning (Grype)
        │
        ▼
Container Security Review
        │
        ▼
Documentation & Remediation
```

Each phase built upon the previous one to provide a comprehensive evaluation of the application's security posture. Findings were validated, documented, and accompanied by remediation recommendations based on security best practices.

---

## Repository Structure 

```text
juice-shop-appsec-assessment/
│
├── threat-model/          # STRIDE analysis and threat modeling
├── findings/              # Vulnerability reports and risk assessment
├── screenshots/           # Evidence supporting each finding
├── supply-chain/          # Dependency, SBOM, and image security assessment
├── container-security/    # Dockerfile and container security review
├── README.md              # Project documentation
└── LICENSE                # MIT License
```

Each directory represents a specific phase of the assessment and contains supporting documentation, analysis, and remediation recommendations.

---

## Key Findings

The assessment identified security issues across the application source code, software dependencies, and container image.

### Application Security

- Identified **8 security findings** through static application security testing and manual code review.
- Confirmed vulnerabilities included:
  - SQL Injection
  - Hardcoded RSA Private Key
  - XML External Entity (XXE)
  - Eval Injection
  - Open Redirect
  - Directory Listing
  - Hardcoded HMAC Secret Key
  - Path Traversal

### Supply Chain Security

- Reviewed application dependencies using **npm audit**.
- Generated a Software Bill of Materials (SBOM) using **Syft**.
- Scanned the container SBOM using **Grype**.
- Identified vulnerabilities in both application dependencies and operating system packages.

### Container Security

The Docker image follows several container security best practices, including:

- Multi-stage Docker build
- Distroless runtime image
- Non-root container execution
- SBOM generation during the build process
- Minimal runtime environment
- Proper image metadata and runtime configuration

The assessment did not identify any major container configuration weaknesses.

---

## Skills Demonstrated

This project demonstrates practical application security skills across multiple domains, including:

### Application Security

- Threat Modeling (STRIDE)
- Static Application Security Testing (SAST)
- Secure Code Review
- Vulnerability Validation
- Risk Assessment
- Security Documentation

### Supply Chain Security

- Dependency Analysis
- Software Bill of Materials (SBOM) Generation
- Dependency Vulnerability Assessment
- Container Image Vulnerability Scanning

### Container Security

- Dockerfile Security Review
- Container Image Analysis
- Runtime Configuration Review
- Container Hardening Assessment

### Security Tools

- Semgrep
- npm audit
- Syft
- Grype
- Docker
- Git
- GitHub

### Reporting & Documentation

- Technical Security Reporting
- Vulnerability Documentation
- Remediation Recommendations
- Security Assessment Reporting

---

## Future Improvements

The assessment can be further enhanced by incorporating additional security testing and automation, including:

- Dynamic Application Security Testing (DAST)
- Interactive Application Security Testing (IAST)
- Automated Software Composition Analysis (SCA) in CI/CD pipelines
- Secret scanning
- Container image vulnerability scanning in CI/CD
- Continuous dependency monitoring
- Automated security gates before deployment

These enhancements would help integrate security into the software development lifecycle and support continuous security validation.

---

## Disclaimer

This project was conducted for educational and portfolio purposes using the **OWASP Juice Shop** application, which is intentionally designed to contain security vulnerabilities for training and learning.

The findings documented in this repository are based on the assessment of the official OWASP Juice Shop source code and container image. The identified vulnerabilities should not be interpreted as security issues in production software.

This repository is intended to demonstrate practical Application Security (AppSec) skills, including threat modeling, secure code review, vulnerability assessment, software supply chain analysis, and container security evaluation.