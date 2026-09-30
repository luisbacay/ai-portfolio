# NOVA: a deterministic multi-agent build pipeline for Claude Code

NOVA is a set of 15 agent prompt files for Claude Code: one orchestrator and 14 specialist agents. Each specialist has one job (planning, backend, frontend, design fidelity, tests, code review, functional QA, security audit, cleanup, stress testing, pentesting, integration, final gate). The orchestrator runs them in a fixed order with explicit pass or fail gates.

It is a deterministic, conditionally branching workflow. It is not an autonomous system. A human approves every phase change that matters, and a blocker stops the run until it is resolved.

Product specific versions of these prompts were adapted for Nuvaris Admiva and Nuvaris Talenta. The files in this repo are the generalized, stack agnostic version.

## Pipeline

```
NOVA -> REX -> COLE -> VEGA -> IRIS -> ZARA -> LEON -> PIPER
     -> AXIS -> WREN -> BOLT -> PHANTOM -> ASH -> ECHO -> JUDGE
```

| Agent | Role |
|---|---|
| NOVA | Orchestrator: reads the project profile, sequences phases, enforces gates |
| REX | Plans a feature |
| COLE | Backend implementation |
| VEGA | Frontend implementation |
| IRIS | Design fidelity check (skips itself when there is no design source) |
| ZARA | Tests |
| LEON | Code review |
| PIPER | Functional QA |
| AXIS | Security audit |
| WREN | Safe cleanup |
| BOLT | Stress testing |
| PHANTOM | Pentest, staging only, hard locked |
| ASH | Post remediation cleanup |
| ECHO | Integration and smoke tests |
| JUDGE | Final gate |

## Design principles

- **Reasoning before verdict.** Review, QA, security and gate agents must write out their reasoning before they state a pass or fail.
- **Evidence required at the gate.** JUDGE requires cited evidence for each finding it accepts or rejects.
- **Ambiguity escalation.** Implementation and review agents include calibration examples and escalate instead of guessing.
- **Hard safety locks.** PHANTOM refuses to run against anything that looks like production. Secrets such as `ANTHROPIC_API_KEY` are off limits to every agent.
- **Profile driven.** Agents read `PROJECT-PROFILE.md` instead of hardcoding a stack, so the same 15 files adapt to a new project.

## What is in this repo

- `agent-prompts/nova-orchestrator/`: the 15 prompt files
- `PROJECT-PROFILE.md`: the schema you fill in once per project
- `domain-rules.md`: platform feature rules referenced from the profile (if present)

## What is not in this repo

- No runtime code. These are prompts and a profile schema, not a framework.
- No filled in project profile. The files carry no project identity by design.
- No automated eval harness yet. Verifying agent behavior is done by running the pipeline on a real project and reading the reports.
- Project specific add-on agents. Those stay hand built per project.

## Using it

1. Copy the prompt folder into `[your repo]/.claude/agents/`.
2. Fill in `PROJECT-PROFILE.md` for your stack.
3. Replace `{{project_slug}}` with your slug across all 15 files.
4. Create `.loop.env`, `.stress.env` and `.pentest.env` for the project and add all three to `.gitignore`.
5. Start with `@nova-{{project_slug}}`.

Commands: `@nova-{{project_slug}}` to start or resume, `resume` after fixing a blocker, `status` to check the phase, `abort` to stop the run, `override [N]` to skip a phase (owner only). Each specialist can also be called on its own, for example `@axis-{{project_slug}}` for a security audit.

## Status and limits

Do not install this cold on a live project. Run it on a throwaway branch first and read every gate report. Prompt behavior depends on the model version, so re-test after model updates.

## Changelog

- 2026-07-14: Added reasoning before verdict to REX, ZARA, PIPER, AXIS, ECHO, PHANTOM and JUDGE. Added ambiguity escalation and calibration examples to COLE, VEGA, IRIS, LEON, WREN and BOLT. Added completion criteria to ASH.
- 2026-07-14: Added domain-rules.md, referenced through domain_ruleset in PROJECT-PROFILE.md.
