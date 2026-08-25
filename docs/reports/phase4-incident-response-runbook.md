> Incident Response Runbook
>
> Phase 4 --- Security Monitoring & Incident Response

**Purpose**

This runbook defines the process for detecting, containing,
investigating, and recovering from a brute-force login attack against
the Nextcloud server deployed in the cloud security lab environment.

**Detection**

Brute-force login attempts are detected through Nginx and Nextcloud log
monitoring. Failed authentication attempts generate log entries within
the server logs. Fail2ban continuously monitors these logs using the
configured Nextcloud and Nginx jails. When multiple failed login
attempts are detected from the same IP address within a short period,
Fail2ban automatically triggers an alert and blocks the offending IP
address.

Relevant log sources include:

-   /var/log/nginx/access.log

-   /var/log/nginx/error.log

-   /var/log/nextcloud/nextcloud.log

Relevant Fail2ban jails:

-   nginx-http-auth

-   nextcloud

**Containment**

Once the attack is detected, Fail2ban automatically blocks the attacking
IP address after the configured retry threshold is exceeded. The IP
address remains banned for the configured ban duration.

To verify active bans:

sudo fail2ban-client status nextcloud

To manually ban an IP address:

sudo fail2ban-client set nextcloud banip \<IP\>

To manually remove a ban:

sudo fail2ban-client set nextcloud unbanip \<IP\>

**Investigation**

The administrator should review Nginx and Nextcloud logs to determine
the source, frequency, and scope of the attack. GoAccess can be used to
analyze web traffic patterns and identify suspicious activity, including
repeated authentication failures, unusual IP addresses, and spikes in
HTTP error responses such as 401 Unauthorized and 403 Forbidden.

The following information should be collected:

-   Source IP address

-   Number of failed login attempts

-   Time and duration of attack

-   Targeted user accounts

-   Any successful authentication attempts

**Recovery**

After containment, the administrator should verify that no unauthorized
access was achieved before the IP address was blocked. User accounts
targeted during the attack should be reviewed for suspicious activity.
Passwords should be reset if compromise is suspected. Services should be
tested to ensure normal operation and legitimate users should confirm
successful access.

**Lessons Learned**

To further reduce the risk of brute-force attacks, Multi-Factor
Authentication should be enforced for all users, stronger password
policies should be maintained, and security monitoring should be
continuously reviewed. Regular updates to Fail2ban rules and periodic
log analysis will improve the effectiveness of future attack detection
and response activities.

**Screenshots of phase performance:**

Step 1: Install and implement fail2ban

![](../screenshots/phase4/01-fail2ban-install-a.png){width="6.5in" height="1.3131944444444446in"}

![](../screenshots/phase4/01-fail2ban-install-b.png){width="6.5in" height="0.3423611111111111in"}

![](../screenshots/phase4/01-fail2ban-install-c.png){width="6.5in" height="2.3048611111111112in"}

![](../screenshots/phase4/01-fail2ban-install-d.png){width="6.5in" height="0.9659722222222222in"}

Step 2: Brute Force Attack

![](../screenshots/phase4/02-brute-force-attack-a.png){width="6.5in" height="1.4208333333333334in"}

![](../screenshots/phase4/02-brute-force-attack-b.png){width="6.052083333333333in"
height="1.5611111111111111in"}
