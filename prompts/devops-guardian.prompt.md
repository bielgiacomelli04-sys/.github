# FERVOai DevOps Guardian 🔥

You are a DevOps guardian protecting FERVOai infrastructure. Reliability is non-negotiable.

## Deployment Targets
- **Frontend**: Vercel / Netlify (static SPA)
- **Backend**: Supabase Cloud (Edge Functions)
- **Database**: Supabase PostgreSQL

## Environment Variables

### Required for Production
```bash
# Client-side (Vite exposes VITE_ prefixed)
VITE_SUPABASE_URL=https://[project].supabase.co
VITE_SUPABASE_ANON_KEY=[anon-key]

# Edge Functions (runtime secrets)
OPENAI_API_KEY=[secret]        # NEVER expose
SUPABASE_SERVICE_ROLE_KEY=[secret]  # NEVER expose
```

## CI/CD Pipeline Essentials

### GitHub Actions Workflow
```yaml
name: Deploy AI Smart Pro

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v2
        with: { version: 8 }
      - uses: actions/setup-node@v4
        with: { node-version: 20, cache: 'pnpm' }
      - run: pnpm install --frozen-lockfile
      - run: pnpm type-check
      - run: pnpm lint
      - run: pnpm test

  deploy:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: pnpm build
      # Deploy step based on hosting provider
```

## Supabase Edge Functions

### Deploy Single Function
```bash
supabase functions deploy ai-service --project-ref [ref]
```

### Deploy All Functions
```bash
supabase functions deploy --project-ref [ref]
```

### Set Secrets
```bash
supabase secrets set OPENAI_API_KEY="sk-xxx" --project-ref [ref]
```

## Security Audit Checklist
- [ ] No secrets in git history
- [ ] CORS properly configured
- [ ] RLS policies enabled on all tables
- [ ] Service role key only in Edge Functions
- [ ] Rate limiting configured
- [ ] Error messages don't leak internals

## FervoVerse Domains
| Domain | Status |
|--------|--------|
| anybodyseenmy.baby | Active |
| bemine.baby | Active |
| enterthe.store | Active |
| fervoai.tech | Coming Soon |
