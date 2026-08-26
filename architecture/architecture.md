# 🏗️ System Architecture

## Overview

The lab is a self-hosted cloud platform running on a single Ubuntu Server VM (or cloud compute instance), with every service containerized via Docker Compose. Nginx sits at the network edge as a TLS-terminating reverse proxy, Keycloak provides centralized identity for the whole stack, and Nextcloud is the user-facing application backed by PostgreSQL, MinIO, and HashiCorp Vault. A dedicated monitoring and incident-response layer (Fail2ban + GoAccess) watches the stack's logs in real time, and Trivy continuously scans every container image for known vulnerabilities.

![Architecture Diagram](architecture-diagram.png)

## Components

| Component | Role | Notes |
|---|---|---|
| **Nginx** | Reverse proxy / TLS termination | Single entry point for all inbound traffic (port 443); enforces HTTPS, HSTS, security headers, and rate limiting |
| **Keycloak** | Identity Provider (IdP) | `cloud-lab` realm; issues OIDC tokens; enforces MFA (TOTP) for Admin role; owns RBAC role definitions |
| **Nextcloud** | Cloud storage / application layer | Federates auth to Keycloak via OIDC SSO; maps Keycloak roles to Nextcloud groups; encrypts files at rest (Server-Side Encryption) |
| **PostgreSQL** | Database | Backing store for Nextcloud and Keycloak; connections encrypted in transit (TLS) |
| **MinIO** | Object storage | S3-compatible backend for Nextcloud file/object storage and backups |
| **HashiCorp Vault** | Secrets & key management | Centralized, versioned store for credentials, tokens, and encryption keys; nothing is hardcoded in compose files or app configs |
| **Fail2ban** | Intrusion prevention | Watches Nginx/Nextcloud auth logs, detects brute-force patterns, automatically bans offending IPs |
| **GoAccess** | Log analysis | Real-time visual traffic/log analysis for investigation and attack visibility |
| **Trivy** | Vulnerability scanning | Scans all container images for known CVEs as part of ongoing vulnerability management |
| **Docker** | Containerization | Every service above runs as a container on an internal Docker network |
| **Ubuntu Server** | Host OS | Underlying host for the Docker engine |

## User Roles & Access Control

| Role | Access Level |
|---|---|
| **Administrators** | Full access; MFA (TOTP) enforced at login |
| **Editors** | Upload / modify permissions |
| **Viewers** | Read-only access |

Access control is enforced through three mechanisms working together:

- **SSO (OIDC):** Keycloak is the single source of truth for authentication — no service performs its own password checks.
- **MFA (TOTP):** Required for the Admin role before a session is granted.
- **RBAC:** Keycloak roles (Admin / Editor / Viewer) are mapped to Nextcloud groups, which determine what each user can see and do.

## Request & Auth Flow

1. A user's browser hits **Nginx** over HTTPS (TLS termination happens here — see [Phase 3](../docs/reports/phase3-encryption.md)).
2. Nginx forwards the request to **Nextcloud**.
3. If the user isn't authenticated, Nextcloud redirects to **Keycloak** for login via the OpenID Connect Authorization Code flow.
4. Keycloak authenticates the user (and enforces TOTP MFA if they're in the Admin role), then issues an OIDC token back to Nextcloud.
5. Nextcloud maps the user's Keycloak role (Admin / Editor / Viewer) to the matching Nextcloud group, which determines what they can see and do.
6. File operations are persisted through **PostgreSQL** (metadata) and **MinIO** (object data); files at rest are protected by Nextcloud's Server-Side Encryption, and DB connections are encrypted in transit.
7. Any secrets or keys needed by the stack (DB credentials, encryption keys, API tokens) are pulled from **Vault** rather than stored in plaintext config.
8. All access and authentication attempts are logged to Nginx and Nextcloud logs.

## Monitoring & Incident Response Flow

1. **Log sources** — Nginx (`/var/log/nginx/access.log`, `/var/log/nginx/error.log`) and Nextcloud (`/var/log/nextcloud/nextcloud.log`) write continuously.
2. **Fail2ban** tails the auth-relevant logs, detects repeated failed logins / brute-force patterns, and automatically bans the source IP at the firewall level.
3. **GoAccess** parses the same log sources into real-time visual reports, giving human-readable traffic patterns and attack visibility for investigation.
4. **Alerts / notifications** — optional email alerts, plus manual review of GoAccess reports and Fail2ban ban lists, feed the incident response process (detection → containment → investigation → recovery).
5. Findings loop back to the infrastructure layer (e.g., tightening Nginx rate limits, rotating a credential in Vault, or updating a Keycloak policy).

See [security-monitoring/fail2ban](../security-monitoring/fail2ban) for configuration details.

## Network & Trust Boundaries

- **Edge:** Nginx is the only service exposed to the host network; all other containers communicate over the internal Docker network.
- **Identity boundary:** Keycloak is the sole source of truth for authentication — no service performs its own password checks.
- **Data boundary:** Vault is the sole source of truth for secrets — no credentials are hardcoded in compose files or app configs.
- **Monitoring boundary:** Fail2ban and GoAccess only consume logs (read-only) — they do not sit inline in the request path.

## Security Controls Implemented

| Control Area | Implementation |
|---|---|
| **IAM & Access Control** | SSO (OIDC), MFA (TOTP), RBAC via Keycloak → Nextcloud group mapping |
| **Encryption** | In transit via TLS (Nginx termination, DB connections); at rest via Nextcloud Server-Side Encryption |
| **Secrets Management** | HashiCorp Vault — centralized, versioned, no plaintext credentials in configs |
| **Monitoring & Logging** | Fail2ban + GoAccess against Nginx/Nextcloud logs |
| **Incident Response** | Detection, containment, investigation, and recovery workflow driven by log analysis and alerts |
| **Vulnerability Management** | Trivy scans of all container images; periodic CVE review |
| **Compliance** | Aligned to CIS Benchmarks and GDPR data-handling principles |

## Related Documentation

- [Phase 1 — Infrastructure Setup](../docs/reports/phase1-infrastructure-setup.md)
- [Phase 2 — IAM Policy](../docs/reports/phase2-iam-policy.md)
- [Phase 3 — Encryption](../docs/reports/phase3-encryption.md)
- [Security Monitoring](../security-monitoring)
