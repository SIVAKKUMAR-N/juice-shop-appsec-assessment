# Container Security Assessment

## Purpose

This assessment evaluates the security posture of the OWASP Juice Shop container image. The objective is to identify security controls implemented within the container and assess whether the image follows container security best practices before deployment.

---

# Assessment Scope

The assessment covers:

- Container image architecture
- Base image selection
- Dockerfile security practices
- Runtime configuration
- Container hardening
- Image metadata
- Runtime verification

---

# Assessment Methodology

The following techniques were used:

- Manual Dockerfile Review
- Docker Image Inspection (`docker inspect`)
- Runtime Container Inspection (`docker inspect <container-id>`)

---

# Assessment Objectives

The assessment verifies whether the container:

- Runs as a non-root user
- Uses a minimal base image
- Exposes only required ports
- Generates a Software Bill of Materials (SBOM)
- Avoids unnecessary packages and build artifacts
- Implements container hardening practices
- Uses secure runtime configuration

---

# Assessment Summary

The OWASP Juice Shop container follows several container security best practices. The Dockerfile uses a multi-stage build process, generates an SBOM during the build, executes the application using a dedicated non-root user, and deploys the application on a minimal Distroless base image. Runtime inspection also confirms that the container is not running in privileged mode and exposes only the required application port.

However, some additional hardening opportunities remain, including enabling a read-only root filesystem and defining resource limits during deployment.

---

# Assessment Artifacts

The following evidence was collected during the assessment:

- Dockerfile review
- Docker image configuration
- Runtime container configuration
- Docker inspect output

These artifacts are documented in the following files:

- 02-dockerfile-review.md
- 03-image-configuration.md
- 04-runtime-security.md
- remediation.md