# Pull Request

## Description
<!-- Provide a brief description of the changes in this PR -->

## Related Issues
<!-- Link related issues: Closes #123, Fixes #456 -->

## Type of Change

- [ ] 🐛 Bug fix (non-breaking change fixing an issue)
- [ ] ✨ New feature (non-breaking change adding functionality)
- [ ] 💥 Breaking change (fix or feature causing existing functionality to change)
- [ ] 🔒 Security fix (vulnerability remediation)
- [ ] 📚 Documentation update
- [ ] ♻️ Code refactoring
- [ ] ⚡ Performance improvement

## Testing

### Test Coverage

- [ ] Unit tests added/updated (coverage ≥80%)
- [ ] Integration tests pass
- [ ] E2E tests pass (if applicable)
- [ ] Manual testing completed

### Test Evidence
<!-- Paste relevant test output or screenshots -->
`pytest` output here

## Security Checklist

- [ ] No secrets committed (API keys, passwords, tokens)
- [ ] Input validation implemented for user-facing endpoints
- [ ] Authentication/authorization checks in place
- [ ] OWASP Top 10 considerations addressed
- [ ] Security scan passed (Semgrep, Trivy, Bandit)
- [ ] Dependencies have no known critical vulnerabilities

## Code Quality

- [ ] Code follows project style guidelines (PEP 8, Airbnb JS)
- [ ] Self-review completed
- [ ] Complex logic documented with comments
- [ ] No linting errors (`black`, `flake8`, `eslint`)
- [ ] Type checking passes (`mypy`, TypeScript strict mode)

## Documentation

- [ ] Code comments added for complex logic
- [ ] API documentation updated (if API changes)
- [ ] Architecture docs updated (`docs/architecture/`)
- [ ] Migration guide provided (if breaking changes)
- [ ] TODO.md updated (if issues detected)

## Accessibility (Frontend changes only)

- [ ] WCAG 2.1 AA compliant
- [ ] Keyboard navigation tested
- [ ] Screen reader tested
- [ ] Color contrast verified (≥4.5:1)
- [ ] Axe accessibility scan passed

## Performance (if applicable)

- [ ] No N+1 queries introduced
- [ ] Database indexes added for new queries
- [ ] Bundle size impact acceptable (<10KB increase)
- [ ] Load testing completed (k6, Lighthouse)

## Infrastructure (if applicable)

- [ ] Terraform changes validated (`terraform plan`)
- [ ] Kubernetes manifests tested (`kubectl apply --dry-run`)
- [ ] Resource limits defined
- [ ] Monitoring/alerting configured
- [ ] Rollback procedure documented

## Deployment Notes
<!-- Any special instructions for deployment? -->

### Database Migrations

- [ ] Migration tested locally
- [ ] Migration is backwards-compatible
- [ ] Rollback script provided

### Feature Flags

- [ ] Feature flag enabled: `feature_name`
- [ ] Gradual rollout plan: [percentage/users]

### Environment Variables

<!-- List any new environment variables required -->
`NEW_CONFIG_VAR=example_value`

## Screenshots/Recordings
<!-- For UI changes, include before/after screenshots or GIFs -->

## Checklist Before Merge

- [ ] All CI checks pass
- [ ] Code reviewed by at least one team member
- [ ] Security team approval (if security-sensitive)
- [ ] Documentation reviewed by technical writer (if docs changes)
- [ ] Ready for deployment

## Post-Merge Actions

- [ ] Monitor error rates in production
- [ ] Verify metrics in Grafana dashboards
- [ ] Update changelog/release notes
- [ ] Notify stakeholders (if user-facing changes)

---

### Reviewer Notes
<!-- Guidance for reviewers: areas needing extra attention, questions -->
