# PCI-DSS Gap Analysis Checklist

## Leadership Context

For engineering leadership, the gap analysis translates compliance requirements into a prioritized engineering workplan. Arriving at a QSA assessment without having done this work first costs months of remediation, risks assessment failure, and delays card-brand certifications at reputational expense.

## Purpose

This checklist supports an initial gap analysis against PCI-DSS v4.0 requirements for engineering teams operating in or adjacent to the cardholder data environment (CDE). It precedes a formal QSA assessment: it surfaces fixable gaps before the auditor arrives and identifies the ones requiring longer remediation timelines.

The checklist is organized by the 12 PCI-DSS requirement domains. Each domain section focuses on the controls engineering teams own or partially own. Compliance, legal, and security teams own some of these; boundary-spanning gaps cause the most trouble.

## Background and Motivation

This checklist was developed from the PCI-DSS gap analysis I led at ActBlue Technical Services (2022–2024), conducted in preparation for a formal QSA assessment. I led the analysis and owned the remediation roadmap; the team executed against it. The gap analysis directly preceded and informed the multi-year PCI environment deprecation program — ultimately migrating 2,600+ entity accounts to Stripe and reducing the payments codebase by 30%.

## Scope Definition (Do This First)

Before running the checklist, define CDE scope.

- [ ] Cardholder data (CHD) and sensitive authentication data (SAD) are inventoried: where it is stored, transmitted, and processed
- [ ] Systems storing, processing, or transmitting CHD are identified and documented
- [ ] Systems with potential impact on CHD security (connected systems) are identified
- [ ] Network segmentation between CDE and non-CDE is documented and validated
- [ ] Tokenization or point-to-point encryption (P2PE) scope reductions are documented with QSA sign-off

Undefined scope defaults to everything, which expands every subsequent requirement.

## Requirement 1: Network Security Controls

- [ ] Firewall/network security group rules are documented and reviewed at least every 6 months
- [ ] Inbound and outbound traffic to the CDE is restricted to documented, business-justified connections
- [ ] Direct public access to CDE components is blocked; DMZ or proxy architecture is in place
- [ ] All "deny all" rules are in place as the default posture
- [ ] Network diagrams show all connections into and out of the CDE and are current

## Requirement 2: Secure Configurations

- [ ] Default vendor passwords are changed on all CDE systems before deployment
- [ ] A system configuration standard exists and is applied consistently (hardening baseline)
- [ ] Only necessary services, protocols, and ports are enabled on CDE systems
- [ ] Configuration standards are reviewed and updated when new vulnerabilities are identified
- [ ] Infrastructure-as-code (Terraform, CloudFormation) is used to enforce configuration standards programmatically where possible

## Requirement 3: Protect Stored Account Data

- [ ] Primary account numbers (PANs) are not stored unless there is a documented business requirement
- [ ] PANs are rendered unreadable anywhere they are stored (tokenization, truncation, hashing, or encryption)
- [ ] CVV/CVC codes are never stored after authorization
- [ ] Full track data is never stored after authorization
- [ ] Data retention and disposal policies are defined and enforced; CHD is deleted on schedule
- [ ] Encryption keys are managed separately from encrypted data; key management procedures are documented

## Requirement 4: Protect Cardholder Data in Transit

- [ ] TLS 1.2 or higher is enforced for all transmission of CHD across open, public networks
- [ ] TLS 1.0 and 1.1 are disabled
- [ ] Certificates are from trusted CAs and are monitored for expiration
- [ ] Internal transmission of CHD is also encrypted (not just external-facing)
- [ ] Wireless networks transmitting CHD use strong encryption

## Requirement 5: Protect Systems Against Malware

- [ ] Anti-malware controls are deployed on all CDE systems (or a documented risk assessment supports an exception)
- [ ] Anti-malware definitions and engines are kept current
- [ ] Periodic malware scans are scheduled and results are logged
- [ ] Anti-malware cannot be disabled by non-administrative users

## Requirement 6: Secure Systems and Software

- [ ] A vulnerability management process exists: new vulnerabilities are tracked, ranked by risk, and patched within defined SLAs (critical: 1 month; high: varies by policy)
- [ ] All public-facing web applications are protected against OWASP Top 10
- [ ] A Web Application Firewall (WAF) is deployed in front of public-facing applications, or applications undergo code review against OWASP Top 10 at least annually
- [ ] Security is incorporated into the SDLC: security requirements, code review, and testing are part of the development process
- [ ] Third-party software components are inventoried and monitored for known vulnerabilities (SCA tooling)
- [ ] Bespoke and custom software undergoes security testing before production deployment

## Requirement 7: Restrict Access to System Components

- [ ] Access to CDE systems and CHD is restricted to individuals with a documented business need
- [ ] Access control is configured to deny-all by default; access is granted explicitly
- [ ] Privileged access is separated from general user access
- [ ] Access rights are reviewed at least every 6 months

## Requirement 8: Identify Users and Authenticate Access

- [ ] Every user with access to CDE systems has a unique ID; shared accounts are eliminated
- [ ] Multi-factor authentication (MFA) is required for all access into the CDE, including remote access and administrative access
- [ ] Passwords/passphrases meet minimum length and complexity requirements (PCI-DSS v4.0: minimum 12 characters)
- [ ] Inactive accounts are disabled within 90 days
- [ ] Service accounts are inventoried and access is limited to the minimum required

## Requirement 9: Restrict Physical Access

- [ ] Physical access controls for CDE hardware are in place (relevant if on-premises; cloud providers cover this for managed services)
- [ ] Media containing CHD is inventoried, classified, and disposed of securely
- [ ] Point-of-interaction (POI) devices are inspected for tampering on a defined schedule (relevant for card-present environments)

## Requirement 10: Log and Monitor All Access

- [ ] Audit logs are enabled for all CDE system components
- [ ] Logs capture: user identification, event type, date/time, success/failure, origination, and affected component
- [ ] Logs are retained for at least 12 months, with the most recent 3 months available for immediate analysis
- [ ] Logs are reviewed daily (automated log analysis tools satisfy this if configured to alert on anomalies)
- [ ] Log data is protected from modification and deletion
- [ ] Time synchronization is configured and consistent across all CDE systems (NTP)

## Requirement 11: Test Security of Systems and Networks

- [ ] Internal and external vulnerability scans are run at least quarterly
- [ ] External vulnerability scans are performed by an Approved Scanning Vendor (ASV)
- [ ] Penetration testing is performed at least annually and after significant infrastructure changes
- [ ] Penetration test scope covers both network and application layers
- [ ] Intrusion detection/prevention systems (IDS/IPS) are deployed and tuned for the CDE
- [ ] File integrity monitoring (FIM) is in place for critical CDE files

## Requirement 12: Support Information Security with Organizational Policies

- [ ] An information security policy exists, is reviewed annually, and is distributed to relevant personnel
- [ ] A risk assessment process is in place and runs at least annually
- [ ] An incident response plan exists, is tested at least annually, and assigns roles explicitly
- [ ] Third-party service providers with access to CHD are inventoried; agreements include PCI-DSS responsibility acknowledgment
- [ ] A responsible disclosure or vulnerability reporting mechanism exists for external researchers

## Common Engineering Gaps

These are the gaps the ActBlue analysis surfaced:

**Logging completeness:** Audit logs exist but do not capture all required fields, or are not retained for the full 12-month period. Fix: audit log configuration against the Requirement 10 checklist above.

**Dependency vulnerabilities:** Third-party libraries in production have known CVEs with no remediation timeline. Fix: implement SCA tooling (Dependabot, Snyk, or equivalent) with alerting and a defined patch SLA.

**Shared accounts:** Shared service account credentials in config files or CI/CD pipelines. Fix: rotate to individual service accounts or secrets management (AWS Secrets Manager, HashiCorp Vault).

**TLS version:** TLS 1.0/1.1 still enabled on internal services because "it's not customer-facing." Fix: enforce TLS 1.2+ uniformly.

**Scope creep from non-standard hosting:** Any CDE component outside the standard deployment stack (a Heroku app, a developer's personal AWS account, an unmanaged VM) expands scope and complicates compliance. Fix: migrate to standard infrastructure or formally exclude from CDE with compensating controls and QSA agreement.

**Key management:** Encryption is in place but keys are stored in the same system as the encrypted data, or key rotation procedures are undocumented. Fix: move to a dedicated key management service and document rotation procedures.
