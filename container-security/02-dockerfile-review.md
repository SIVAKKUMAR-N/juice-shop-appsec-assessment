# Dockerfile Security Review

## Purpose

This document reviews the OWASP Juice Shop Dockerfile to assess whether it follows secure container image construction practices. The review focuses on build security, image hardening, least privilege, and supply chain security.

---

# Dockerfile Overview

The Dockerfile uses a multi-stage build process to separate the application build environment from the production runtime environment.

The build stage is responsible for installing dependencies, compiling the application, and generating a Software Bill of Materials (SBOM). The runtime stage copies only the required application artifacts into a lightweight Distroless image for deployment.

This design minimizes the attack surface of the final container image while improving software supply chain visibility.

---

# Security Controls Review

## 1. Multi-stage Build

```dockerfile
FROM node:24 AS installer
...
FROM gcr.io/distroless/nodejs24-debian13
```

### Observation

The Dockerfile separates the build environment from the production runtime environment.

### Security Benefit

- Removes build tools from the final image.
- Reduces image size.
- Minimizes the attack surface.

### Assessment

The multi-stage build process is implemented correctly and ensures that only the necessary application artifacts are included in the production image.

---

## 2. Minimal Runtime Image

```dockerfile
FROM gcr.io/distroless/nodejs24-debian13
```

### Observation

The production container uses Google's Distroless Node.js image instead of a standard Linux distribution.

### Security Benefit

- Eliminates unnecessary operating system utilities.
- Removes package managers and interactive shells.
- Reduces opportunities for post-exploitation activities.

### Assessment

Using a Distroless image strengthens the runtime security posture by minimizing the number of available components that could be targeted by an attacker.

---

## 3. Production Dependency Installation

```dockerfile
RUN npm install --omit=dev --unsafe-perm
RUN npm dedupe --omit=dev
```

### Observation

Only production dependencies are installed, and duplicate packages are removed.

### Security Benefit

- Excludes development libraries from production.
- Reduces the dependency footprint.
- Lowers the likelihood of vulnerable packages being deployed.

### Assessment

The dependency installation process follows good deployment practices by limiting the runtime environment to only the packages required by the application.

---

## 4. Removal of Unnecessary Files

```dockerfile
RUN rm -rf frontend/node_modules
RUN rm -rf frontend/.angular
RUN rm -rf frontend/src/assets
```

### Observation

Development artifacts and unnecessary directories are removed before the runtime image is created.

### Security Benefit

- Reduces image size.
- Removes unnecessary files.
- Limits information disclosure.

### Assessment

Removing development artifacts contributes to a cleaner production image and reduces the available attack surface.

---

## 5. Software Bill of Materials (SBOM)

```dockerfile
RUN npm install -g @cyclonedx/cyclonedx-npm
RUN npm run sbom
```

### Observation

The Dockerfile automatically generates a Software Bill of Materials during the build process.

### Security Benefit

- Improves visibility into application dependencies.
- Supports vulnerability management.
- Enhances software supply chain transparency.

### Assessment

Generating an SBOM as part of the build process demonstrates good software supply chain security practices and simplifies future dependency analysis.

---

## 6. Non-root User Execution

```dockerfile
USER 65532
```

### Observation

The application runs using a dedicated non-root user.

### Security Benefit

- Prevents unnecessary root privileges.
- Limits the impact of a successful compromise.
- Supports the Principle of Least Privilege.

### Assessment

Executing the application as a non-root user significantly improves runtime security by restricting container privileges.

---

## 7. Working Directory

```dockerfile
WORKDIR /juice-shop
```

### Observation

A dedicated working directory is configured for the application.

### Security Benefit

- Organizes application files.
- Prevents accidental execution from unexpected locations.

### Assessment

Using a dedicated working directory improves maintainability and supports predictable container execution.

---

## 8. Network Exposure

```dockerfile
EXPOSE 3000
```

### Observation

Only the application service port is exposed.

### Security Benefit

- Reduces unnecessary network exposure.
- Limits externally accessible services.

### Assessment

The Dockerfile exposes only the required application port, following the principle of minimizing exposed services.

---

## 9. Application Startup

```dockerfile
CMD ["/juice-shop/build/app.js"]
```

### Observation

The container directly launches the compiled Node.js application.

### Security Benefit

- Provides predictable startup behavior.
- Avoids unnecessary startup scripts.
- Simplifies container execution.

### Assessment

The application starts directly using the compiled entry point, resulting in a straightforward and maintainable runtime configuration.

---

# Overall Assessment

The Dockerfile incorporates several container security best practices, including a multi-stage build process, a minimal Distroless runtime image, production-only dependency installation, automatic SBOM generation, execution as a non-root user, and limited network exposure.

These controls collectively reduce the container's attack surface and improve both runtime security and software supply chain visibility.

Some runtime hardening measures, such as enabling a read-only root filesystem or defining resource limits, are deployment configurations rather than Dockerfile configurations and should be applied when the container is deployed.

---

# Conclusion

The OWASP Juice Shop Dockerfile demonstrates a strong security baseline for containerized applications. The implemented controls align with modern container security practices and provide a solid foundation for secure deployment. Additional runtime hardening can further improve the overall security posture without requiring changes to the Dockerfile itself.