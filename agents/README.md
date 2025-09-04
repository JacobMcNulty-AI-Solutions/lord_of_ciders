# Single Agent Internal Capabilities and Modules

This directory outlines the internal capabilities and modules of the single agent, organized by type and purpose. Each module describes specific functionalities, internal tool access policies, and naming conventions that guide the agent's internal task execution.

## Directory Structure

```
.claude/agents/
├── README.md                    # This file
├── _templates/                  # Internal module templates
│   ├── base-agent.yaml
│   └── agent-types.md
├── development/                 # Development modules
│   ├── backend/
│   ├── frontend/
│   ├── fullstack/
│   └── api/
├── testing/                     # Testing modules
│   ├── unit/
│   ├── integration/
│   ├── e2e/
│   └── performance/
├── architecture/                # Architecture modules
│   ├── system-design/
│   ├── database/
│   ├── cloud/
│   └── security/
├── devops/                      # DevOps modules
│   ├── ci-cd/
│   ├── infrastructure/
│   ├── monitoring/
│   └── deployment/
├── documentation/               # Documentation modules
│   ├── api-docs/
│   ├── user-guides/
│   ├── technical/
│   └── readme/
├── analysis/                    # Analysis modules
│   ├── code-review/
│   ├── performance/
│   ├── security/
│   └── refactoring/
├── data/                        # Data modules
│   ├── etl/
│   ├── analytics/
│   ├── ml/
│   └── visualization/
└── specialized/                 # Specialized modules
    ├── mobile/
    ├── embedded/
    ├── blockchain/
    └── ai-ml/
```

## Naming Conventions

Internal capability documentation files follow this naming pattern:
`[type]-[specialization]-[capability].md`

Examples:
- `dev-backend-api.md`
- `test-unit-jest.md`
- `arch-cloud-aws.md`
- `docs-api-openapi.md`

## Internal Capability Activation Triggers

The single agent internally activates specific capabilities based on:
1. **Keywords in user request**: "test", "deploy", "document", "review"
2. **File patterns**: `*.test.js` → activates testing capability, `*.tf` → activates infrastructure capability
3. **Task complexity**: Multi-step tasks trigger internal orchestration capabilities
4. **Domain detection**: Database queries → activates data capability, API endpoints → activates backend capability

## Internal Tool Access Policies

The single agent's internal capabilities have specific tool access policies:
- **Development capabilities**: Full file system access, code execution
- **Testing capabilities**: Test runners, coverage tools, limited write access
- **Architecture capabilities**: Read-only access, diagram generation
- **Documentation capabilities**: Markdown tools, read access, limited write to docs/
- **DevOps capabilities**: Infrastructure tools, deployment scripts, environment access
- **Analysis capabilities**: Read-only access, static analysis tools