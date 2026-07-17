# Asset Identification

## Purpose

This document identifies the critical assets within the OWASP Juice Shop application. These assets are valuable resources that require protection against unauthorized access, modification, disclosure, or disruption.

---

# Asset Inventory

| Asset | Description | Security Objectives |
|--------|-------------|---------------------|
| User Accounts | User identities, profiles, and account information | Confidentiality, Integrity |
| Authentication Credentials | User passwords, password hashes, and JWT tokens | Confidentiality, Integrity |
| Product Catalog | Product names, descriptions, prices, and inventory | Integrity, Availability |
| Customer Orders | Order details and purchase history | Confidentiality, Integrity |
| SQLite Database | Primary data store containing application data | Confidentiality, Integrity, Availability |
| File System | Stores images, videos, documents, logs, and uploaded files | Confidentiality, Integrity, Availability |
| REST API | Backend endpoints that provide application functionality | Integrity, Availability |
| Application Source Code | Business logic and application functionality | Integrity |
| Session Tokens | Authentication and session management tokens | Confidentiality, Integrity |

---

# Asset Classification

## High Value Assets

These assets have the greatest impact if compromised.

- Authentication Credentials
- SQLite Database
- User Accounts
- Session Tokens

---

## Medium Value Assets

- Customer Orders
- Product Catalog
- File System

---

## Low Value Assets

- Static Images
- Public Videos
- Product Information

---

# Security Objectives

The following security objectives are applied to each asset.

| Objective | Description |
|-----------|-------------|
| Confidentiality | Prevent unauthorized disclosure of information. |
| Integrity | Prevent unauthorized modification of information. |
| Availability | Ensure assets remain accessible to authorized users. |

---

# Critical Assets for Threat Modeling

The following assets will be analyzed during the STRIDE assessment.

- Angular Frontend
- Express Backend
- SQLite Database
- File System
- REST API
- User Authentication