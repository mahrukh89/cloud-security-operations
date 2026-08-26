# 🛰️ Security Monitoring

This directory covers the detection and governance side of the lab: intrusion prevention, log-based analysis, and container vulnerability scanning.

| Folder | What it covers |
|---|---|
| [`fail2ban/`](fail2ban) | Brute-force detection & automated IP banning, including a live simulated attack |
| [`logs/`](logs) | Log sources monitored and how they're analyzed |
| [`vulnerability-scanning/`](vulnerability-scanning) | Trivy container image scan results |

For the full incident response process (Detection → Containment → Investigation → Recovery → Lessons Learned), see the [Phase 4 Incident Response Runbook](../docs/reports/phase4-incident-response-runbook.md).
