# DevSecOps CI/CD Security Project

## 🎯 Project Overview

This project demonstrates a secure DevSecOps ecosystem using GitHub Actions, focusing on:
- **Secret Management**: Secure handling of credentials and tokens
- **Pipeline Hardening**: Protected CI/CD workflows
- **Access Control**: Role-based permissions and authentication
- **Supply Chain Security**: Dependency scanning and artifact signing

## 🔒 Security Model

### Threat Model (STRIDE)

**Primary Threats:**
- **Spoofing**: Stolen tokens to impersonate developers
- **Tampering**: Modified repos, CI configs, or artifacts
- **Repudiation**: Unsigned commits with no attribution
- **Information Disclosure**: CI logs leaking secrets
- **Denial of Service**: Pipeline abuse (build storms)
- **Elevation of Privilege**: CI service accounts escalating to production

### Security Controls

1. **Secret Management**
   - Central secret store (GitHub Secrets/Vault)
   - Masked CI variables, protected branches
   - Secret scanning + rotation policies

2. **Pipeline Hardening**
   - Protected branches with required reviews
   - Enforced CI checks before merge
   - Least privilege for runners
   - No secrets in untrusted PR pipelines

3. **Access Control**
   - Multi-Factor Authentication (MFA) required
   - Role-Based Access Control (RBAC)
   - Signed commits mandatory
   - Audited configuration changes

4. **Supply Chain Security**
   - Dependency scanning (Dependabot)
   - Pinned action versions
   - Signed artifacts

---

## 🚀 Getting Started - Collaborators Setup

### Prerequisites

All collaborators **must** complete these security requirements:

### 1. Enable Multi-Factor Authentication (MFA)

**Required for all contributors.**

1. Go to GitHub Settings → [Password and authentication](https://github.com/settings/security)
2. Click "Enable two-factor authentication"
3. Follow the setup wizard (authenticator app or SMS)

📖 [GitHub MFA Documentation](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa)

---

### 2. Configure Signed Commits (SSH Method)

**All commits must be cryptographically signed.**

#### Step 1: Generate SSH Key (if you don't have one)

```bash
# Generate new SSH key
ssh-keygen -t ed25519 -C "your.email@example.com"

# Start ssh-agent
eval "$(ssh-agent -s)"

# Add your key
ssh-add ~/.ssh/id_ed25519
```

#### Step 2: Add SSH Key to GitHub

```bash
# Copy your public key
cat ~/.ssh/id_ed25519.pub
```

1. Go to GitHub Settings → [SSH and GPG keys](https://github.com/settings/keys)
2. Click "New SSH key"
3. **Key type**: Select **"Signing Key"** (Important!)
4. Paste your public key
5. Click "Add SSH key"

#### Step 3: Configure Git for Signing

```bash
# Set SSH as signing format
git config --global gpg.format ssh

# Set your SSH key as signing key
git config --global user.signingkey ~/.ssh/id_ed25519.pub

# Enable automatic commit signing
git config --global commit.gpgsign true
```

#### Step 4: Verify It Works

```bash
# Make a test commit
git commit --allow-empty -m "Test signed commit"

# Verify signature
git log --show-signature -1
```

You should see: `Good "git" signature for your.email@example.com with ED25519 key`

📖 [GitHub Signed Commits Documentation](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits)

---

### 3. CODEOWNERS Review Requirements

Certain files require mandatory review from repository maintainers:

- `.github/workflows/` - All CI/CD pipelines
- `.github/CODEOWNERS` - Access control configuration
- `docs/security/` - Security documentation
- Any files containing `*secret*`, `*auth*`, `*deploy*`

This is automatically enforced when you open a Pull Request.

---

## 📁 Repository Structure

```
/
├── .github/
│   ├── workflows/          # CI/CD pipelines
│   │   └── verify-signatures.yml
│   ├── CODEOWNERS          # Code review requirements
│   └── dependabot.yml      # Dependency updates
├── src/                    # Source code
├── docs/
│   └── security/           # Security documentation
│       └── security.md     # Implementation details
└── README.md               # This file
```

---

## 🔐 Security Features Implemented

### ✅ Phase 4 - Security Implementation

| Feature | Status | Description |
|---------|--------|-------------|
| **Branch Protection** | ✅ Configured | Main branch requires PR reviews, status checks |
| **Signed Commits** | ✅ Enforced | All commits verified via CI workflow |
| **CODEOWNERS** | ✅ Active | Security-critical files require approval |
| **Secret Scanning** | 🔄 Planned | GitHub secret scanning enabled |
| **Dependency Scanning** | 🔄 Planned | Dependabot configuration |
| **Pipeline Hardening** | 🔄 Planned | Secure workflow templates |
| **Supply Chain Security** | 🔄 Planned | SBOM generation, artifact signing |

---

## 🤝 Contributing

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Sign** your commits (automatically if configured above)
4. **Push** to your branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Pull Request Requirements

- ✅ All commits must be signed
- ✅ CI checks must pass
- ✅ Code review approval required
- ✅ No merge conflicts
- ✅ Security-sensitive files need maintainer review

---

## 📚 Documentation

- [Security Implementation Details](./docs/security/security.md)
- [Threat Model](./docs/security/security.md#threat-model)
- [Incident Response](./docs/security/security.md#incident-response)

---

## 📞 Security Contact

To report security vulnerabilities, please contact: alisadariana@protonmail.com

**Do NOT open public issues for security vulnerabilities.**

---

## 📄 License

[TBD]
