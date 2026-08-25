> Data Security & Encryption
>
> Phase 5 --- Security Audit & Compliance Report

1.  **Executive Summary**

This project involved the design and deployment of a self-hosted cloud
security lab on an Ubuntu Server virtual machine using Docker
containers. The environment consisted of Nextcloud for cloud storage,
PostgreSQL as the database backend, Keycloak for Identity and Access
Management (IAM), MinIO for object storage, HashiCorp Vault for secrets
and key management, and Nginx as a reverse proxy with TLS encryption.
Security controls were implemented throughout the environment, including
Single Sign-On (SSO), Role-Based Access Control (RBAC), Multi-Factor
Authentication (MFA), encryption for data in transit and at rest,
centralized secrets management, security monitoring through Fail2ban and
GoAccess, and vulnerability assessment using Trivy. The overall security
posture of the cloud lab was assessed against CIS Benchmark controls and
GDPR requirements, demonstrating a strong level of protection through
secure authentication, encryption, logging, intrusion prevention, and
compliance-focused security practices, while identifying a small number
of areas for future improvement such as advanced container hardening and
automated backup management.

2.  **Infrastructure Diagram:**

![](../screenshots/phase5/01-infrastructure-diagram.png){width="6.5in" height="3.6777777777777776in"}

3.  **IAM Summary**

Identity and Access Management (IAM) within the cloud security lab was
implemented using Keycloak as the centralized identity provider. Three
user roles were created to enforce Role-Based Access Control (RBAC):
Admin, Editor, and Viewer. The Admin role possesses full administrative
privileges, including user management, system configuration, and
security policy administration. The Editor role is permitted to upload,
modify, and manage files within Nextcloud but is restricted from
performing administrative functions. The Viewer role has read-only
access to resources and cannot modify data or access management
features.

Single Sign-On (SSO) was implemented by integrating Nextcloud with
Keycloak using the OpenID Connect (OIDC) protocol. This integration
allows users to authenticate once through Keycloak and securely access
cloud services without maintaining separate credentials. Multi-Factor
Authentication (MFA) was enforced for administrative accounts using
Time-Based One-Time Passwords (TOTP), providing an additional layer of
security beyond traditional passwords. The IAM implementation follows
the principle of least privilege by ensuring users receive only the
permissions required for their assigned responsibilities.

4.  **Encryption Summary**

Multiple encryption mechanisms were implemented to protect data
throughout its lifecycle. Data in transit is secured through Transport
Layer Security (TLS), configured using Nginx as a reverse proxy. All
communication between users and cloud services occurs over HTTPS,
protecting sensitive information from interception and unauthorized
access.

Data at rest is protected through Nextcloud Server-Side Encryption,
ensuring that files stored within the cloud environment remain encrypted
on disk. This prevents direct access to stored data without proper
authorization and decryption mechanisms. HashiCorp Vault serves as the
centralized secrets and key management solution for the environment.
Sensitive credentials, application secrets, and encryption-related
information are securely stored within Vault rather than being
maintained in plaintext configuration files.

Key management is further strengthened through Vault\'s versioning and
rotation capabilities. When a secret is updated, Vault automatically
creates a new version while preserving previous versions for audit and
recovery purposes. This approach supports secure cryptographic key
lifecycle management and reduces risks associated with long-term key
exposure.

5.  **Vulnerability Scan Results**

Container images were assessed using Trivy vulnerability scanning to
identify known security weaknesses. The scans focused on detecting
Critical and High severity Common Vulnerabilities and Exposures (CVEs)
present within deployed container images.

  ----------------------------------------------------------------------------------------
  **Container**   **Critical   **High    **Status**     **Action**
                  CVEs**       CVEs**                   
  --------------- ------------ --------- -------------- ----------------------------------
  **Nextcloud**   **1**        **6**     **Reviewed**   **Updated to latest stable image
                                                        where possible**

  **Keycloak**    **0**        **4**     **Reviewed**   **Configuration reviewed and risks
                                                        accepted**

  **MinIO**       **1**        **5**     **Reviewed**   **Latest image deployed**

  **Vault**       **0**        **3**     **Reviewed**   **No critical issues identified**

  **Nginx**       **0**        **2**     **Reviewed**   **Updated and monitored**
  ----------------------------------------------------------------------------------------

The vulnerability assessment identified several security findings across
container images. Wherever possible, updated container versions and
secure configurations were applied to reduce exposure. Remaining
vulnerabilities that could not be mitigated within the scope of the
project were documented and accepted as residual risk due to the
isolated nature of the lab environment and the absence of publicly
exposed services.

6.  **CIS Benchmark Gap Analysis**

The cloud environment was evaluated against selected CIS Benchmark
controls to assess adherence to security best practices.

  -------------------------------------------------------------------------------
  **Control Area** **Status**   **Remarks**
  ---------------- ------------ -------------------------------------------------
  TLS / HTTPS      Pass         HTTPS enforced across all services

  Password Policy  Pass         Strong password requirements configured

  MFA              Pass         TOTP enabled for administrator accounts

  Least Privilege  Pass         RBAC implemented successfully

  Secrets          Pass         Secrets stored securely in Vault
  Management                    

  Logging          Pass         Nginx and Nextcloud logging enabled

  Intrusion        Pass         Fail2ban configured and tested
  Prevention                    

  Container        Partial      Some containers require elevated privileges
  Hardening                     

  Patch Status     Pass         Latest stable container images deployed

  Backup           Partial      VM snapshots available but automated backup not
                                implemented
  -------------------------------------------------------------------------------

The assessment indicates strong compliance with recommended CIS security
controls. Core areas including authentication, encryption, access
control, logging, and intrusion prevention were fully implemented. Minor
gaps remain in automated backup management and advanced container
hardening practices, which could further improve resilience and
operational security in a production environment.

7.  **GDPR Mapping**

The implemented security controls were mapped against key GDPR
requirements to demonstrate compliance with privacy and data protection
principles.

  ------------------------------------------------------------------------
  **GDPR      **Requirement**        **Implemented Control**
  Article**                          
  ----------- ---------------------- -------------------------------------
  Article 5   Integrity and          RBAC, MFA, Encryption
              Confidentiality        

  Article 25  Data Protection by     Secure cloud architecture and IAM
              Design                 controls

  Article 30  Records of Processing  Nginx and Nextcloud logging
              Activities             

  Article 32  Security of Processing TLS, Server-Side Encryption, Vault

  Article 33  Breach Detection and   Fail2ban monitoring and incident
              Response               response procedures
  ------------------------------------------------------------------------

The implemented controls support GDPR objectives by ensuring
confidentiality, integrity, accountability, and protection of user data.
Encryption technologies, centralized identity management, security
monitoring, and audit logging collectively contribute to a secure and
privacy-focused cloud environment.

8.  **Recommendations**

Based on the security assessment, several improvements are recommended
to further strengthen the cloud environment.

1.  Implement automated backup and disaster recovery procedures for
    Nextcloud data, PostgreSQL databases, and Vault secrets to improve
    business continuity and resilience.

2.  Extend Multi-Factor Authentication requirements to all users rather
    than limiting MFA to administrative accounts, reducing the
    likelihood of unauthorized access through compromised credentials.

3.  Deploy a centralized Security Information and Event Management
    (SIEM) platform such as Wazuh or the ELK Stack to provide advanced
    threat detection, log correlation, and continuous security
    monitoring capabilities.

These enhancements would further improve the security posture,
compliance readiness, and operational maturity of the self-hosted cloud
environment while aligning the deployment more closely with enterprise
cloud security best practices.
