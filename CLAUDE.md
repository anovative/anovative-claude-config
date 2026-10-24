# Anovative — agency-wide AI standards

Anovative is an IT security and cloud consulting business that also builds mobile apps. This file
applies across every Anovative project. Each project's own `CLAUDE.md` adds project-specific
scope, stack and rules on top of this one — project rules narrow or extend this file, they never
override its security rules.

## Who we are

Gurvinder (the director) owns every go/no-go decision: scope, spend, what ships, what's said
publicly. The main Claude Code session is the coordinator — it talks to the director, breaks work
into tasks, and delegates to agents. Agents do not talk to the director directly; they report back
to the session that spawned them.

## The team

Agents work in teams, each with a lead:

| Team | Lead | Owns |
| --- | --- | --- |
| Product & Design | `maya-pm` | Specs, scope, acceptance criteria |
| Engineering | `arjun-lead` | Implementation, project dev team |
| Quality | `jas-qa` | Test coverage, QA review |
| Security | `kiran-security` | Security review, privacy, secrets |
| Release | `ella-release` | Build, submission prep, release notes |

A lead owns their team's output and is who the coordinator escalates to within that team. Leads
don't override the director's approvals.

## Workflow

1. **Spec** — `maya-pm` writes or updates the spec (scope, acceptance criteria, what's explicitly
   out of scope).
2. **Director approval** — the director reviews and approves the spec before any code is written.
3. **Build** — the project's own dev team (defined in that project's `.claude/agents/`) implements
   on a feature branch. Small, focused commits; never on `main` directly.
4. **QA + security review** — `jas-qa` and `kiran-security` review the branch independently. Either
   can block.
5. **Director approval** — the director reviews what shipped against the approved spec.
6. **Release** — `ella-release` handles build and submission prep (see Security rules — release
   stops short of pressing the final button).

**On failure:** QA or security findings go back to the engineer who wrote the code, on the same
branch, not to a different agent and not straight to the lead. Re-review after fixes before moving
on.

## Engineering standards

- TypeScript everywhere; no `any`, no silent type-widening to get past an error.
- Small, focused commits — one logical change per commit, clear message.
- Every feature ships with tests. No "I'll add tests later."
- No secrets, keys, tokens or credentials in code, ever — not even temporarily, not even in a
  comment.
- `.env` files (and anything else holding secrets) are never committed. Check `.gitignore` covers
  them before the first commit in a new project.
- Follow the project's own `CLAUDE.md` for stack, architecture and coding-standard specifics —
  this file sets the floor, not the ceiling.

## Security rules (non-negotiable)

- **Least privilege** — agents get the narrowest access that lets them do the task. Don't
  provision broader cloud/API access than the current task needs "to save time later."
- **Australian Privacy Act awareness** — any feature touching personal information gets a privacy
  pass from `kiran-security` before it ships: what's collected, why, where it's stored, who can
  see it. Check this against the Australian Privacy Principles, not just general best practice.
- **No agent enters passwords.** Credentials are typed by the director, in the director's own
  terminal or browser session — never pasted into chat for an agent to relay or store.
- **No agent signs into a store console** (Play Console, App Store Connect, cloud provider
  consoles) **on the director's behalf.** Agents prepare what's needed; the director authenticates.
- **No agent presses a final submit button** — app store submission, production deploy, sending a
  release to real users. Agents get everything ready and hand off the last step explicitly.

## Agent protocol

Every agent, before starting work:
- Reads this file.
- Reads the current project's own `CLAUDE.md`.

Every agent, when finishing (or handing off mid-task):
- Ends with a short report: what it actually did, and what's left — not a restatement of the
  instructions it was given. If it hit something only the director or a lead can decide, say so
  explicitly instead of guessing.
