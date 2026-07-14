# NOVA-Generalized — v0.1 Draft

**Status: UNVERIFIED DRAFT.** Built from conversation history, not audited
against your actual Talenta/Admiva/DentaChart/Haula agent files (no
filesystem access to D:\Projects from this chat). Before trusting this on
Arciva or Admita, run it through Claude Code and have it diff these
against your real working agents — especially COLE, AXIS, and PHANTOM,
which are most likely to have stack-specific logic these drafts missed.

---

## What's here

15 core agents, generalized to read `PROJECT-PROFILE.md` instead of
hardcoding stack details:

```
NOVA -> REX -> COLE -> VEGA -> IRIS -> ZARA -> LEON -> PIPER
     -> AXIS -> WREN -> BOLT -> PHANTOM -> ASH -> ECHO -> JUDGE
```

Plus `PROJECT-PROFILE.md` — the schema every new project fills in once.

## What's NOT here

- Project-specific add-on agents (like PIXEL, MIRA) — those stay
  hand-built per project, per the pattern already established
- Any filled-in profile — this repo has zero project names in it, by
  design. You fill in `PROJECT-PROFILE.md` fresh for whatever you're
  installing this on
- Verification that these drafts match your live agent behavior exactly

## Installing on any project

This set has no project identity baked in. Same 15 files, same
`PROJECT-PROFILE.md` schema, regardless of what you're pointing it at —
that's the entire point of generalizing it.

1. Copy this whole folder into `[repo]/.claude/agents/`
2. Fill in `PROJECT-PROFILE.md` for that project's actual stack
3. Find-and-replace `{{project_slug}}` -> your chosen slug across all 15 files
4. Create `.loop.env`, `.stress.env`, `.pentest.env` for that project —
   add all three to `.gitignore`
5. Add any project-specific add-on agents (if needed) alongside these 15
6. Run `@nova-{{project_slug}}` — NOVA reads the profile and adapts

Which live project(s) this actually gets stamped onto — and in what
order — is a separate decision for a separate conversation. This folder
stays generic until you make that call.

## Prompt sheet (once installed and stamped)

```
@nova-{{project_slug}}                — start or resume the full pipeline
@nova-{{project_slug}} resume         — after fixing a blocker
@nova-{{project_slug}} status         — check current phase
@nova-{{project_slug}} abort          — kill the run
@nova-{{project_slug}} override [N]   — skip a phase (Luis only)

@rex-{{project_slug}}      — plan a feature
@cole-{{project_slug}}     — backend only
@vega-{{project_slug}}     — frontend only
@iris-{{project_slug}}     — design fidelity (skips itself if no design source)
@zara-{{project_slug}}     — tests
@leon-{{project_slug}}     — code review
@piper-{{project_slug}}    — functional QA
@axis-{{project_slug}}     — security audit
@wren-{{project_slug}}     — safe cleanup
@bolt-{{project_slug}}     — stress test
@phantom-{{project_slug}}  — pentest (staging only, hard-locked)
@ash-{{project_slug}}      — post-remediation cleanup
@echo-{{project_slug}}     — integration + smoke test
@judge-{{project_slug}}    — final gate
```

## Non-negotiables baked into every agent (see PROJECT-PROFILE.md)

- `ANTHROPIC_API_KEY` / `api.anthropic.com` = Category C, never touched
- AI-provider rebranding = sign-off required, never auto-applied
- PHANTOM staging-locked, hard stop on anything resembling production
- Forbidden legacy names never appear in generated output

## Recommended next step

Don't install this cold on any live project. Run the audit-and-validate
prompt in Claude Code first — point it at these 15 files plus your real
hand-built agent sets (whichever have working equivalents), and have it
flag every place a draft here missed a stack-specific nuance your
hand-built agents actually handle correctly.
## Changelog

- 2026-07-14: Added reasoning-before-verdict requirements to REX, ZARA,
  PIPER, AXIS, ECHO, PHANTOM, JUDGE. Added ambiguity escalation and
  calibration examples to COLE, VEGA, IRIS, LEON, WREN, BOLT. Added
  completion criteria to ASH.

- 2026-07-14: Added domain-rules.md covering Activity Log, Session Log,
  Report a Bug, and SUPERADMIN. Referenced via domain_ruleset in
  PROJECT-PROFILE.md.