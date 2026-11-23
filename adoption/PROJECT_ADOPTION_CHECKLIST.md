# Project Adoption Checklist

Use this checklist when applying the standard to an existing repository.

## A. Safe inspection first
- [ ] Record current branch, HEAD, and working-tree status.
- [ ] Identify existing `AGENTS.md`, OpenCode config, agent definitions, skills, and commands.
- [ ] Identify existing PRD, architecture, decision log, task tracker, test strategy, and CI.
- [ ] Identify branch protections and current Git workflow.
- [ ] Identify generated, ignored, sensitive, or license-restricted directories.
- [ ] Do not overwrite existing files or uncommitted work.

## B. Map existing documentation
| Existing file | Standard role | Keep / merge / create |
|---|---|---|
| | PRD | |
| | DECISIONS | |
| | ARCHITECTURE | |
| | TASKS | |
| | DEVELOPMENT_RULES | |
| | TEST_STRATEGY | |

## C. Configure OpenCode
- [ ] Merge governance rules into existing `AGENTS.md`.
- [ ] Merge `instructions` and Agent definitions into existing OpenCode config.
- [ ] Install Agent Markdown files without replacing existing custom Agents.
- [ ] Validate config against installed OpenCode version.
- [ ] Test each Agent's permissions with harmless sample tasks.
- [ ] Confirm Reviewer cannot edit files.
- [ ] Confirm high-risk Git operations require approval or are denied.

## D. Configure Git platform
- [ ] Protect integration/release branches.
- [ ] Require CI checks.
- [ ] Require PR review where supported.
- [ ] Restrict force-push and branch deletion.
- [ ] Protect release credentials and signing keys.
- [ ] Set least-privilege tokens.

## E. Pilot
- [ ] Select one low-risk task.
- [ ] Create task card.
- [ ] Run Developer → Reviewer → QA → final decision.
- [ ] Archive handoff, review, and QA reports.
- [ ] Record friction and revise the standard.

## F. Approval
- Project owner:
- Date:
- Standard version:
- Exceptions:
