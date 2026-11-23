# Git Branching Strategy for AI Collaboration

## 1. Branch principles
- Protected integration/release branches are not used for direct development.
- One task or tightly coupled change set per feature branch.
- Branch names include task ID.
- Parallel agents use separate worktrees or clones.
- PR is the normal integration boundary.

## 2. Branch naming
- `feature/<TASK-ID>-short-description`
- `fix/<TASK-ID>-short-description`
- `docs/<TASK-ID>-short-description`
- `chore/<TASK-ID>-short-description`
- `release/<version>` (release owner only)

## 3. Worktree workflow
Example commands (adapt paths and branch names after checking repository state):

```bash
git status --short --branch
git worktree add ../worktrees/IME-001 -b feature/IME-001-theme
```

Never create a worktree over an existing directory. Check branch and worktree state first.

## 4. Integration
1. Developer completes implementation and handoff.
2. Reviewer inspects base-to-head diff.
3. QA verifies acceptance criteria.
4. CI required checks pass.
5. Project owner or authorized integrator approves merge.
6. Delete task branch only after merge and confirmation that no work is lost.

## 5. Prohibited without explicit approval
- `git push --force`
- `git reset --hard`
- `git clean -fd` or broader destructive cleanup
- rewriting shared history
- deleting remote branches
- direct push to protected branches
- merging or releasing

## 6. Conflict management
- Do not resolve semantic conflicts by choosing one side blindly.
- Identify the conflicting files and behavior.
- Consult module contracts and decision records.
- Ask the project owner when behavior or data semantics are ambiguous.
- Run affected tests after resolution.
