# Logs — Sources & Analysis

The lab centers monitoring on two application layers: the reverse proxy (Nginx) and the application itself (Nextcloud). No log file exports are checked into this repo (they're runtime artifacts, excluded via `.gitignore`) — this document records **what** is logged and **how** it's analyzed, so the setup is reproducible.

## Log Sources

| Log | Path | Purpose |
|---|---|---|
| Nginx access log | `/var/log/nginx/access.log` | All HTTP requests — source IP, status code, path, timing |
| Nginx error log | `/var/log/nginx/error.log` | Proxy/TLS-level failures |
| Nextcloud log | `/var/log/nextcloud/nextcloud.log` | Application-level events — auth attempts, file operations, admin actions |

## Analysis Tooling

**GoAccess** is used to analyze web traffic patterns in real time (and generate historical HTML reports), specifically to surface:

- Repeated authentication failures
- Unusual or unrecognized source IPs
- Spikes in `401 Unauthorized` / `403 Forbidden` responses
- Overall traffic and error-rate trends

## What Investigation Looks Like

When Fail2ban (see [`../fail2ban`](../fail2ban)) flags or bans an IP, the following is pulled from these logs to scope an incident:

- Source IP address
- Number of failed login attempts
- Time and duration of the attack window
- Targeted user account(s)
- Whether any attempt succeeded before the ban took effect

This maps directly to the **Investigation** phase of the [Incident Response Runbook](../../docs/reports/phase4-incident-response-runbook.md).

## GDPR Relevance

Nginx and Nextcloud logging directly satisfies **GDPR Article 30 (Records of Processing Activities)** and feeds **Article 33 (Breach Detection and Response)** — see the full mapping in the [Phase 5 audit report](../../docs/reports/phase5-security-audit-compliance.md).
