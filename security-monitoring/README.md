# Vulnerability Scanning — Trivy

Every container image in the stack was scanned with **Trivy** to identify known CVEs, focused on Critical and High severity findings.

## Scan Results

| Container | Critical CVEs | High CVEs | Status | Action Taken |
|---|---|---|---|---|
| **Nextcloud** | 1 | 6 | Reviewed | Updated to latest stable image where possible |
| **Keycloak** | 0 | 4 | Reviewed | Configuration reviewed, risk accepted |
| **MinIO** | 1 | 5 | Reviewed | Latest image deployed |
| **Vault** | 0 | 3 | Reviewed | No critical issues identified |
| **Nginx** | 0 | 2 | Reviewed | Updated and monitored |

## Findings Summary

Every image had at least one High-severity CVE at scan time. Where an updated base/application image was available, it was pulled to reduce exposure. Remaining vulnerabilities that couldn't be remediated within the scope of the project (e.g. no upstream fix yet) were documented and accepted as residual risk — justified here specifically because the lab is isolated and has no publicly exposed services.

## Example Scan Command

```bash
trivy image --severity CRITICAL,HIGH <image-name>:<tag>
```

## Related

- Full findings and remediation rationale: [Phase 5 — Security Audit & Compliance Report](../../docs/reports/phase5-security-audit-compliance.md)
- Maps to **CIS Benchmark → Patch Status** (Pass) and **Container Hardening** (Partial) controls in the same report.
