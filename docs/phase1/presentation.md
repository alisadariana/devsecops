# DevSecOps Ecosystem Security

## State of the Art & Security Requirements

**Project Focus:** GitHub Actions, CI/CD Security, and Issue Tracking Systems

---

## Slide 1: Introduction & Project Scope

### DevSecOps Ecosystem Overview

- **Core Technologies**: GitHub Actions, Git, CI/CD Pipelines, Issue Tracking
- **Implementation**: GitHub Actions (Primary CI/CD Platform)
- **Security Focus Areas**:
  - Secret management and credential protection
  - Pipeline hardening and integrity
  - Access control and authentication
  - Supply chain security

### Critical Statistics

- **742% increase** in supply chain attacks (2019-2022)
- **25% increase** in monthly supply chain attacks (April-August 2025)
- **75%** of software supply chains reported attacks in 2024 (exceeding Gartner's 45% prediction for 2025)
- **45%** of organizations experienced supply chain attacks by 2025 (Gartner prediction - actual rate significantly higher)

---

## Slide 2: Major Security Incidents

### Recent Critical Breaches (2024-2025)

**1. tj-actions/changed-files Supply Chain Attack (March 2025)**

- **CVE-2025-30066** (CVSS: 8.6)
- **Impact**: 23,000+ repositories affected
- **Method**: Compromised GitHub Personal Access Token (PAT)
- **Result**: CI/CD secrets exposed in workflow logs (AWS keys, GitHub tokens, npm tokens, SSH keys)
- **Added to CISA KEV catalog** - Active exploitation confirmed

**2. reviewdog/action-setup Compromise (March 2025)**

- **Cascading supply chain attack** targeting v1 tag
- Malicious Base64-encoded payload injected
- Affected downstream dependencies including tj-actions

**3. Git Security Vulnerabilities (January 2025)**

- **CVE-2024-50349** and **CVE-2024-52006**
- ANSI escape sequence exploitation
- Credential theft through misleading prompts

---

## Slide 3: Threat Landscape & Attack Vectors

### OWASP Top 10 CI/CD Security Risks

1. **CICD-SEC-1**: Insufficient Flow Control Mechanisms
2. **CICD-SEC-2**: Inadequate Identity and Access Management
3. **CICD-SEC-3**: Dependency Chain Abuse
4. **CICD-SEC-4**: Poisoned Pipeline Execution (PPE)
5. **CICD-SEC-5**: Insufficient PBAC (Pipeline-Based Access Controls)
6. **CICD-SEC-6**: Insufficient Credential Hygiene
7. **CICD-SEC-7**: Insecure System Configuration
8. **CICD-SEC-8**: Ungoverned Usage of 3rd Party Services
9. **CICD-SEC-9**: Improper Artifact Integrity Validation
10. **CICD-SEC-10**: Insufficient Logging and Visibility

### Common Attack Methods

- **Credential theft** via exposed secrets in logs
- **Malicious commits** to trusted repositories
- **Pipeline poisoning** through pull request manipulation
- **Dependency confusion** and typosquatting
- **Token compromise** for unauthorized access

---

## Slide 4: Applicable Security Standards & Frameworks

### Primary Standards for CI/CD Security

**1. SLSA (Supply-chain Levels for Software Artifacts)**

- **Governed by**: OpenSSF (Open Source Security Foundation)
- **Focus**: Build integrity, provenance tracking, tamper resistance
- **Levels**: 1-4 (incremental security maturity)
- **Key Feature**: SLSA Provenance for artifact verification

**2. NIST Secure Software Development Framework (SSDF)**

- **Document**: NIST SP 800-218
- **Four Practice Groups**:
  - Prepare the Organization (PO)
  - Protect the Software (PS)
  - Produce Well-Secured Software (PW)
  - Respond to Vulnerabilities (RV)

**3. NIST SP 800-204D**

- **Title**: Strategies for Integration of Software Supply Chain Security in DevSecOps CI/CD Pipelines
- **Focus**: Secure build processes, attestations, artifact management

**4. OWASP Top 10 CI/CD Security Risks**

- **Purpose**: Identify and mitigate CI/CD-specific vulnerabilities
- **Community-driven** framework based on real-world incidents

**5. CIS Benchmarks**

- Docker Benchmark
- Kubernetes Benchmark
- Cloud provider-specific benchmarks (AWS, Azure, GCP)

---

## Slide 5: Security Requirements - CIA Triad Analysis

### Confidentiality Requirements

**Critical Assets to Protect:**

- API keys, tokens, and credentials stored as GitHub Secrets
- Source code and proprietary algorithms
- Infrastructure configuration and deployment keys
- Customer data and PII processed in pipelines

**Controls:**

- Encrypted secret storage (Libsodium sealed boxes)
- RBAC for secret access
- Environment-specific secret scoping
- Secret masking in logs (automatic redaction)
- OIDC-based authentication (short-lived tokens)

### Integrity Requirements

**Critical Needs:**

- Artifact integrity verification (prevent tampering)
- Code signing and verification
- Immutable audit logs
- Protected branches with required reviews
- Dependency verification and SBOM generation

**Controls:**

- Hash-based artifact verification
- SLSA Provenance attestations
- Commit signing with GPG keys
- Branch protection rules
- Dependency pinning to specific commit hashes

### Availability Requirements

**Critical Needs:**

- Pipeline reliability and resilience
- Rapid incident response and recovery
- Redundancy for critical infrastructure
- DDoS protection for CI/CD systems

**Controls:**

- Multiple runner availability (GitHub-hosted + self-hosted)
- Workflow retry mechanisms
- Rate limiting and resource quotas
- Comprehensive monitoring and alerting

---

## Slide 6: Risk Assessment Summary

### High-Severity Risks

| Risk Category | Likelihood | Impact | Priority |
|--------------|------------|--------|----------|
| Secret Exposure | **High** | **Critical** | P0 |
| Supply Chain Attack | **High** | **Critical** | P0 |
| Credential Theft | **High** | **High** | P1 |
| Pipeline Poisoning | **Medium** | **Critical** | P1 |
| Dependency Confusion | **Medium** | **High** | P2 |

### Financial Impact

- **Average data breach cost**: $4.88 million (2024) — 10% increase from $4.45M in 2023
- **Industry-specific costs**: Healthcare ($9.77M), Financial ($5.9M)
- **Ransomware recovery time**: 23 days average
- **Supply chain attack costs**: $60B in 2025 → projected $138B by 2031
- **CDK Global attack**: $1+ billion in losses

### Regulatory Implications

- **GDPR** compliance for data handling
- **SOC 2** requirements for security controls
- **PCI-DSS** for payment data in pipelines
- **Executive Order 14028** (US) on software supply chain security

---

## Slide 7: Key Takeaways & Next Steps

### Critical Findings

1. **Rapidly evolving threat landscape** with sophisticated attacks targeting CI/CD
2. **GitHub Actions is a prime target** - 23K+ repos affected in recent incidents
3. **Secret management is paramount** - most breaches involve credential exposure
4. **Standards exist but adoption lags** - SLSA, NIST frameworks provide clear guidance
5. **Reality exceeded predictions** - 75% of organizations hit by supply chain attacks in 2024, surpassing Gartner's 45% forecast for 2025

### Recommended Focus Areas

✓ Implement **SLSA Level 2+** provenance tracking  
✓ Adopt **NIST SSDF** practices across SDLC  
✓ Deploy **automated secret scanning** with GitGuardian/TruffleHog  
✓ Enable **branch protection** and required reviews  
✓ Use **hash-pinned actions** instead of version tags  
✓ Implement **OIDC authentication** to eliminate long-lived credentials  
✓ Deploy **comprehensive logging** and SIEM integration  
✓ Conduct **regular security audits** using OWASP Top 10 CI/CD checklist  

### Phase 2 Deliverables

- Threat model based on STRIDE methodology
- Secure CI/CD pipeline implementation using GitHub Actions
- Documentation of security controls and hardening measures
- Incident response procedures and runbooks

---

## References

- CISA KEV Catalog: CVE-2025-30066
- OWASP Top 10 CI/CD Security Risks (2022)
- NIST SP 800-218 (SSDF) & SP 800-204D
- SLSA Framework v1.0 (OpenSSF)
- GitHub Security Best Practices Documentation
- IBM Cost of a Data Breach Report 2024
- Palo Alto Networks Unit 42: Supply Chain Attack Analysis (2025)
- Cyble Threat Intelligence: Supply Chain Attacks Report (2025)
- Cybersecurity Ventures: Supply Chain Attack Cost Projections
- Sonatype: State of the Software Supply Chain Report