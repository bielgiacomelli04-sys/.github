# GitHub Copilot Base Configuration

This document defines foundational engineering standards, shared behaviors, and quality expectations for all GitHub Copilot interactions across this repository. These instructions apply globally and are inherited by custom agents unless overridden locally.

---

## 1. Code Quality Standards

### Python
- PEP 8 (2025 revision)
  - 4-space indentation
  - Max line length: 79 (code) / 72 (docstrings)
  - Names: `snake_case` (func/vars), `PascalCase` (classes), `UPPER_SNAKE_CASE` (constants)
- Type hints required (Python ≥3.10)
- Google-style docstrings w/ example

```python
def calculate_discount(price: float, discount_rate: float) -> float:
    """Calculate discounted price with validation.

    Args:
        price: Original price in USD (must be positive)
        discount_rate: Discount as decimal (0.0-1.0)

    Returns:
        Final price after discount application.

    Raises:
        ValueError: If price is negative or discount_rate out of range.

    Example:
        >>> calculate_discount(100.0, 0.15)
        85.0
    """
    if price < 0 or not 0 <= discount_rate <= 1:
        raise ValueError("Invalid input range")
    return price * (1 - discount_rate)
```

### JavaScript / TypeScript
- Airbnb + TypeScript ESLint rules
- 2-space indentation
- Single quotes; trailing commas in multi-line structures
- Explicit return types
- Strict TypeScript mode

```ts
interface User {
  id: string
  email: string
  createdAt: Date
}

function formatUser(user: User): string {
  return `${user.email} (joined ${user.createdAt.toLocaleDateString()})`
}
```

### Go
- `gofmt` enforced; lint with `golint`, vet with `go vet`
- Explicit error handling, wrapping with context

```go
res, err := performOperation()
if err != nil {
    return fmt.Errorf("performOperation failed: %w", err)
}
```

---

## 2. Documentation Requirements

### Inline
- Explain the "why" of non-trivial logic (>30s to understand)
- Avoid redundant comments describing obvious code

### Architecture (`docs/`)
- `docs/architecture/`: Mermaid diagrams + data flow
- `docs/api/`: OpenAPI / GraphQL schemas & auth flows
- `docs/database/`: Schema diagrams, migration rationale
- `docs/runbooks/`: Operational + incident response guides

### Component Doc Template
```
[Component Name]
Overview: 2–3 sentence purpose
Architecture: Diagram + key decisions
Rationale: Why this approach; alternatives
Tradeoffs: Decision | Benefit | Cost table
Dependencies: External + internal
Rollback: Steps to safely revert
```

---

## 3. Testing Standards

- Unit: ≥80% line coverage (core logic)
- Integration: ≥70% API + DB
- E2E: All critical user journeys

AAA pattern example:
```python
def test_user_registration_success():
    """Successful registration with valid data."""
    # Arrange
    user_data = {"email": "new@example.com", "password": "Secure123!"}
    # Act
    response = client.post("/api/v1/register", json=user_data)
    # Assert
    assert response.status_code == 201
    assert response.json()["email"] == user_data["email"]
```

Naming: `test_<function>_<condition>_<expected>`

---

## 4. Security Standards (OWASP Top 10 2025)

Key controls:
1. Broken Access Control – least privilege, deny-by-default
2. Cryptographic Failures – strong hashing (argon2/bcrypt), TLS 1.3
3. Injection – parameterized queries, input sanitization
4. Insecure Design – threat modeling for new features
5. Security Misconfiguration – disable debug, remove defaults
6. Vulnerable Components – automated dependency scanning
7. Authentication Failures – MFA, rate limiting, secure sessions
8. Integrity Failures – signed artifacts, pipeline integrity
9. Logging/Monitoring – central aggregation + anomaly alerts
10. SSRF – strict URL validation + network segmentation

Secrets management: never commit; vault+env; pre-commit scanning.

---

## 5. Accessibility (WCAG 2.1 AA)
- Alt text for images
- Contrast ≥4.5:1 normal / ≥3:1 large text
- Keyboard accessible; visible focus (≥2px)
- ARIA roles/attributes for custom widgets

```tsx
// Accessible button
<button
  type="button"
  onClick={handleSubmit}
  disabled={isLoading}
  aria-label="Submit registration form"
  aria-busy={isLoading}
>
  {isLoading ? 'Submitting…' : 'Submit'}
  </button>
```

---

## 6. Performance Benchmarks
Backend: p95 <200ms, p99 <500ms; avoid N+1; cache hot paths (Redis)
Frontend: LCP <2.5s, FID <100ms, CLS <0.1; initial bundle <250KB gz; lazy-load large assets (WebP/AVIF)

---

## 7. Infrastructure Standards
Kubernetes: non-root, resource requests+limits, probes, network policies.
Terraform: remote state, validated modules, documented outputs.
CI/CD: SAST (Semgrep), DAST (ZAP), dependency scanning (Trivy), secret detection.
Observability: structured logs + metrics + tracing; deployment event emission.

---

## 8. Automated Issue Detection
Scan for: security vulns (SQLi, XSS, secrets), perf (N+1, O(n²)), accessibility (missing alt/ARIA), code smells (duplication, deep nesting), maintenance issues.
Log findings to `TODO.md` with severity (🔴/🟡/🟢) + root cause + proposed fix.

---

## 9. Verification Workflow

### Static Analysis
Python: `black`, `isort`, `flake8`, `mypy --strict`, `bandit`
JS/TS: `eslint`, `tsc --noEmit`, `prettier --check`
Go: `gofmt`, `golint`, `go vet`

### Testing
Python: `pytest --cov=app --cov-fail-under=80`
JS/TS: `npm test -- --coverage`
Security: targeted tests (`pytest -m security` / `npm run test:security`)
Accessibility: axe + Lighthouse CI

### Security Scanning
Semgrep rulesets (security-audit, owasp-top-ten, cwe-top-25)
Dependency: `safety check`, `npm audit --audit-level=high`
Container: `trivy image` for HIGH/CRITICAL

### Code Review Checklist
- Functionality & edge cases
- Tests & coverage thresholds
- Docs updated (architecture/API) for significant changes
- Security (no new OWASP risks)
- Performance (no regressions)
- Accessibility compliance
- Style/lint pass
- Dependency health
- Backwards compatibility / migrations
- Observability (logs/metrics/errors)

---

## 10. Continuous Improvement
Provide optimization proposals with quantified benefits, tradeoffs, rollback plan.

Format:
```
Proposed Optimization: Title
Current State: summary
Change: proposal
Benefits: list (quantify when possible)
Costs: list
Implementation: steps
Rollback: steps
```

---

## 11. Communication Standards
Tone: professional, explanatory, transparent, confident, educational.
Structure: Summary → Implementation → Verification → Documentation → Issues Found → Next Steps.
Avoid: incomplete snippets, unstated assumptions, vague advice.

---

## 12. Tool Config Examples

### `pytest.ini`
```ini
[pytest]
testpaths = tests
python_files = test_*.py
python_classes = Test*
python_functions = test_*
addopts = --verbose --strict-markers --cov=app --cov-report=term-missing --cov-report=html --cov-fail-under=80
markers =
  unit: Unit tests
  integration: Integration tests
  e2e: End-to-end tests
  security: Security tests
  slow: Slow-running tests
```

### ESLint (excerpt)
```json
{
  "extends": [
    "airbnb-typescript",
    "plugin:@typescript-eslint/recommended",
    "plugin:jsx-a11y/recommended",
    "plugin:security/recommended"
  ],
  "rules": {
    "max-len": ["error", { "code": 100 }],
    "@typescript-eslint/explicit-function-return-type": "error",
    "jsx-a11y/anchor-is-valid": "error",
    "no-console": "warn"
  }
}
```

### `.pre-commit-config.yaml` (excerpt)
```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.5.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-json
      - id: detect-private-key
  - repo: https://github.com/psf/black
    rev: 23.12.1
    hooks:
      - id: black
        args: [--line-length=79]
  - repo: https://github.com/pycqa/isort
    rev: 5.13.2
    hooks:
      - id: isort
        args: [--profile=black]
  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets
```

---

## 13. Emergency Procedures
Rollback:
```bash
kubectl rollout undo deployment/app-name -n production
git revert <commit-sha>
git push origin main
```
Security Incident:
Assess severity → Contain → Patch → Deploy → Notify → Post-mortem (`docs/security/incidents/`).

---

## 14. Version Control Standards
Commits: `<type>(<scope>): <subject>` where type ∈ {feat, fix, docs, style, refactor, test, chore, security}.
Include BREAKING CHANGE footer when needed.
Branches: `feature/`, `fix/`, `security/`, `hotfix/` prefixes.

Audit Log Example:
```json
{
  "timestamp": "2025-11-11T04:05:00Z",
  "actor": "user@example.com",
  "action": "DELETE",
  "resource": "users/12345",
  "ip_address": "203.0.113.42",
  "user_agent": "Mozilla/5.0...",
  "result": "success"
}
```

---

## 15. Reference Links
Python: https://docs.python.org/3/
React: https://react.dev/
TypeScript: https://www.typescriptlang.org/docs/
Kubernetes: https://kubernetes.io/docs/
Terraform: https://developer.hashicorp.com/terraform/docs
OWASP Top 10: https://owasp.org/www-project-top-ten/
CWE Top 25: https://cwe.mitre.org/top25/
WCAG 2.1: https://www.w3.org/WAI/WCAG21/quickref/
ARIA: https://www.w3.org/WAI/ARIA/apg/
Pytest: https://docs.pytest.org/
Jest: https://jestjs.io/docs/
Playwright: https://playwright.dev/

---

_Last Updated: 2025-11-11 · Maintainer: Engineering Team · Review Cycle: Quarterly_
1. .github/copilot-instructions.md
text
# GitHub Copilot Base Configuration

This document defines the foundational engineering standards, shared behaviors, and quality expectations for all GitHub Copilot interactions in this repository. These instructions apply globally and are inherited by all custom agents.

---

## Core Engineering Principles

### 1. Code Quality Standards

#### Python
- **Style Guide**: PEP 8 (2025 revision)
  - 4-space indentation (never tabs)
  - Max line length: 79 characters (code), 72 characters (docstrings)
  - Function/variable naming: `snake_case`
  - Class naming: `PascalCase`
  - Constants: `UPPER_SNAKE_CASE`
- **Type Hints**: Required for all function signatures (Python 3.10+ syntax)
- **Docstrings**: Google-style format with examples
def calculate_discount(price: float, discount_rate: float) -> float:
"""Calculate discounted price with validation.

text
  Args:
      price: Original price in USD (must be positive)
      discount_rate: Discount as decimal (0.0-1.0)

  Returns:
      Final price after discount application

  Raises:
      ValueError: If price is negative or discount_rate out of range

  Example:
      >>> calculate_discount(100.0, 0.15)
      85.0
  """
text

#### JavaScript/TypeScript
- **Style Guide**: Airbnb JavaScript Style Guide + TypeScript ESLint
- 2-space indentation
- Single quotes for strings
- Trailing commas in multi-line structures
- Explicit return types for functions
- **Type Safety**: Strict TypeScript mode enabled
interface User {
id: string;
email: string;
createdAt: Date;
}

function formatUser(user: User): string {
return ${user.email} (joined ${user.createdAt.toLocaleDateString()});
}

text

#### Go
- **Style Guide**: Official Go formatting (`gofmt`, `golint`)
- **Error Handling**: Always check errors explicitly
result, err := performOperation()
if err != nil {
return fmt.Errorf("operation failed: %w", err)
}

text

### 2. Documentation Requirements

#### Inline Documentation
- **Purpose**: Explain *why*, not *what* (code should be self-documenting for "what")
- **Complexity Threshold**: Comment any logic that takes >30 seconds to understand
- **Examples**:
✅ GOOD: Explains reasoning
Use exponential backoff to avoid overwhelming the API during retries
time.sleep(2 ** attempt)

❌ BAD: States the obvious
Sleep for 2 to the power of attempt
time.sleep(2 ** attempt)

text

#### Architecture Documentation (`docs/`)
Every significant change must update or create documentation in `docs/`:

- **`docs/architecture/`**: System design, component diagrams (Mermaid), data flow
- **`docs/api/`**: API contracts (OpenAPI/GraphQL schemas), authentication flows
- **`docs/database/`**: Schema diagrams, migration guides, indexing strategies
- **`docs/runbooks/`**: Operational procedures, incident response, troubleshooting

#### Documentation Template Structure
[Component Name]
Overview
High-level purpose and context (2-3 sentences)

Architecture
Component diagram (Mermaid) and key design decisions

Rationale
Why this approach? What alternatives were considered?

Tradeoffs
Decision	Benefit	Cost
Example	...	...
Dependencies
External services

Internal modules

Required configuration

Rollback Procedure
Step-by-step instructions to revert changes safely

text

### 3. Testing Standards

#### Test Coverage Requirements
- **Unit Tests**: ≥80% line coverage for business logic
- **Integration Tests**: ≥70% coverage for API endpoints and database operations
- **E2E Tests**: 100% coverage of critical user journeys

#### Test Structure (AAA Pattern)
def test_user_registration_success():
"""Test successful user registration with valid data."""
# Arrange
user_data = {"email": "new@example.com", "password": "Secure123!"}

text
# Act
response = client.post("/api/v1/register", json=user_data)

# Assert
assert response.status_code == 201
assert "id" in response.json()
assert response.json()["email"] == user_data["email"]
text

#### Test Naming Convention
- **Pattern**: `test_<function>_<condition>_<expected_result>`
- **Examples**:
  - `test_create_user_with_valid_data_succeeds`
  - `test_login_with_invalid_password_returns_401`
  - `test_delete_resource_without_permission_forbidden`

### 4. Security Requirements

#### OWASP Top 10 (2025) Compliance
All code must be validated against the latest OWASP Top 10:

1. **A01:2025 - Broken Access Control**
   - Implement authorization checks at every endpoint
   - Use principle of least privilege
   - Default deny access

2. **A02:2025 - Cryptographic Failures**
   - Never store passwords in plain text (use bcrypt/argon2)
   - Use TLS 1.3 for data in transit
   - AES-256 for data at rest

3. **A03:2025 - Injection**
   - Always use parameterized queries
   - Sanitize user input (whitelist, not blacklist)
   - Escape output based on context (HTML, SQL, OS commands)

4. **A04:2025 - Insecure Design**
   - Threat modeling for new features
   - Security requirements in design phase
   - Secure defaults

5. **A05:2025 - Security Misconfiguration**
   - Disable debug mode in production
   - Remove default credentials
   - Principle of least privilege for services

6. **A06:2025 - Vulnerable and Outdated Components**
   - Automated dependency scanning (Dependabot, Renovate)
   - No dependencies with known critical CVEs
   - Regular security updates

7. **A07:2025 - Authentication Failures**
   - Multi-factor authentication for sensitive operations
   - Rate limiting on authentication endpoints
   - Secure session management

8. **A08:2025 - Software and Data Integrity Failures**
   - Verify signatures of dependencies
   - Use CI/CD pipeline integrity checks
   - Immutable infrastructure

9. **A09:2025 - Logging and Monitoring Failures**
   - Log authentication events (success/failure)
   - Alert on suspicious patterns
   - Centralized log aggregation

10. **A10:2025 - Server-Side Request Forgery (SSRF)**
    - Validate and sanitize URLs
    - Whitelist allowed protocols and domains
    - Network segmentation

#### Secrets Management
- **Never commit secrets**: Use `.gitignore` patterns for sensitive files
- **Environment variables**: Store secrets in secure vaults (AWS Secrets Manager, HashiCorp Vault)
- **Scanning**: Pre-commit hooks with `detect-secrets` or `trufflehog`

### 5. Accessibility Standards (WCAG 2.1 AA)

#### Perceivable
- **1.1.1 Non-text Content**: All images have alt text
- **1.4.3 Contrast Minimum**: Color contrast ≥4.5:1 for normal text, ≥3:1 for large text

#### Operable
- **2.1.1 Keyboard**: All functionality available via keyboard
- **2.4.7 Focus Visible**: Visible focus indicators (2px outline minimum)

#### Understandable
- **3.3.1 Error Identification**: Clear error messages with suggestions
- **3.3.2 Labels or Instructions**: All form inputs have associated labels

#### Robust
- **4.1.2 Name, Role, Value**: Proper ARIA attributes for custom components

// ✅ ACCESSIBLE: Semantic HTML + ARIA
<button
type="button"
onClick={handleSubmit}
disabled={isLoading}
aria-label="Submit registration form"
aria-busy={isLoading}

{isLoading ? 'Submitting...' : 'Submit'}
</button>

// ❌ INACCESSIBLE: Non-semantic div

<div onClick={handleSubmit}>Submit</div> ```
6. Performance Standards
Backend
API Response Time: p95 <200ms, p99 <500ms

Database Queries: No N+1 patterns, use eager loading

Caching: Redis for frequently accessed data (TTL-based invalidation)

Frontend
Core Web Vitals (2025 targets):

LCP (Largest Contentful Paint): <2.5s

FID (First Input Delay): <100ms

CLS (Cumulative Layout Shift): <0.1

Bundle Size: Initial load <250KB (gzipped)

Image Optimization: WebP/AVIF formats, lazy loading

7. Infrastructure Standards
Kubernetes
Security Context: Always set runAsNonRoot: true

Resource Limits: Define both requests and limits

Health Checks: Liveness and readiness probes required

Network Policies: Default deny, explicit allow

Terraform
State Management: Remote backend (S3 + DynamoDB for locking)

Modules: Reusable modules for common patterns

Validation: Input variable validation with custom error messages

Documentation: Output values with descriptions

CI/CD
Security Scanning: SAST (Semgrep), DAST (OWASP ZAP), dependency scanning (Trivy)

Test Gates: All tests must pass before merge

Deployment Strategy: Rolling updates with automatic rollback on failure

Observability: Emit deployment events to monitoring systems

Bug Detection & Logging
Proactive Code Analysis
When generating or reviewing code, automatically scan for:

Security Vulnerabilities

SQL injection (string interpolation in queries)

XSS (unescaped user input in HTML)

Hardcoded secrets (API keys, passwords)

Missing authentication/authorization checks

Insecure deserialization

Performance Issues

N+1 query patterns

Missing database indexes

Inefficient algorithms (O(n²) where O(n log n) possible)

Memory leaks (unclosed resources)

Accessibility Violations

Missing alt text on images

Insufficient color contrast

Missing ARIA labels on interactive elements

Non-semantic HTML (<div> as button)

Code Smells

God classes/functions (>200 lines)

Deep nesting (>4 levels)

Duplicate code (DRY violations)

Magic numbers without constants

Commented-out code

Issue Logging Format
TODO.md Structure
text
## Detected Issues - [YYYY-MM-DD HH:MM:SS UTC]

### 🔴 CRITICAL: [Brief Description]
- **File**: `path/to/file.ext:line_number`
- **Category**: Security | Performance | Accessibility | Bug
- **Cause**: Root cause explanation
- **Current Code**:
  ```language
  // Problematic code snippet
Fix:

text
// Corrected code snippet
Impact: Describe user/system impact

References: [OWASP/CWE/WCAG links if applicable]

Confidence: 0-100% (based on detection certainty)

🟡 MEDIUM: [Brief Description]
[Same structure as above]

🟢 LOW: [Brief Description]
[Same structure as above]

text

#### Severity Classification
- **🔴 CRITICAL**: Security vulnerabilities, data loss risks, system crashes
- **🟡 MEDIUM**: Performance degradation, accessibility barriers, maintainability issues
- **🟢 LOW**: Code style violations, minor optimizations, documentation gaps

#### When to Create GitHub Issues vs TODO.md
- **GitHub Issues**: Security vulnerabilities (HIGH/CRITICAL), breaking bugs affecting users
  - Mark as `confidential` for security issues
  - Tag with `security`, `bug`, `accessibility`, etc.
  - Assign to relevant team/owner
- **TODO.md**: Code smells, technical debt, minor improvements, refactoring suggestions

---

## Verification Workflow

Every code change must include verification steps:

### 1. Static Analysis
Python
black . --check --line-length=79
isort . --check-only --profile=black
flake8 . --max-line-length=79
mypy . --strict
bandit -r . -ll

JavaScript/TypeScript
npm run lint
npm run typecheck
npm run format:check

Go
gofmt -l .
golint ./...
go vet ./...

text

### 2. Testing
Run all tests with coverage
pytest --cov=app --cov-report=html --cov-fail-under=80
npm run test -- --coverage --watchAll=false

Security tests
npm run test:security
pytest tests/security/

text

### 3. Security Scanning
SAST
semgrep --config=p/security-audit --config=p/owasp-top-ten .

Dependency scanning
safety check --json
npm audit --audit-level=high

Container scanning (if applicable)
trivy image myapp:latest --severity HIGH,CRITICAL

text

### 4. Accessibility Testing
Automated a11y testing
npm run test:a11y
axe-core --url http://localhost:3000

Lighthouse CI
lhci autorun --collect.url=http://localhost:3000

text

---

## Code Review Checklist

Before submitting code (or suggesting code), verify:

- [ ] **Functionality**: Code works as intended with edge cases handled
- [ ] **Tests**: New code has ≥80% test coverage
- [ ] **Documentation**: Complex logic explained, API docs updated
- [ ] **Security**: No OWASP Top 10 vulnerabilities introduced
- [ ] **Performance**: No obvious performance regressions
- [ ] **Accessibility**: WCAG 2.1 AA compliant (frontend)
- [ ] **Style**: Passes linting and formatting checks
- [ ] **Dependencies**: No new vulnerabilities introduced
- [ ] **Backwards Compatibility**: Migration path for breaking changes
- [ ] **Observability**: Logging, metrics, and error tracking added

---

## Continuous Improvement

### Suggest Optimizations When:
1. **Performance**: Profiling shows bottlenecks
2. **Security**: New CVE affects dependencies
3. **Maintainability**: Code complexity metrics exceed thresholds
4. **Cost**: Cloud spend can be reduced without sacrificing quality
5. **Developer Experience**: Automation opportunities identified

### Optimization Format
Proposed Optimization: [Title]
Current State
[Describe existing implementation]

Proposed Change
[Describe suggested improvement]

Benefits
Benefit 1 (quantified when possible)

Benefit 2

Costs/Tradeoffs
Cost 1 (time, complexity, risk)

Cost 2

Implementation Plan
Step 1

Step 2

Step 3

Rollback Strategy
[How to revert if needed]

text

---

## Communication Standards

### Tone & Style
- **Professional**: Direct, analytical, structured
- **Explanatory**: Always justify decisions with "Why this is optimal"
- **Transparent**: Document tradeoffs honestly
- **Confident**: Avoid hedging language ("might", "maybe", "possibly") when facts are clear
- **Educational**: Reference official documentation and best practices

### Response Structure
1. **Summary**: 1-2 sentence overview of what was done
2. **Implementation**: Complete, runnable code with inline comments
3. **Verification**: Exact commands to test/lint/validate
4. **Documentation**: Architecture notes, design rationale
5. **Issues Found**: TODO.md entries or GitHub Issues
6. **Next Steps**: Suggested follow-up actions (if any)

### Avoid
- Placeholder comments like `// TODO: implement this`
- Incomplete code snippets without context
- Generic advice without specific examples
- Assumptions without stating them explicitly

---

## Tool-Specific Configurations

### Pytest Configuration
pytest.ini
[pytest]
testpaths = tests
python_files = test_.py
python_classes = Test
python_functions = test_*
addopts =
--verbose
--strict-markers
--cov=app
--cov-report=term-missing
--cov-report=html
--cov-fail-under=80
markers =
unit: Unit tests
integration: Integration tests

security: Security tests
slow: Slow-running tests

text

### ESLint Configuration
{
"extends": [
"airbnb-typescript",
"plugin:@typescript-eslint/recommended",
"plugin:jsx-a11y/recommended",
"plugin:security/recommended"
],
"rules": {
"max-len": ["error", { "code": 100 }],
"@typescript-eslint/explicit-function-return-type": "error",
"jsx-a11y/anchor-is-valid": "error",
"no-console": "warn"
}
}

text

### Pre-commit Hooks
.pre-commit-config.yaml
repos:

repo: https://github.com/pre-commit/pre-commit-hooks
rev: v4.5.0
hooks:

id: trailing-whitespace

id: end-of-file-fixer

id: check-yaml

id: check-json

id: detect-private-key

repo: https://github.com/psf/black
rev: 23.12.1
hooks:

id: black
args: [--line-length=79]

repo: https://github.com/pycqa/isort
rev: 5.13.2
hooks:

id: isort
args: [--profile=black]

repo: https://github.com/Yelp/detect-secrets
rev: v1.4.0
hooks:

id: detect-secrets

text

---

## Emergency Procedures

### Rollback Protocol
If a deployment causes production issues:

1. **Immediate**: Revert to last known good version
kubectl rollout undo deployment/app-name -n production

OR
git revert <commit-sha>
git push origin main

text

2. **Communicate**: Notify team via incident channel

3. **Investigate**: Preserve logs and metrics for post-mortem

4. **Document**: Create incident report in `docs/incidents/YYYY-MM-DD-title.md`

### Security Incident Response
If a security vulnerability is discovered in production:

1. **Assess**: Determine severity (CVSS score) and exposure
2. **Contain**: Apply temporary mitigations (WAF rules, rate limiting)
3. **Fix**: Develop and test patch
4. **Deploy**: Emergency deployment with expedited review
5. **Notify**: Inform affected users if data breach occurred (per compliance requirements)
6. **Post-Mortem**: Document in `docs/security/incidents/`

---

## Version Control Standards

### Commit Message Format
<type>(<scope>): <subject>

<body> <footer> ```
Types: feat, fix, docs, style, refactor, test, chore, security

Example:

text
feat(auth): add multi-factor authentication support

Implement TOTP-based MFA using pyotp library. Users can enable
MFA in account settings and must provide 6-digit code on login.

Closes #234
BREAKING CHANGE: Login endpoint now requires 'mfa_token' field when MFA is enabled
Branch Naming
Feature: feature/short-description

Bug Fix: fix/issue-number-description

Security: security/vulnerability-description

Hotfix: hotfix/critical-issue

Pull Request Template
text
## Description
[Describe changes]

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Security fix
- [ ] Documentation update

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests pass
- [ ] Manual testing completed

## Security
- [ ] No new vulnerabilities introduced
- [ ] Security scan passed
- [ ] Secrets not committed

## Documentation
- [ ] Code comments added
- [ ] API documentation updated
- [ ] Architecture docs updated (if applicable)

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Tests provide adequate coverage (≥80%)
- [ ] No breaking changes (or documented)
Compliance & Audit
Data Privacy (GDPR/CCPA)
User Consent: Explicit opt-in for data collection

Right to Deletion: Implement user data export and deletion endpoints

Data Minimization: Collect only necessary information

Encryption: PII encrypted at rest and in transit

Audit Logging
text
# Example: Audit log format
{
  "timestamp": "2025-11-11T04:05:00Z",
  "actor": "user@example.com",
  "action": "DELETE",
  "resource": "users/12345",
  "ip_address": "203.0.113.42",
  "user_agent": "Mozilla/5.0...",
  "result": "success"
}
Appendix: Reference Links
Official Documentation
Python: https://docs.python.org/3/

React: https://react.dev/

TypeScript: https://www.typescriptlang.org/docs/

Kubernetes: https://kubernetes.io/docs/

Terraform: https://developer.hashicorp.com/terraform/docs

Security Resources
OWASP Top 10: https://owasp.org/www-project-top-ten/

CWE Top 25: https://cwe.mitre.org/top25/

NIST Cybersecurity: https://www.nist.gov/cyberframework

Accessibility
WCAG 2.1: https://www.w3.org/WAI/WCAG21/quickref/

ARIA: https://www.w3.org/WAI/ARIA/apg/

Testing
Pytest: https://docs.pytest.org/

Jest: https://jestjs.io/docs/

Playwright: https://playwright.dev/

Last Updated: 2025-11-11
Maintainer: Engineering Team
Review Cycle: Quarterly

text
