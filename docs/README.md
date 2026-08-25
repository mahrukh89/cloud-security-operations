# Screenshot Evidence Index

All evidence screenshots for this project, grouped by phase and named in execution order (`NN-step-description.png`). Each phase's full narrative lives in the matching report under [`docs/reports/`](../reports).

## Phase 1 — Infrastructure Setup ([report](../reports/phase1-infrastructure-setup.md))
| # | Step | Files |
|---|---|---|
| 01 | Update the system | `01-update-system.png` |
| 02 | Install Docker | `02-install-docker.png` |
| 03 | Set up Docker | `03-setup-docker.png` |
| 04 | Change directory & edit code file | `04-cd-and-nano-code-file-a/b.png` |
| 05 | Compile the code | `05-compile-code-a/b/c.png` |
| 06 | Compose and launch stack | `06-compose-and-launch.png` |
| 07 | Keycloak running | `07-keycloak-a/b.png` |
| 08 | Nextcloud running | `08-nextcloud-a/b.png` |
| 09 | MinIO running | `09-minio-a/b.png` |
| 10 | Vault running | `10-vault-a/b.png` |

## Phase 2 — Identity & Access Management ([report](../reports/phase2-iam-policy.md))
| # | Step | Files |
|---|---|---|
| 01 | Keycloak realm & client created | `01-keycloak-realm-client-a/b/c.png` |
| 02 | Client configuration | `02-creating-client-a/b/c/d.png` |
| 03 | Admin / Editor / Viewer roles created | `03-roles-admin-editor-viewer.png` |
| 04 | OIDC SSO configuration (Nextcloud Social Login) | `04-oidc-sso-config-a/b/c/d/e.png` |
| 05 | Role → group mapping | `05-role-mapping.png` |
| 06 | Final Admin login | `06-final-login-a/b.png` |
| 07 | Editor login | `07-editor-login-a/b.png` |
| 08 | Viewer login | `08-viewer-login-a/b.png` |
| 09 | Admin account detail | `09-admin-account-a/b.png` |

## Phase 3 — Data Security & Encryption ([report](../reports/phase3-encryption.md))
| # | Step | Files |
|---|---|---|
| 01 | TLS enabled via Nginx (data in transit) | `01-tls-nginx-a/b/c.png` |
| 02 | Nextcloud server-side encryption enabled | `02-nextcloud-server-side-encryption-a/b/c.png` |
| 03 | Default Encryption Module + file storage test | `03-dem-file-storage-a/b/c.png` |
| 04 | File accessed from Ubuntu host | `04-file-accessed-ubuntu-a/b.png` |
| 05 | HashiCorp Vault deployed for key management | `05-hashicorp-vault-a/b/c/d.png` |
| 06 | Secret rotation / version history | `06-password-rotation-history-a/b.png` |

## Phase 4 — Security Monitoring & Incident Response ([report](../reports/phase4-incident-response-runbook.md))
| # | Step | Files |
|---|---|---|
| 01 | Fail2ban installed & configured | `01-fail2ban-install-a/b/c/d.png` |
| 02 | Simulated brute-force attack & auto-ban | `02-brute-force-attack-a/b.png` |

## Phase 5 — Security Audit & Compliance ([report](../reports/phase5-security-audit-compliance.md))
| # | Step | Files |
|---|---|---|
| 01 | Full infrastructure diagram | `01-infrastructure-diagram.png` |
