# PROJECT-PROFILE.md

Fill this in once per new Nuvaris project. Every generalized agent reads
this file (plus CLAUDE.md) before touching anything. If a field doesn't
apply, write `none` — don't delete the line, agents key off presence/absence.

```yaml
project_slug: ""            # e.g. arciva, admita — used in @nova-{slug} tags
project_name: ""            # e.g. Nuvaris Arciva
author: "Nuvaris Cloud by MulTech"

# --- Stack ---
language: ""                # e.g. TypeScript, JavaScript
framework: ""                # e.g. Next.js 15, Fastify, Express (vanilla SPA)
monorepo: false              # true/false
package_manager: ""          # pnpm | npm
database: ""                 # e.g. Supabase PostgreSQL, Neon, JSON flat-file
orm: ""                      # e.g. Prisma, none
prisma_client_path: ""       # only if custom path, else "default"
id_convention: ""            # e.g. companyId, tenantId, none (single-tenant)

# --- Testing / Quality ---
test_runner: ""              # e.g. node:test + tsx --test, Jest, Vitest, none
lint_command: ""             # e.g. pnpm lint
build_command: ""            # e.g. pnpm --filter=web build
stress_tool: "k6"            # default unless overridden

# --- API conventions ---
error_format: ""             # e.g. RFC 7807 via sendProblem(), custom JSON
validation_status_code: ""   # e.g. 422

# --- Auth ---
auth_pattern: ""             # e.g. custom JWT, Supabase Auth

# --- Deploy ---
deploy_target: ""            # e.g. Hostinger, Vercel, Railway
deploy_method: ""            # e.g. manual zip upload, git push auto-deploy
staging_url_env: ".pentest.env"   # where PHANTOM/BOLT read staging URL

# --- Design source (drives IRIS/PIXEL conditional logic) ---
design_source: ""            # claude_design | html_reference | none

# --- Domain rules (drives MIRA-style conditional add-ons) ---
domain_ruleset: "domain-rules.md"           # e.g. none, or path to domain-rules.md

# --- Compliance ---
compliance: ""                # e.g. DPA RA 10173, none

# --- Forbidden legacy names (never appear in output) ---
forbidden_names:
  - "MulTech RDMS"
  - "MTS"
  - "Kodanda Cloud"
  - "KDC"
```

## Non-negotiables (apply regardless of profile values — do not override)

- `ANTHROPIC_API_KEY` and `api.anthropic.com` = Category C. No agent touches these, ever.
- All other Anthropic/Claude brand references in shipped product code get
  abstracted via `lib/ai/client.ts` + `lib/ai/config.ts`
  (`NUVARIS_AI_MODEL` / `NUVARIS_AI_MODEL_HEAVY`).
- AI-provider rebranding (SDK imports, env vars, model strings) requires
  Luis's sign-off. Never auto-applied by WREN or ASH.
- PHANTOM never attacks a URL that isn't explicitly the staging URL in
  `.pentest.env`. If that file is missing or points at anything resembling
  production, PHANTOM stops and escalates.
