# GitHub Copilot Global Configuration & Templates

> **Universal engineering standards and GitHub Copilot instructions for all your repositories**

This is a special `.github` repository that provides global configuration inherited by all repositories in your GitHub account/organization. It includes comprehensive GitHub Copilot instructions, code quality workflows, PR/issue templates, and technical debt tracking.

## 🎯 What This Repository Does

When you create a repository named `.github` at the **user** or **organization** level, GitHub automatically makes certain files available to **all your repositories** that don't have their own versions:

- **Copilot Instructions** → Defines AI coding assistant behavior
- **Issue/PR Templates** → Standardizes contribution workflows
- **Code Owners** → Auto-assigns reviewers based on file paths
- **Workflows** → Reusable CI/CD pipelines
- **Community Health Files** → CONTRIBUTING.md, CODE_OF_CONDUCT.md, SECURITY.md

## 📁 Repository Structure

```
.github/
├── copilot-instructions.md              # 🤖 Copilot base configuration
├── CODEOWNERS                            # 👥 Code ownership mapping
├── pull_request_template.md             # ✅ PR checklist template
├── workflows/
│   └── code-quality.yml                  # 🔍 Automated quality checks
├── ISSUE_TEMPLATE/
│   ├── bug_report.md                     # 🐛 Bug report template
│   └── security_vulnerability.md         # 🔒 Security issue template
├── TODO.md                               # 📋 Technical debt tracker
└── README.md                             # 📖 This file
```

## 🚀 Quick Start

### 1. Fork or Copy This Repository

**Option A: Fork (keeps connection to updates)**

```bash
# Fork via GitHub UI, then clone
git clone https://github.com/YOUR_USERNAME/.github.git
cd .github
```

**Option B: Use as Template (independent copy)**

```bash
# Create new repo named '.github' from this template
# Then clone and customize
git clone https://github.com/YOUR_USERNAME/.github.git
cd .github
```

### 2. Customize for Your Organization

Edit these files with your team's specifics:

**`CODEOWNERS`** - Replace placeholder teams with actual GitHub usernames/teams:

```diff
- @engineering-team
+ @your-org/engineering

- @security-team
+ @your-org/security @security-lead
```

**`copilot-instructions.md`** - Adjust standards for your tech stack:

- Update Python/JS/Go style guides
- Modify test coverage thresholds
- Add company-specific security policies
- Customize documentation requirements

**`workflows/code-quality.yml`** - Configure for your environment:

- Set Python/Node.js versions
- Add/remove linters based on your stack
- Configure Codecov or other coverage tools
- Adjust security scanning tools

### 3. Push to Your GitHub Account

```bash
git add .
git commit -m "chore: customize global config for my organization"
git push origin main
```

### 4. Verify Inheritance

Create a **new test repository** and check:

- Issue/PR templates appear when creating issues/PRs
- Copilot follows instructions defined in `copilot-instructions.md`
- Workflows can be referenced via `uses: YOUR_USERNAME/.github/.github/workflows/code-quality.yml@main`

## 📘 What's Included

### 🤖 Copilot Instructions (`copilot-instructions.md`)

Comprehensive AI coding standards covering:

| Section | Description |
|---------|-------------|
| **Code Quality** | PEP 8, Airbnb JS/TS, Go formatting standards |
| **Documentation** | Google-style docstrings, architecture diagrams, API schemas |
| **Testing** | ≥80% unit coverage, integration tests, E2E patterns |
| **Security** | OWASP Top 10 2025, secrets management, input validation |
| **Accessibility** | WCAG 2.1 AA compliance, ARIA labels, keyboard nav |
| **Performance** | Database optimization, caching strategies, bundle analysis |
| **Infrastructure** | Terraform/Kubernetes best practices, monitoring setup |
| **Git Workflow** | Commit messages, branch naming, PR requirements |

### ✅ PR Template (`pull_request_template.md`)

Enforces quality gates:

- ✅ Test coverage ≥80%
- 🔒 Security checklist (OWASP, secrets scan)
- ♿ Accessibility validation (WCAG 2.1 AA)
- ⚡ Performance impact assessment
- 📚 Documentation updates
- 🚀 Deployment notes

### 🐛 Issue Templates

**Bug Report** - Structured bug reporting with:

- Reproducible steps
- Environment details
- Impact severity (Critical/High/Medium/Low)
- Error logs and screenshots

**Security Vulnerability** - Confidential security reporting with:

- CVSS scoring
- Proof of concept
- Impact assessment (CIA triad)
- Remediation timeline

### 🔍 Code Quality Workflow (`workflows/code-quality.yml`)

Automated CI checks:

**Python Quality**

- Black (formatting)
- isort (import sorting)
- flake8 (linting)
- mypy (type checking)
- Bandit (security scanning)
- pytest (tests + coverage)

**JavaScript/TypeScript Quality**

- ESLint (linting)
- TypeScript compiler (type checking)
- Prettier (formatting)
- Jest (tests + coverage)
- Axe (accessibility)

**Security Scanning**

- Semgrep (SAST - OWASP Top 10, CWE Top 25)
- Trivy (vulnerability scanner)
- TruffleHog (secret detection)
- Safety/npm audit (dependency scanning)

### 📋 Technical Debt Tracker (`TODO.md`)

Structured technical debt tracking with:

- Priority levels (P0-P3 with SLAs)
- Issue template (file, cause, fix, owner)
- Categories (Security, Performance, Accessibility, etc.)
- Resolved issues archive

## 🔧 Advanced Usage

### Referencing Workflows in Other Repos

```yaml
# In another repo's .github/workflows/ci.yml
name: CI
on: [push, pull_request]

jobs:
  quality:
    uses: YOUR_USERNAME/.github/.github/workflows/code-quality.yml@main
    secrets: inherit
```

### Overriding Global Files

If a repository has its own version of a file, it takes precedence over the global one:

| File Location | Priority |
|---------------|----------|
| `my-repo/.github/pull_request_template.md` | **1st** (used) |
| `.github/.github/pull_request_template.md` | 2nd (ignored) |

### Multi-Language Projects

The workflow gracefully handles missing languages:

- Python checks skip if no `requirements-dev.txt`
- JS checks skip if no `package.json`
- Security scans run on all projects

## 🛡️ Security Considerations

This repository is **PUBLIC** by design (for template sharing). **Never commit**:

- ❌ API keys, tokens, passwords
- ❌ Internal infrastructure details
- ❌ Proprietary business logic
- ❌ Customer data or PII

Keep sensitive configuration in:

- Private repositories
- GitHub Secrets
- Vault/Parameter Store

## 📝 Maintenance

### Update Frequency

| Component | Recommended Cycle |
|-----------|-------------------|
| Copilot Instructions | Quarterly |
| Security Policies | As threats evolve |
| Tool Versions | Monthly (Python, Node.js) |
| Workflow Actions | When new versions release |

### Keeping in Sync

```bash
# Pull latest changes
git pull origin main

# Review and merge into your fork
git remote add upstream https://github.com/ORIGINAL_AUTHOR/.github.git
git fetch upstream
git merge upstream/main
```

## 🤝 Contributing

Improvements welcome! This is a **community-driven template**:

1. Fork this repository
2. Create feature branch (`git checkout -b feature/amazing-improvement`)
3. Make changes following the standards in `copilot-instructions.md`
4. Test with a sample project
5. Submit PR with description of improvements

## 📄 License

[MIT License](LICENSE) - Use freely, attribution appreciated

## 🙏 Acknowledgments

Built with best practices from:

- **GitHub** - Copilot API and repository inheritance
- **OWASP** - Security standards (Top 10 2025)
- **W3C** - Accessibility guidelines (WCAG 2.1)
- **Python/JavaScript/Go communities** - Style guides and tooling

---

## 📚 Additional Resources

- [GitHub Community Health Files](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)
- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [Reusable Workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows)
- [OWASP Top 10 2025](https://owasp.org/www-project-top-ten/)
- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)

---

**Last Updated**: November 2025
**Maintainer**: Community-driven
**Version**: 1.0.0
