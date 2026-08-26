# ☁️ Self-Hosted Cloud Security Lab

**CY464 — Cloud Security | Course Project**

A fully self-hosted, Docker-based cloud platform built end-to-end with enterprise-grade security controls: centralized Identity & Access Management, encryption in transit and at rest, secrets management, intrusion prevention, and a formal security audit mapped against CIS Benchmarks and GDPR.

> Built and documented in 5 phases — from bare VM to an audited, monitored, production-style security stack.

![Infrastructure Diagram](docs/screenshots/phase5/01-infrastructure-diagram.png)

---

## 📌 Project Overview

This lab simulates a small enterprise cloud environment, deployed entirely on a self-managed Ubuntu Server VM using Docker. It was designed to demonstrate practical, hands-on implementation of core cloud security domains rather than just theory:

- **Identity & Access Management** — centralized auth, SSO, RBAC, MFA
- **Data protection** — TLS in transit, server-side encryption at rest, centralized secrets/key management
- **Security monitoring & incident response** — intrusion detection/prevention and a documented runbook
- **Governance** — vulnerability scanning and a formal audit against CIS Benchmark controls and GDPR articles

## 🧱 Tech Stack

| Layer | Technology |
|---|---|
| Host OS | Ubuntu Linux (Server) |
| Containerization | Docker / Docker Compose |
| Cloud Storage | Nextcloud |
| Database | PostgreSQL |
| Identity Provider (IAM/SSO) | Keycloak (OpenID Connect) |
| Object Storage | MinIO |
| Secrets & Key Management | HashiCorp Vault |
| Reverse Proxy / TLS | Nginx |
| Intrusion Prevention | Fail2ban |
| Log Analysis | GoAccess |
| Vulnerability Scanning | Trivy |

## 🏗️ Architecture

Nginx sits in front of the stack as a TLS-terminating reverse proxy. Keycloak acts as the single identity provider for the environment; Nextcloud federates to it via OIDC for Single Sign-On, while role membership in Keycloak (Admin / Editor / Viewer) maps directly onto Nextcloud's authorization groups. MinIO provides S3-compatible object storage, PostgreSQL backs Nextcloud's application data, and HashiCorp Vault centralizes all secrets, credentials, and key material so nothing sensitive lives in plaintext config. Fail2ban watches Nginx/Nextcloud logs in real time and automatically bans brute-force sources.

---

## 📂 Repository Structure

```
.
cloud-security-lab-portfolio/
│
├── README.md
├── SETUP.md
├── LICENSE
├── .gitignore
│
├── architecture/
│   ├── architecture-diagram.png
│   └── architecture.md
│
├── docs/
│   ├── reports/
│   │   ├── phase1-infrastructure-setup.md
│   │   ├── phase2-iam-policy.md
│   │   ├── phase3-encryption.md
│   │   ├── phase4-incident-response-runbook.md
│   │   └── phase5-security-audit-compliance.md
│   │
│   └── screenshots/
│       ├── README.md
│       ├── phase1/
│       ├── phase2/
│       ├── phase3/
│       ├── phase4/
│       └── phase5/
│
├── security-monitoring/
│   ├── fail2ban/
│   ├── logs/
│   └── vulnerability-scanning/
│
└── .gitignore
```

Every screenshot is named `NN-step-description.png` so it sorts in the exact order the step was performed, and every phase report's images link straight into `docs/screenshots/<phase>/`, so the write-up and the evidence stay in sync.

---

## 🚀 Phase 1 — Infrastructure Setup

Stood up the Ubuntu VM, installed Docker, and brought the full stack (Keycloak, Nextcloud, MinIO, Vault) online via Docker Compose, with local DNS mapping configured so the services were reachable from the host browser.

📄 [Full report](docs/reports/phase1-infrastructure-setup.md) · 🖼️ [Screenshots](docs/screenshots/phase1)

## 🔐 Phase 2 — Identity & Access Management (IAM)

Centralized identity on Keycloak (`cloud-lab` realm) with three least-privilege roles — **Admin**, **Editor**, **Viewer** — federated into Nextcloud over OIDC-based SSO. Mandatory TOTP MFA enforced for the Admin role via a custom browser authentication flow.

| Role | Access | MFA |
|---|---|---|
| Admin | Full control — users, settings, apps, all files | ✅ Required (TOTP) |
| Editor | Upload/edit/share own + shared files, no admin panel | Optional |
| Viewer | Read-only on shared files | Not enforced |

📄 [Full report / IAM policy](docs/reports/phase2-iam-policy.md) · 🖼️ [Screenshots](docs/screenshots/phase2)

## 🔒 Phase 3 — Data Security & Encryption

TLS enabled end-to-end via Nginx (data in transit), Nextcloud Server-Side Encryption enabled (data at rest), and HashiCorp Vault deployed for centralized secrets/key management with version history on rotation.

📄 [Full report](docs/reports/phase3-encryption.md) · 🖼️ [Screenshots](docs/screenshots/phase3)

## 🚨 Phase 4 — Security Monitoring & Incident Response

Fail2ban deployed against Nginx/Nextcloud auth logs to detect and auto-block brute-force login attempts, backed by a full incident response runbook covering **Detection → Containment → Investigation → Recovery → Lessons Learned**.

📄 [Full runbook](docs/reports/phase4-incident-response-runbook.md) · 🖼️ [Screenshots](docs/screenshots/phase4)

## 📊 Phase 5 — Security Audit & Compliance Report

Closing audit of the whole environment: Trivy vulnerability scans of every container image, a gap analysis against CIS Benchmark controls, and a mapping of implemented controls to GDPR Articles 5, 25, 30, 32, and 33.

**CIS Benchmark result:** 8/10 controls fully passed (TLS, password policy, MFA, least privilege, secrets management, logging, intrusion prevention, patch status); 2 partial (container hardening, automated backup).

📄 [Full audit report](docs/reports/phase5-security-audit-compliance.md) · 🖼️ [Screenshots](docs/screenshots/phase5)

---

## ✅ Key Security Controls Implemented

- [x] Centralized IAM with Single Sign-On (OIDC)
- [x] Role-Based Access Control (least privilege)
- [x] Multi-Factor Authentication (TOTP)
- [x] TLS encryption for data in transit
- [x] Server-side encryption for data at rest
- [x] Centralized secrets/key management with rotation & versioning
- [x] Automated intrusion prevention (Fail2ban)
- [x] Log-based traffic analysis (GoAccess)
- [x] Container vulnerability scanning (Trivy)
- [x] Documented incident response runbook
- [x] CIS Benchmark & GDPR compliance mapping

## 🔭 Recommendations for Future Work

1. Automated backup & disaster recovery for Nextcloud data, PostgreSQL, and Vault secrets.
2. Extend MFA to all users, not just Admin.
3. Deploy a centralized SIEM (Wazuh / ELK Stack) for advanced threat detection and log correlation.

---

## 👥 Author

Mahrukh 

## 📄 License

This project is licensed under the [MIT License](LICENSE) — feel free to reference it for learning purposes.
