# Commit Convention

Recommended format:

```text
<type>(<scope>): <imperative summary> [<TASK-ID>]
```

Types:
- `feat`: new capability
- `fix`: bug fix
- `docs`: documentation
- `refactor`: behavior-preserving restructuring
- `test`: tests
- `build`: build/dependency tooling
- `ci`: CI configuration
- `chore`: maintenance

Rules:
- One commit should represent one coherent change.
- Avoid vague messages such as `update`, `fix stuff`, or `AI changes`.
- Do not include secrets, generated binaries, local settings, or user data.
- Explain breaking changes in the body and update contracts/migration docs.
- Do not amend or rewrite shared commits without approval.

Example:
`fix(ime): preserve user dictionary during repair [IME-014]`
