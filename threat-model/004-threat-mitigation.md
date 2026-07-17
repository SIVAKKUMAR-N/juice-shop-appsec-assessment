# Threat Mitigation

## Purpose

This document describes the security controls implemented or recommended to mitigate the threats identified during the STRIDE analysis.

---

## 1. Authentication & Authorization

### Threats Addressed

- JWT Token Theft
- Credential Stuffing
- Account Hijacking
- Privilege Escalation

### Security Controls

- Multi-Factor Authentication (MFA)
- Short-lived JWTs
- Refresh Token Rotation
- Secure Password Policy
- Role-Based Access Control (RBAC)
- Server-side Authorization Checks
- Session Invalidation after Password Change

---

## 2. Input Validation

### Threats Addressed

- SQL Injection
- Command Injection
- Path Traversal
- Stored XSS
- ReDoS

### Security Controls

- Server-side Input Validation
- Parameterized Queries
- Input Length Validation
- Allowlist Validation
- HTML Sanitization
- Output Encoding

---

## 3. File Security

### Threats Addressed

- Path Traversal
- Directory Listing
- Malicious File Upload
- ZIP Bomb

### Security Controls

- Disable Directory Listing
- Validate File Paths
- Restrict Upload Size
- Restrict File Types
- Store Uploads Outside Web Root
- Scan Uploaded Files

---

## 4. Logging & Monitoring

### Threats Addressed

- Repudiation
- Suspicious Login Attempts
- Administrative Abuse

### Security Controls

- Audit Logging
- Security Event Logging
- Timestamp Logging
- IP Address Logging
- Log Monitoring
- Tamper-resistant Logs

---

## 5. Availability Protection

### Threats Addressed

- Login Flooding
- API Flooding
- ReDoS
- Resource Exhaustion

### Security Controls

- Rate Limiting
- Request Throttling
- WAF
- File Upload Limits
- CPU & Memory Limits
- Request Timeouts

---

## 6. Container Security

### Threats Addressed

- Command Injection
- Privilege Escalation
- Container Escape

### Security Controls

- Run as Non-root User
- Read-only File System (where possible)
- Drop Linux Capabilities
- Minimal Base Images
- Regular Image Scanning (Trivy)

---

## 7. Secret Management

### Threats Addressed

- Hardcoded Secrets
- JWT Secret Exposure

### Security Controls

- Environment Variables
- Secret Rotation
- Secret Managers
- Never Store Secrets in Source Code

---

## 8. Error Handling

### Threats Addressed

- Stack Trace Disclosure

### Security Controls

- Generic Error Messages
- Centralized Exception Handling
- Disable Debug Mode in Production
- Log Errors Securely