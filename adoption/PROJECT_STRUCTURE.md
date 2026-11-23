# Recommended Project Documentation Structure

```text
repository/
├── AGENTS.md
├── opencode.jsonc
├── .opencode/
│   └── agents/
│       ├── architect.md
│       ├── developer.md
│       ├── qa.md
│       └── reviewer.md
├── docs/
│   ├── product/
│   │   ├── PRD.md
│   │   └── DECISIONS.md
│   ├── architecture/
│   │   ├── ARCHITECTURE.md
│   │   └── API_CONTRACTS.md
│   ├── development/
│   │   ├── DEVELOPMENT_RULES.md
│   │   ├── TASKS.md
│   │   ├── HANDOFF_PROTOCOL.md
│   │   ├── tasks/
│   │   ├── handoffs/
│   │   └── reviews/
│   ├── testing/
│   │   ├── TEST_STRATEGY.md
│   │   ├── ACCEPTANCE_CRITERIA.md
│   │   └── qa-reports/
│   └── releases/
│       └── CHANGELOG.md
└── src/
```

Adapt this structure to the repository. Do not move existing documentation solely to match this tree if doing so creates churn or breaks links.
