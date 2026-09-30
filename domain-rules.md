# domain-rules.md — MulTech / Nuvaris Cloud Platform Features
Referenced by `domain_ruleset` in PROJECT-PROFILE.md for any project
that includes these platform-level features.

## Activity Log
- Tracks user actions: login, create, edit, delete at minimum, expand
  per-project as needed
- Logged per user AND per tenant
- Tenants are fully isolated, one tenant's activity log is never
  visible to or queryable by another tenant
- SUPERADMIN can view activity logs across all tenants, regular
  tenant admins only see their own tenant's log

## Session Log
- Rolling buffer, continuously tracks the last 30 minutes of user
  activity in the background
- Discards anything older than 30 minutes automatically
- Only persisted permanently when Report a Bug is triggered, at which
  point the current 30 minute buffer is attached to that bug report
- Never persisted or exposed otherwise, this is not a general audit
  trail, it exists only to support bug reports

## Report a Bug
- Button visible to all users, bottom right corner, all pages
- Captures on submit: current Session Log buffer, current page/route,
  user info, screenshot if feasible
- Available to all roles, not SUPERADMIN only
- Submitted reports route to MulTech, not visible to other tenants

## SUPERADMIN
- MulTech-level role, not scoped to a single tenant
- Access includes: System Settings, Tenant Settings, Tenant Branding,
  Module toggles (enable/disable per-tenant modules), cross-tenant
  Activity Log visibility
- Distinct from a regular tenant Admin, tenant Admin manages their own
  school/org only, SUPERADMIN manages the platform across all tenants
- Every Nuvaris project with multi-tenancy inherits this role
  distinction, REX and COLE must never conflate SUPERADMIN with a
  tenant-level admin role when planning or building auth logic