> **IAM Policy Document**
>
> Phase 2 --- Identity & Access Management

# 1. Overview

This document defines the Identity and Access Management (IAM) policy
for the Self-Hosted Cloud Security Lab. All identity services are
provided by Keycloak (cloud-lab realm) integrated with Nextcloud via
OpenID Connect (OIDC) Single Sign-On (SSO). Three distinct roles enforce
least-privilege access across the platform.

# 2. Role Definitions and Permissions

The following table summarises all roles, their assigned users, and
permission boundaries:

  ------------------------------------------------------------------------
  **Role**     **Assigned    **Permissions**        **Restrictions**
               To**                                 
  ------------ ------------- ---------------------- ----------------------
  **admin**    admin-user    Full Nextcloud access: MFA (TOTP) required at
                             manage users,          every login
                             settings, apps, all    
                             files                  

  **editor**   editor-user   Upload, edit, share    Cannot access Admin
                             files; access personal panel or change server
                             and shared folders     settings

  **viewer**   viewer-user   View and download      Cannot upload, edit,
                             shared files only      delete files or access
                                                    Admin panel
  ------------------------------------------------------------------------

## 2.1 Admin Role

-   Has full control over Nextcloud: user management, app installation,
    storage quotas, security settings.

-   Mapped to Nextcloud group: Admin.

-   Subject to mandatory TOTP MFA (see Section 3).

## 2.2 Editor Role

-   Can upload, edit, and share files within personal and shared
    folders.

-   Cannot access the Nextcloud Admin Panel or modify server
    configuration.

-   Mapped to Nextcloud group: Editor.

## 2.3 Viewer Role

-   Read-only access to files shared with the viewer group.

-   Cannot upload, edit, rename, or delete any files.

-   Cannot access the Admin Panel.

-   Mapped to Nextcloud group: Viewer.

# 3. Multi-Factor Authentication (MFA) Policy

TOTP (Time-based One-Time Password) MFA is enforced through Keycloak
Authentication Policies. The following table documents which roles
require MFA and how it is enforced:

  ------------------------------------------------------------------------
  **Role**           **MFA Required?**  **Enforcement Mechanism**
  ------------------ ------------------ ----------------------------------
  **admin**          YES                Keycloak Required Action:
                                        Configure OTP set on admin-user.
                                        Browser flow enforces TOTP at
                                        every login.

  editor             No (optional)      Users may voluntarily enable TOTP
                                        under their Keycloak account
                                        settings.

  viewer             No                 No MFA enforced. Read-only role
                                        presents minimal risk.
  ------------------------------------------------------------------------

**MFA Enforcement Steps in Keycloak:**

-   Authentication \> Flows \> browser flow duplicated to
    \'browser-mfa\'.

-   Browser - Conditional OTP step set to Required within the flow.

-   Required Action \'Configure OTP\' added to admin-user account.

-   On first login, admin-user is prompted to scan a QR code using
    Google Authenticator or similar TOTP app.

# 4. SSO Configuration (Keycloak + Nextcloud OIDC)

Single Sign-On is implemented using the OpenID Connect Authorization
Code flow between Keycloak and Nextcloud.

**Keycloak Client Settings:**

-   Realm: cloud-lab

-   Client ID: nextcloud

-   Client Authentication: Enabled (confidential client)

-   Valid Redirect URI:
    http://nextcloud.local/apps/sociallogin/custom_oidc/keycloak

**Nextcloud Configuration (Social Login app):**

-   Provider: Custom OpenID Connect

-   Authorize URL:
    http://keycloak.local:8080/realms/cloud-lab/protocol/openid-connect/auth

-   Token URL:
    http://keycloak.local:8080/realms/cloud-lab/protocol/openid-connect/token

-   Scope: openid email profile

-   Group mapping: Keycloak roles automatically mapped to Nextcloud
    groups.

# 5. Shared Responsibility Boundary

As per the CY464 Shared Responsibility Model for this self-hosted stack:

-   Keycloak is responsible for: authentication, token issuance, MFA
    enforcement, session management, and role assignment.

-   Nextcloud is responsible for: authorisation enforcement (checking
    group membership), file access control, and Admin Panel access
    restrictions.

-   The student/operator is responsible for: configuring both systems
    correctly, patching containers, rotating client secrets, and
    auditing login activity.

# 6. Screenshots (Deliverable)

# Step 1: Created the cloud realm and a confidential client named nextcloud inside Keycloak.

# ![](../screenshots/phase2/01-keycloak-realm-client-a.png){width="6.5in" height="3.1347222222222224in"}![](../screenshots/phase2/01-keycloak-realm-client-b.png){width="6.5in" height="2.8618055555555557in"}![](../screenshots/phase2/01-keycloak-realm-client-c.png){width="6.5in" height="2.5618055555555554in"} 

# Creating client here

# 

# ![](../screenshots/phase2/02-creating-client-a.png){width="6.5in" height="2.10625in"}

# ![](../screenshots/phase2/02-creating-client-b.png){width="6.5in" height="2.6569444444444446in"}

# 

# ![](../screenshots/phase2/02-creating-client-c.png){width="6.5in" height="3.0631944444444446in"}

# ![](../screenshots/phase2/02-creating-client-d.png){width="6.5in" height="1.91875in"}

# Step 2: Created three roles (admin, editor, viewer) and assigned them to users admin,editor and viewer

# The roles were created and passwords were also set up and MFA was allowed so before login each user have to provide a MFA code

# ![](../screenshots/phase2/03-roles-admin-editor-viewer.png){width="6.5in" height="2.96875in"}

# Step 3:Step 5: Configured OIDC parameters in the Nextcloud Social Login app to enable Single Sign-On (SSO).

# ![](../screenshots/phase2/04-oidc-sso-config-a.png){width="6.5in" height="3.316666666666667in"}

# ![](../screenshots/phase2/04-oidc-sso-config-b.png){width="1.9056047681539807in" height="2.6666666666666665in"}![](../screenshots/phase2/04-oidc-sso-config-c.png){width="5.239583333333333in" height="2.8985640857392827in"}

# ![](../screenshots/phase2/04-oidc-sso-config-d.png){width="6.5in" height="2.967361111111111in"}

# ![](../screenshots/phase2/04-oidc-sso-config-e.png){width="6.5in" height="3.765277777777778in"}

# We mapped the assigned roles here and attach the relevant links here

# ![](../screenshots/phase2/05-role-mapping.png){width="2.4583333333333335in" height="3.7041557305336834in"}

# Step 4: Final login

# ![](../screenshots/phase2/06-final-login-a.png){width="6.5in" height="3.204861111111111in"}![](../screenshots/phase2/06-final-login-b.png){width="6.5in" height="3.2333333333333334in"}

# Provided the editor login here 

# ![](../screenshots/phase2/07-editor-login-a.png){width="6.5in" height="3.4in"}

# ![](../screenshots/phase2/07-editor-login-b.png){width="6.5in" height="3.1902777777777778in"}

# Then viewer

# ![](../screenshots/phase2/08-viewer-login-a.png){width="6.5in" height="5.198611111111111in"}![](../screenshots/phase2/08-viewer-login-b.png){width="6.5in" height="3.323611111111111in"}

# 

# Admin account

# ![](../screenshots/phase2/09-admin-account-a.png){width="6.5in" height="4.066666666666666in"}![](../screenshots/phase2/07-editor-login-a.png){width="6.5in" height="3.4in"}![](../screenshots/phase2/09-admin-account-b.png){width="6.5in" height="3.209722222222222in"}
