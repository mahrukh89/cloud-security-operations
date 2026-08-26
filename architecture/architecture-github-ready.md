# Cloud Security Architecture

## Overview

This project implements a self-hosted cloud security environment running on an Ubuntu Server virtual machine. The environment uses Docker containers to deploy Nextcloud, PostgreSQL, Keycloak, MinIO, HashiCorp Vault, and Nginx.

The architecture combines **identity and access management, encryption, secrets management, security monitoring, incident response, vulnerability assessment, and compliance controls** into one security-focused cloud environment.

![Cloud Security Architecture](architecture-diagram.png)

---

## Architecture Layers

### 1. User & Access Layer

Three user roles are defined within the environment:

- **Administrator** — full administrative access
- **Editor** — upload, edit, and share files
- **Viewer** — read-only access

Keycloak provides centralized identity management, while Nextcloud uses role/group mapping to enforce authorization. Administrative accounts are protected with TOTP-based MFA.

---

### 2. Edge / Reverse Proxy Layer

**Nginx** operates as the reverse proxy at the front of the environment.

Its security responsibilities include:

- TLS/HTTPS termination
- Secure routing to the internal services
- Nginx access and error logging
- Acting as the primary entry point for web traffic

TLS is used to protect data in transit between users and the cloud services.

---

### 3. Application & Service Layer

The main services are deployed as containerized components:

| Component | Security / Operational Role |
|---|---|
| **Nextcloud** | User-facing cloud storage and file management |
| **Keycloak** | Centralized IAM, OIDC SSO, RBAC and MFA |
| **PostgreSQL** | Database backend |
| **MinIO** | S3-compatible object storage |
| **HashiCorp Vault** | Centralized secrets and key management |
| **Nginx** | Reverse proxy and TLS termination |

---

### 4. Data Protection Layer

The architecture protects data through multiple controls:

- **TLS** protects data in transit.
- **Nextcloud Server-Side Encryption** protects stored files at rest.
- **HashiCorp Vault** provides centralized secrets and key management.
- Vault versioning supports controlled secret updates and recovery.

These controls provide protection across different stages of the data lifecycle.

---

### 5. Security Monitoring & Incident Response

The environment includes dedicated defensive security controls.

**Fail2ban** monitors Nginx and Nextcloud authentication logs. Repeated failed login attempts can trigger automatic IP blocking.

Relevant log sources include:

- `/var/log/nginx/access.log`
- `/var/log/nginx/error.log`
- `/var/log/nextcloud/nextcloud.log`

**GoAccess** can be used to analyze web traffic and identify suspicious authentication activity, unusual IP addresses, and HTTP error spikes.

The incident-response workflow follows:

**Detection → Containment → Investigation → Recovery → Lessons Learned**

---

### 6. Vulnerability Management

**Trivy** is used to scan deployed container images for known vulnerabilities.

The security audit documents Critical and High severity CVEs across components and records remediation actions or accepted residual risk.

This adds a vulnerability-management layer to the operational security architecture.

---

## Authentication & Request Flow

The normal authentication flow is:

1. A user accesses the cloud service through the Nginx reverse proxy using HTTPS.
2. Nginx routes the request to the appropriate application service.
3. Nextcloud uses Keycloak for centralized authentication through **OpenID Connect (OIDC)**.
4. Keycloak authenticates the user and enforces TOTP MFA for the administrator account.
5. Keycloak roles are mapped to corresponding Nextcloud groups.
6. Nextcloud applies the resulting permissions to determine what the user can access.
7. Application and object-storage data are handled through PostgreSQL and MinIO.
8. Encryption protects stored data, while Vault manages sensitive secrets and keys.

---

## Security Monitoring Flow

The incident-response architecture can be summarized as:

```text
Authentication Attempts
        │
        ▼
Nginx / Nextcloud Logs
        │
        ├──────────────► GoAccess
        │                 │
        │                 ▼
        │            Traffic Analysis
        │
        ▼
     Fail2ban
        │
        ▼
Repeated Failed Logins
        │
        ▼
Automatic IP Ban
        │
        ▼
Investigation & Recovery
```

This workflow demonstrates practical defensive security operations rather than only infrastructure deployment.

---

## Security Controls Implemented

| Security Area | Controls |
|---|---|
| **IAM** | Keycloak, RBAC, role-based permissions |
| **Authentication** | OIDC SSO, TOTP MFA |
| **Encryption** | TLS, Nextcloud Server-Side Encryption |
| **Secrets Management** | HashiCorp Vault |
| **Logging** | Nginx and Nextcloud logs |
| **Intrusion Prevention** | Fail2ban |
| **Log Analysis** | GoAccess |
| **Vulnerability Management** | Trivy, CVE assessment |
| **Compliance** | CIS Benchmark gap analysis, GDPR mapping |
| **Incident Response** | Detection, containment, investigation, recovery |

---

## Trust & Security Boundaries

### Identity Boundary

Keycloak acts as the centralized identity provider for authentication, token issuance, MFA enforcement, session management, and role assignment.

### Authorization Boundary

Nextcloud applies authorization based on the mapped user groups and roles, controlling access to files and administrative functions.

### Data Protection Boundary

Sensitive data is protected through TLS, server-side encryption, and centralized secrets/key management through Vault.

### Monitoring Boundary

Nginx and Nextcloud logs provide security telemetry for Fail2ban and GoAccess. These controls support detection and investigation of suspicious authentication activity.

---

## Related Documentation

- [Phase 1 — Infrastructure Setup](../docs/reports/phase1-infrastructure-setup.md)
- [Phase 2 — IAM Policy](../docs/reports/phase2-iam-policy.md)
- [Phase 3 — Encryption](../docs/reports/phase3-encryption.md)
- [Phase 4 — Incident Response Runbook](../docs/reports/phase4-incident-response-runbook.md)
- [Phase 5 — Security Audit & Compliance](../docs/reports/phase5-security-audit-compliance.md)
- [Security Monitoring](../security-monitoring)

---

## Portfolio Focus

This architecture demonstrates practical experience across:

**Cloud Security → IAM → MFA/SSO → Encryption → Secrets Management → Logging → Detection → Incident Response → Vulnerability Management → Compliance**
