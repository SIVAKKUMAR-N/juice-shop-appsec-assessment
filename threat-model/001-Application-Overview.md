# OWASP Juice Shop – Application Overview

## Purpose

This document provides a high-level overview of the OWASP Juice Shop application to establish the context for threat modeling. It describes the application's architecture, technologies, trust boundaries, and the scope of the threat model.

---

# Application Overview

OWASP Juice Shop is an intentionally vulnerable open-source web application developed by the OWASP Foundation for security education, secure coding, and penetration testing. The application simulates a modern e-commerce platform where users can browse products, register accounts, place orders, and perform various shopping-related activities.

For this threat model, Juice Shop is analyzed as a typical web application to identify security threats affecting its architecture and data flows.

---

# Architecture Overview

Juice Shop follows a client-server architecture consisting of:

- Angular Frontend
- Express.js Backend
- SQLite Database
- Server File System

The frontend communicates with the backend through REST APIs, while the backend accesses the SQLite database and server file system to process application requests.

---

# Technology Stack

| Component | Technology |
|-----------|------------|
| Frontend | Angular |
| Backend | Node.js + Express.js |
| Database | SQLite |
| Language | TypeScript |
| Runtime | Node.js |

---

# Threat Model Scope

The threat model covers the following components:

- User
- Angular Frontend
- Express Backend
- SQLite Database
- Server File System
- Data Flows between components
- Trust Boundaries

The analysis focuses on identifying threats using the STRIDE methodology.

---

# Assumptions

- Users access the application over HTTP/HTTPS.
- The frontend and backend are hosted within the same application deployment.
- The backend communicates with the SQLite database and the server file system.
- External infrastructure such as operating systems, cloud platforms, and network devices are outside the scope of this threat model.

---

# Threat Modeling Methodology

The threat model is developed using:

- Data Flow Diagrams (DFD)
  - Level 0 (Context Diagram)
  - Level 1 (Application Architecture)
- Trust Boundary Identification
- Asset Identification
- STRIDE Threat Analysis