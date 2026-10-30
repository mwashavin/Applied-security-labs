# Applied Security Labs

Hands-on offensive security labs built in an isolated Microsoft 365 / Entra ID trial tenant. Each lab follows the same structure, scenario, build, attack narrative, evidenced impact, detection, and remediation. It is backed by real screenshots from a live, self-owned environment rather than theory alone.

**Author:** Wafula Mwangi
**Environment:** Microsoft 365 E5 trial tenant (isolated lab, no production data)
**Focus areas:** Identity and access security, SaaS misconfiguration, OAuth/application security

---

## Labs

### 01 — [Illicit Consent Grant via an Over-Scoped OAuth App](./01-illicit-consent-grant/)
Simulated a consent-phishing attack using a deliberately over-scoped, unpublished OAuth app. Demonstrated that Entra ID's secure defaults block naive direct-user consent, and that this protection is fully bypassed the moment an administrator approves the same app without scrutinizing its requested scopes, after which every user in the tenant silently inherits persistent mail/files/contacts access. Evidenced end-to-end with a decoded bearer token obtained via the OAuth2 authorization code flow.

**Key finding:** Admin consent is a single point of failure for an entire tenant's worth of users.

### 02 — [Unreviewed Break-Glass Exclusion in Conditional Access](./02-break-glass-conditional-access/)
Built a tenant-wide "Require MFA" Conditional Access policy and examined the standard practice of excluding a break-glass admin account from it. Used Report-only mode and Entra ID sign-in logs to show, at the log level, that the excluded account's sign-ins never even evaluate against the policy. This is a silent, standing bypass of every control the policy enforces.

**Key finding:** Break-glass exclusions are necessary, but become invisible blind spots without a recurring review process.

### 03 — [Dormant Account With Standing Access](./03-stale-contractor-account/)
Examined a test account representing an offboarded external contractor: zero sign-in activity across the full log retention window, yet active membership in a public Microsoft 365 group with real, writable SharePoint document access. Verified by signing into the dormant account directly and confirming full read/write/delete access, live.

**Key finding:** Dormant accounts carry no behavioral baseline, making them invisible to anomaly-based detection. The risk persists purely through inaction.

### 04 — [Over-Permissioned Service Principal](./04-service-principal-app-permissions/)
Granted the same test application tenant-wide **application** (not delegated) Graph permissions - mail, files, and user profiles - and authenticated as the app itself via the OAuth2 client credentials flow, with zero user involvement. Proved live impact with a direct Graph API call enumerating every user in the tenant.

**Key finding:** Application permissions remove the human from the loop entirely — there's no user to phish or notice anomalous behavior, only a client secret standing between an attacker and tenant-wide data access.

### 05 — [Over-Scoped Slack App & Post-Install Scope Creep](./05-slack-overscoped-app/)

Installed a deliberately over-scoped, plausibly-named Slack app (“Expence Tracker”) with broad read access to channel history, the user directory, and shared files, approved with a single click by a workspace member. Uncovered a secondary finding along the way: Slack enforces scopes at the individual API-endpoint level, finer-grained than the consent screen suggests, and expanding an app’s permissions post-install requires only a lightweight “reinstall” re-approval rather than a fresh review. Verified impact live by calling the Slack Web API directly with the resulting bot token to enumerate every channel and retrieve real message history.

Key finding: Scope-grant screens can understate real API-level access, and permission expansion after initial approval gets far less scrutiny than the original install — a quiet pathway for scope creep over an app’s lifetime.

---

## Methodology

Every lab in this repository was:
- Built and tested exclusively in a self-owned, isolated Microsoft 365 trial tenant.
- Documented with first-hand screenshots captured during the actual exercise.
- Cleaned up after testing (client secrets revoked, admin consent grants removed) following documentation

## Contact

Open to opportunities in red teaming, SaaS/cloud security, and application security review.
[https://www.linkedin.com/in/wafula-mwangi/]· [mwashavin@gmail.com]
