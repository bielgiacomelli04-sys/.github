# FERVOai Workspace - GitHub Copilot Instructions

> 🔥 **The Holy Manuscript of Neon Nonsense** — Comprehensive guidance for AI coding agents working in the FERVOai multiverse, including AI Smart Pro and all FervoVerse projects.

---

## 🎯 Brand Essence (READ THIS FIRST)

**FERVOai exists to slap boredom across the face.** We're the rebellious AI brand that treats automation like a martial art and bureaucracy like a final boss. Everything we touch must feel **alive, loud, pixelated, and slightly unhinged**.

### Core Philosophy
- **Micro-products** that solve your Tuesday afternoon
- **Brazilian-first**, world-focused
- **Pixel-perfect** retro-futuristic aesthetic
- **Irreverent** but helpful
- **Fast, Direct, Cheap, Unapologetic**

### Brand Voice
- **Loud and confident** — retro as hell
- **Profane when appropriate** — and it's almost always appropriate
- **Humor is mandatory** — subtlety is optional
- **Playful but not childish**
- **Aggressive but not obnoxious**
- **Helpful but doesn't kiss ass**

> Example CTA: "Quer resolver? Copia o prompt e foda-se o resto."

---

## 🎨 FERVOai Visual Identity (MANDATORY)

### Color Palette
```css
/* PRIMARY PALETTE - USE THESE */
--fervo-red: #FF2D2D;        /* Danger, passion, deadlines exploding */
--fervo-yellow: #FFD82A;     /* Chaos you pretend to have under control */
--fervo-cyan: #00D1D0;       /* Neon promises, cyber blue */
--fervo-green: #1EFF9B;      /* System overheating, acid green */
--fervo-white: #EFEFEF;      /* Contrast - even maniacs need it */
--fervo-black: #000000;      /* Background base */
```

### Typography
- **Titles/Headlines**: `Bebas Neue` — SCREAMING, bold, unapologetic
- **Body Text**: `Inter` — clean, readable, professional contrast
- **Hierarchy**: Clear, bold, unafraid to stare society in the face

```css
/* Font stack */
font-family: 'Inter', sans-serif;  /* Body */
font-family: 'Bebas Neue', sans-serif;  /* Headlines */
```

### Visual Style Requirements
- **Pixel art aesthetic** — 16-bit, arcade, retro-futurist
- **CRT scanline effects** when appropriate
- **Glitch animations** for emphasis
- **High contrast** with strong outlines
- **Fire/flame motifs** — pixel fire is core iconography
- **Dark backgrounds** (#000000) with neon accents

### Creative Directives
```
IF NOT FUN → IT DIES
IF NOT PIXELATED → IT BURNS
IF NOT BOLD → THROWN OUT THE WINDOW
```

---

## 🏗️ Project Overview

### Workspace Structure
```
ai-pro/                              # Root workspace (FERVOai multiverse)
├── ai-smart-pro/                    # AI Smart Pro - Main SaaS application
│   ├── src/
│   │   ├── components/              # React components (PascalCase.tsx)
│   │   ├── contexts/AuthContext.tsx # Global auth state
│   │   ├── lib/supabase.ts          # API clients & services
│   │   └── App.tsx                  # Main app with services config
│   └── supabase/functions/          # Deno Edge Functions
├── supabase/                        # Shared database schema
│   ├── tables/                      # SQL table definitions
│   └── migrations/                  # Database migrations
├── docs/                            # Additional documentation
├── imgs/                            # Image assets
│
├── # FERVOai Brand Resources (ROOT)
├── FERVOai Brand Bible — Unfiltered Edition.md
├── fervoai_brand_documentation.md
├── index.html                       # FERVOai landing page reference
├── flame_*.png                      # Fire/flame pixel art assets
└── README.md                        # Project overview
```

### AI Smart Pro Overview
A production SaaS platform offering 6 AI-powered business services with a credit-based payment system:

1. **Content Generation** — Blog posts, marketing copy
2. **Translation** — 100+ languages
3. **Data Analysis** — CSV/Excel insights
4. **Audio Transcription** — Speech-to-text
5. **Code Generation** — Multi-language
6. **Image Generation** — AI image prompts

**Tech Stack**: React 18 + TypeScript + Vite + TailwindCSS + Supabase + OpenAI GPT-4o-mini

---

## 🔥 Architecture

### Request Flow
```
User → AuthContext (JWT) → Edge Function → Credit Check → OpenAI API → DB Update → Response
```

### Key Files Reference

| File | Purpose |
|------|---------|
| `ai-smart-pro/src/lib/supabase.ts` | All API clients (aiService, userService, paymentService) |
| `ai-smart-pro/src/contexts/AuthContext.tsx` | Global auth state, user profile, credits |
| `ai-smart-pro/supabase/functions/ai-service/index.ts` | Main AI processing Edge Function |
| `ai-smart-pro/src/App.tsx` | Service definitions, routing |
| `ai-smart-pro/src/components/ServiceWizard.tsx` | 6-step AI service interface |
| `FERVOai Brand Bible — Unfiltered Edition.md` | Brand voice & creative directives |
| `fervoai_brand_documentation.md` | Complete brand & product documentation |
| `index.html` | FERVOai landing page reference implementation |

---

## 💻 Code Patterns & Standards

### Authentication (ALWAYS use AuthContext)
```typescript
import { useAuth } from '../contexts/AuthContext';

const MyComponent = () => {
  const { user, profile, session, loading, refreshProfile } = useAuth();

  if (loading) return <Spinner />;
  if (!user) return <AuthModal />;  // NEVER bypass auth

  // profile.credits contains user's credit balance
  return <AuthenticatedContent credits={profile?.credits || 0} />;
};
```

### AI Service Calls (Always with JWT)
```typescript
import { aiService } from '../lib/supabase';

const response = await aiService.processService(
  'content-generation',  // serviceType
  prompt,                // user input
  { quality: 'high', language: 'English' }  // ServiceConfig
);
// Returns: { result: string, tokensUsed: number, serviceType: string }
```

### Component Structure (Follow this order)
```typescript
// 1. Imports (React, deps, internal, types)
import React, { useState, useEffect } from 'react';
import { useAuth } from '../contexts/AuthContext';
import { Button } from './ui/button';

// 2. Interface definitions
interface FervoComponentProps {
  title: string;
  onComplete: () => void;
  variant?: 'fire' | 'neon' | 'pixel';
}

// 3. Component as arrow function with typed props
export const FervoComponent: React.FC<FervoComponentProps> = ({
  title,
  onComplete,
  variant = 'fire'
}) => {
  // 4. State & hooks (useState, useEffect, useAuth)
  const { user, profile } = useAuth();
  const [loading, setLoading] = useState(false);

  useEffect(() => {
    // Effect logic
  }, []);

  // 5. Event handlers (prefix: handle*)
  const handleSubmit = async () => {
    setLoading(true);
    try {
      // Logic here
      onComplete();
    } catch (error) {
      console.error('Error:', error);
    } finally {
      setLoading(false);
    }
  };

  // 6. JSX return with TailwindCSS classes + FERVOai styling
  return (
    <div className="bg-black p-6 space-y-4 border-2 border-fervo-red">
      <h2 className="font-bebas text-3xl text-fervo-yellow uppercase tracking-wide">
        {title}
      </h2>
      <Button
        onClick={handleSubmit}
        disabled={loading}
        className="bg-fervo-red hover:bg-fervo-yellow text-black font-bold"
      >
        {loading ? 'FERVENDO...' : 'FERVER AGORA 🔥'}
      </Button>
    </div>
  );
};
```

### TypeScript Standards
- **Strict mode**: No implicit `any` - use `unknown` if truly unknown
- **Props**: Always define interfaces for component props
- **Return types**: Explicit return types on all functions
- **Imports**: Use absolute paths with `@/` alias when available

```typescript
// ✅ FERVOai way
interface Props {
  title: string;
  onComplete: () => void;
  heat?: 'low' | 'medium' | 'inferno';  // On-brand naming
}
export const Component: React.FC<Props> = ({ title, onComplete, heat = 'medium' }) => { ... }

// ❌ Boring and wrong
const Component = ({ title, onComplete }) => { ... }  // Missing types
```

---

## 🎮 Service Types & Credit Costs

| Service | Type String | Credits (std/high/premium) |
|---------|-------------|----------------------------|
| Content Generation | `content-generation` | 10/25/50 |
| Translation | `translation` | 8/20/40 |
| Data Analysis | `data-analysis` | 15/35/70 |
| Audio Transcription | `audio-transcription` | 5/12/25 |
| Code Generation | `code-generation` | 12/30/60 |
| Image Generation | `image-generation` | 20/45/90 |

---

## ⚡ Edge Function Pattern (Deno)

```typescript
// ALWAYS include CORS headers
const corsHeaders = {
  'Access-Control-Allow-Origin': '*',
  'Access-Control-Allow-Headers': 'authorization, x-client-info, apikey, content-type',
  'Access-Control-Allow-Methods': 'POST, GET, OPTIONS, PUT, DELETE, PATCH',
};

// Handle OPTIONS preflight
if (req.method === 'OPTIONS') {
  return new Response(null, { status: 200, headers: corsHeaders });
}

// ALWAYS verify JWT token before processing
const authHeader = req.headers.get('authorization');
if (!authHeader) {
  throw new Error('No authorization header - access denied 🔥');
}
const token = authHeader.replace('Bearer ', '');
// Validate via Supabase Auth API before processing
```

---

## 🗄️ Database Schema (Supabase PostgreSQL)

```sql
-- Core tables
profiles(id, email, credits, created_at, updated_at)
credit_transactions(id, user_id, amount, type, description, created_at)
ai_generations(id, user_id, service_type, credits_used, status, created_at)
generated_content(id, generation_id, content, metadata, created_at)
```

---

## 🛠️ Development Commands

```powershell
# In ai-smart-pro/ directory
pnpm install          # Install dependencies
pnpm dev              # Start dev server (localhost:5173)
pnpm build            # Production build
pnpm lint             # ESLint check

# Supabase CLI
supabase functions serve ai-service --env-file supabase/.env.local  # Local testing
supabase functions deploy ai-service                                 # Deploy
```

---

## 🚨 Common Pitfalls & Solutions

| Issue | Cause | Solution |
|-------|-------|----------|
| "Invalid or expired token" | Session expired | Call `supabase.auth.getSession()` and refresh |
| Infinite re-renders | Missing useEffect deps | Always include dependency array `[]` |
| CORS errors | Missing headers | Include `corsHeaders` in ALL Edge Function responses |
| Credit not updating | Missing refresh | Call `refreshProfile()` after service completion |
| Brand inconsistency | Wrong colors/fonts | Reference Brand Bible - use exact hex values |
| Boring UI | Forgetting FERVOai style | Add fire emojis 🔥, pixel borders, neon colors |

---

## 🔒 Security Rules

1. **Never expose API keys** in client code - use Edge Functions for OpenAI calls
2. **Always validate JWT** server-side in Edge Functions before processing
3. **Sanitize user input** - limit prompt length (5000 chars max)
4. **Check credit balance** before processing: `profile.credits >= estimatedCost`
5. **No PII in console.log** - never log emails, prompts, or results in production

---

## 📝 TODO Tracking

When discovering issues, log them in `ai-smart-pro/TODO.md`:

```markdown
### 🔥 [Issue Title]
- **File**: `path/to/file.tsx:line`
- **Category**: Security | Performance | Accessibility | Bug | Brand
- **Heat Level**: 🔥 (low) | 🔥🔥 (medium) | 🔥🔥🔥 (critical)
- **Cause**: Brief explanation
- **Fix**: Proposed solution
```

---

## 🎯 Quick Reference: Common Tasks

### Add new AI service
1. Add service config to `App.tsx` services array
2. Add type to `ServiceConfig` interface in `supabase.ts`
3. Update Edge Function `generateEnhancedPrompt()` and `getSystemPrompt()`

### Update credit costs
- Modify `aiService.calculatePrice()` in `supabase.ts`

### Add new component (FERVOai style)
1. Create in `src/components/` with PascalCase naming
2. Follow component structure pattern above
3. Use TailwindCSS + shadcn/ui components from `./ui/`
4. **Apply FERVOai colors**: `bg-fervo-red`, `text-fervo-yellow`, etc.
5. **Use Bebas Neue for headlines**: `font-bebas text-3xl uppercase`
6. **Add fire/pixel elements** where appropriate 🔥

### Create FERVOai-styled button
```tsx
<button className="
  bg-fervo-red
  hover:bg-fervo-yellow
  text-black
  font-bebas
  text-xl
  uppercase
  tracking-wider
  px-6 py-3
  border-2 border-white
  transition-all
  hover:shadow-[0_0_20px_rgba(255,45,45,0.5)]
">
  FERVER AGORA 🔥
</button>
```

### Create FERVOai card component
```tsx
<div className="
  bg-black
  border-2 border-fervo-cyan
  p-6
  relative
  before:absolute before:inset-0 before:bg-scanlines before:pointer-events-none
">
  <h3 className="font-bebas text-2xl text-fervo-green uppercase">
    TÍTULO AQUI
  </h3>
  <p className="font-inter text-fervo-white mt-2">
    Descrição do conteúdo...
  </p>
</div>
```

---

## 🌐 FervoVerse Domain Reference

Different domains = different "pocket realities" in the FERVOai multiverse:

### Core FERVOai Domains
| Domain | Purpose | Persona |
|--------|---------|---------|
| `fervoai.com.br` | Main brand hub | Corporate devil with flaming confidence |
| `fervoai.online` | Developer playground, APIs | Cyberpunk guardian of perpetual uptime |
| `fervoai.store` | Products marketplace | Dojo master of efficiency |
| `fervoai.shop` | Quick commerce | Neon ronin of productivity |
| `fervoai.tech` | (Coming soon) | TBD |

### FervoVerse Pocket Realities
| Domain | Purpose | Status |
|--------|---------|--------|
| `justonemore.beer` | Social challenge game — suspense, creativity, AI | 🟢 Live |
| `seekhelp.me` | Digital helping hand — support & empathy AI | 🟡 Validation |
| `sadbut.fun` | Internet's saddest, funniest AI — memes meet melancholy | 🟡 Data Sourcing |
| `anybodyseenmy.baby` | The Disappearance Game — AI life stories from another dimension | 🔵 Concept |
| `enterthe.store` | Vaporwave marketplace — stuff nobody else sells | 🔵 Design |
| `lookwithin.guru` | Personal growth powered by AI & curiosity | 🟡 Alpha |
| `bemine.baby` | Matchmaking? Maybe. FervoVerse connections, definitely | 🔵 Concept |
| `dynothegyno.me` | TBD | 🔵 Concept |

**Status Legend**: 🟢 Live | 🟡 In Development | 🔵 Concept/Planning

---

## 🤖 Custom Copilot Agents

This workspace includes 4 specialized custom Copilot agents for different engineering roles. All agents MUST follow FERVOai brand guidelines.

### Available Agents

| Agent | Specialty | Invocation |
|-------|-----------|------------|
| **Backend Architect** | Python, APIs, databases, microservices, Edge Functions | `@copilot /agent backend-architect` |
| **Frontend Craftsman** | React, TypeScript, accessibility, TailwindCSS, Storybook | `@copilot /agent frontend-craftsman` |
| **DevOps Guardian** | Kubernetes, Terraform, CI/CD, Supabase, observability | `@copilot /agent devops-guardian` |
| **QA Security Analyst** | Testing, OWASP, SAST/DAST, vulnerability assessment | `@copilot /agent qa-security-analyst` |

### Agent Configuration Files

Located in `.github/agents/`:
- `backend-architect.yaml`
- `frontend-craftsman.yaml`
- `devops-guardian.yaml`
- `qa-security-analyst.yaml`

### Agent Cross-Compatibility

All agents share:
- **FERVOai Brand Guidelines** — Visual identity, voice, tone (see sections above)
- **TODO.md Format** — Consistent issue tracking with 🔥 heat levels
- **Documentation Standards** — Same structure across all deliverables
- **Security-First Mindset** — OWASP Top 10, input validation, JWT handling

### Agent-Specific Guidelines

**Backend Architect** must:
- Follow Supabase Edge Function patterns (Deno runtime)
- Use CORS headers in all responses
- Implement credit system integration
- Document API contracts in OpenAPI format

**Frontend Craftsman** must:
- Apply FERVOai colors (`--fervo-red`, `--fervo-yellow`, etc.)
- Use Bebas Neue for headlines, Inter for body
- Ensure WCAG 2.1 AA compliance
- Add pixel/fire motifs where appropriate

**DevOps Guardian** must:
- Configure for Supabase + Vercel/MiniMax deployment
- Set up monitoring for credit system and AI service calls
- Implement rate limiting for Edge Functions
- Document rollback procedures

**QA Security Analyst** must:
- Test authentication flows (Supabase Auth)
- Verify credit deduction accuracy
- Scan for exposed API keys (OpenAI, Supabase)
- Validate input sanitization (5000 char limit on prompts)

### Reference Documentation

Full agent specifications: `Create at least four distinct Custom Copilot agent.md`

---

## 📚 Documentation Index

### Brand Resources (Root)
- `FERVOai Brand Bible — Unfiltered Edition.md` — Voice, tone, creative directives
- `fervoai_brand_documentation.md` — Complete brand & product specs
- `index.html` — Reference LP implementation

### Technical Docs (ai-smart-pro/)
- `README.md` — Project overview
- `DEVELOPMENT.md` — Development guide
- `AI-AGENT-QUICKREF.md` — Quick reference for AI agents
- `TODO.md` — Technical debt tracking
- `.github/copilot-instructions.md` — Detailed app-level instructions

### Architecture Docs (Root)
- `ARCHITECTURE.md` — System architecture
- `DOCUMENTATION_INDEX.md` — Full documentation index
- `SECURITY.md` — Security guidelines
- `PRODUCTION_CHECKLIST.md` — Deployment checklist

---

## 🔥 Final Reminders

1. **Every pixel matters** — FERVOai aesthetic is non-negotiable
2. **Brand voice is loud** — No boring corporate speak
3. **Colors are sacred** — Use exact hex values from palette
4. **Fire emoji is your friend** — 🔥 Use liberally but not annoyingly
5. **Dark mode is default** — Black backgrounds, neon accents
6. **Bebas Neue for headlines** — Always uppercase, always bold
7. **Inter for body** — Clean, readable, professional contrast

> "If it's not fun, it dies. If it's not pixelated, it burns. If it's not bold, it gets thrown out the window."
> — FERVOai Brand Bible

---

**Welcome to the FERVOai multiverse.** 🔥

*Last Updated: November 30, 2025*
