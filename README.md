# OpenCode Multi-Agent System

Adaptive Hybrid Architecture for software engineering with 16 specialized agents.

## Architecture

```
User Request
    ↓
Orchestrator → Task Classifier
    ↓
SIMPLE ──→ Simple Planner → Fast Implementer → Done
COMPLEX ──→ Strategic Planner → Plan Reviewer → Human Approval → Senior Implementer → Reviewer → Done
```

**Quick Path**: Simple tasks (1-3 files, <50 lines)
**Deep Path**: Complex tasks (4+ files, research needed, architectural decisions)

### Agent Overview

| Agent | Role | Model Tier |
|-------|------|------------|
| **Orchestrator** | Central coordinator, routing, approvals | Standard |
| **Task Classifier** | Binary SIMPLE/COMPLEX classification | Fast |
| **Simple Planner** | 3-5 step plans for quick tasks | Fast |
| **Fast Implementer** | Execute simple plans | Fast |
| **Strategic Planner** | 5-20 step plans, spawns sub-agents | Premium |
| **Researcher** | Web search, docs, best practices | Efficient |
| **Architect** | Design decisions, trade-offs | Premium |
| **Plan Reviewer** | Score plans 0-100 before approval | Standard |
| **Senior Implementer** | Production-quality code | Standard |
| **Frontend Specialist** | React/Vue/Angular, accessibility | Efficient |
| **Document Writer** | READMEs, API docs, ADRs | Fast |
| **Reviewer** | Code quality, spawns Security | Standard |
| **Security Advisor** | OWASP Top 10, threat modeling | Premium |
| **Testing Specialist** | Unit/integration/E2E tests | Efficient |

---

## Quick Start

### 1. Set Environment Variables

```bash
export CONTEXT7_API_KEY="your-api-key"
```

### 2. Configure OpenCode

The system uses `opencode.json` for all configuration. Key settings:

```json
{
  "model": "opencode/claude-sonnet-4-5",
  "small_model": "opencode/claude-haiku-4-5",
  "default_agent": "orchestrator"
}
```

---

## Configuration Reference

### Global Settings

| Setting | Default | Description |
|---------|---------|-------------|
| `model` | `opencode/claude-sonnet-4-5` | Default model for agents |
| `small_model` | `opencode/claude-haiku-4-5` | Fast/cheap model for simple tasks |
| `default_agent` | `orchestrator` | Entry point agent |
| `plugin` | `opencode-antigravity-auth@1.0.0` | Authentication plugin |

### MCP Servers

| Server | URL | Purpose |
|--------|-----|---------|
| `context7` | `https://mcp.context7.com/mcp` | Advanced context retrieval |
| `gh_grep` | `https://mcp.grep.app` | GitHub code search |

Both configured with `timeout: 10000` (10s).

### Agent Configuration Options

Each agent supports:

| Option | Type | Description |
|--------|------|-------------|
| `model` | string | Model identifier |
| `temperature` | 0.0-1.0 | Creativity vs determinism |
| `maxSteps` | int | Max agentic turns before stop |
| `reasoningEffort` | `low\|high` | Reasoning depth |
| `thinking` | object | Extended thinking config (Claude) |
| `tools` | object | Tool permissions |
| `permission` | object | Bash/task delegation rules |

### Temperature Settings

| Agent Type | Temperature | Rationale |
|------------|-------------|-----------|
| Classifiers | 0.1 | Deterministic routing |
| Planners | 0.2 | Structured, consistent plans |
| Implementers | 0.3 | Some creativity for code |
| Researcher | 0.5 | Exploratory search |

### MaxSteps Limits

| Agent | maxSteps | Rationale |
|-------|----------|-----------|
| Classifier | 5 | Quick binary decision |
| Simple Planner | 10 | Short planning cycle |
| Fast Implementer | 10 | Limited scope |
| Plan Reviewer | 15 | Evaluation only |
| Researcher | 20 | Search iterations |
| Document Writer | 20 | Writing cycles |
| Reviewer | 20 | Review passes |
| Security Advisor | 25 | Thorough analysis |
| Testing Specialist | 25 | Test creation |
| Strategic Planner | 30 | Deep planning |
| Architect | 30 | Design iterations |
| Senior Implementer | 30 | Complex code |
| Frontend Specialist | 30 | UI implementation |
| Orchestrator | 50 | Multi-agent coordination |

### Extended Thinking (Claude)

Critical agents get 16K thinking budget:

```json
{
  "thinking": {
    "type": "enabled",
    "budgetTokens": 16000
  }
}
```

Enabled for: `strategic-planner`, `architect`, `security-advisor`

### Bash Permission Patterns

Auto-allowed read-only commands:

```
git status/log/diff/branch
ls, cat, head, tail, wc
npm/yarn/pnpm test
npm run test/lint/build
pytest, go test, cargo test
```

All other bash commands require explicit approval.

### Tool Access Matrix

| Agent | grep/glob | todo | question | lsp | bash |
|-------|-----------|------|----------|-----|------|
| Orchestrator | ✓ | ✓ | ✓ | - | - |
| Classifier | ✓ | - | - | - | - |
| Simple Planner | ✓ | - | - | - | - |
| Strategic Planner | ✓ | ✓ | ✓ | - | - |
| Researcher | ✓ | - | - | - | - |
| Architect | ✓ | - | - | - | - |
| Plan Reviewer | ✓ | - | - | - | - |
| Senior Implementer | - | ✓ | - | ✓ | ask |
| Frontend Specialist | - | ✓ | - | ✓ | ask |
| Reviewer | ✓ | - | ✓ | ✓ | ask |
| Security Advisor | ✓ | - | - | - | ask |
| Testing Specialist | - | ✓ | - | ✓ | ask |

---

## Model Configuration

### Recommended Models

| Agent | Primary | Fallback | Tier |
|-------|---------|----------|------|
| Orchestrator | Claude Sonnet 4.5 | Gemini 3 Pro | 2 |
| Task Classifier | Claude Haiku 4.5 | GPT 5 Nano | 4 |
| Simple Planner | Claude Haiku 4.5 | Gemini 2.5 Flash | 4 |
| Fast Implementer | GPT 5.1 Codex Mini | Claude Haiku 4.5 | 4 |
| **Strategic Planner** | **Claude Opus 4.5** | GPT 5.2 | **1** |
| Researcher | Gemini 3 Flash | Claude Sonnet 4.5 | 3 |
| **Architect** | **Claude Opus 4.5** | Gemini 3 Pro | **1** |
| Plan Reviewer | Claude Sonnet 4.5 | Gemini 3 Pro | 2 |
| Senior Implementer | GPT 5.2 Codex | Claude Sonnet 4.5 | 2 |
| Frontend Specialist | GPT 5.1 Codex | Claude Sonnet 4.5 | 3 |
| Document Writer | Gemini 3 Flash | Claude Haiku 4.5 | 4 |
| Reviewer | Claude Sonnet 4.5 | GPT 5.1 Codex | 2 |
| **Security Advisor** | **Claude Opus 4.5** | GPT 5.2 | **1** |
| Testing Specialist | GPT 5.1 Codex | Claude Sonnet 4.5 | 3 |

### Model Tiers

| Tier | Models | Cost | Use Cases |
|------|--------|------|-----------|
| 1 Premium | Opus 4.5, GPT 5.2 | $$$$$ | Critical decisions, security |
| 2 Standard | Sonnet 4.5, GPT 5.2 Codex | $$$ | Complex coding, review |
| 3 Efficient | GPT 5.1 Codex, Gemini 3 Flash | $$ | Research, tests, frontend |
| 4 Fast | Haiku 4.5, GPT 5 Nano | $ | Classification, simple tasks |

### Complete Fallback Chains

| Agent | 1st | 2nd | 3rd | 4th |
|-------|-----|-----|-----|-----|
| Orchestrator | Claude Sonnet 4.5 | Gemini 3 Pro | GPT 5.2 | Claude Opus 4.5 |
| Classifier | Claude Haiku 4.5 | GPT 5 Nano | Gemini 2.5 Flash | Gemini 2.0 Flash Lite |
| Simple Planner | Claude Haiku 4.5 | Gemini 2.5 Flash | GPT 5.1 Codex Mini | Gemini 3 Flash |
| Fast Implementer | GPT 5.1 Codex Mini | Claude Haiku 4.5 | Gemini 2.5 Flash | GPT 5.1 Codex |
| Strategic Planner | Claude Opus 4.5 | GPT 5.2 | Gemini 3 Pro | Claude Sonnet 4.5 |
| Researcher | Gemini 3 Flash | Claude Sonnet 4.5 | Gemini 2.5 Pro | GPT 5.2 |
| Architect | Claude Opus 4.5 | Gemini 3 Pro | GPT 5.2 | Claude Sonnet 4.5 |
| Plan Reviewer | Claude Sonnet 4.5 | Gemini 3 Pro | GPT 5.2 | Claude Opus 4.5 |
| Senior Implementer | GPT 5.2 Codex | Claude Sonnet 4.5 | GPT 5.1 Codex Max | Gemini 3 Pro |
| Frontend Specialist | GPT 5.1 Codex | Claude Sonnet 4.5 | GPT 5.2 Codex | Gemini 3 Flash |
| Document Writer | Gemini 3 Flash | Claude Haiku 4.5 | Gemini 2.5 Flash | GPT 5 Nano |
| Reviewer | Claude Sonnet 4.5 | GPT 5.1 Codex | Gemini 3 Pro | GPT 5.2 Codex |
| Security Advisor | Claude Opus 4.5 | GPT 5.2 | Gemini 3 Pro | Claude Sonnet 4.5 |
| Testing Specialist | GPT 5.1 Codex | Claude Sonnet 4.5 | GPT 5.2 Codex | Gemini 3 Pro |

### Model Selection Decision Tree

```
Is task SIMPLE?
├── YES → Haiku/Nano tier
└── NO →
    Is agent making irreversible decisions?
    ├── YES (Planner, Architect, Security) → Opus tier
    └── NO →
        Is agent writing production code?
        ├── YES → Codex tier (5.2 complex, 5.1 standard)
        └── NO →
            Is agent doing research/docs?
            ├── YES → Flash tier
            └── NO → Sonnet tier
```

### Available Model IDs

**OpenCode Prefix:**
```
opencode/claude-opus-4-5
opencode/claude-sonnet-4-5
opencode/claude-haiku-4-5
opencode/gpt-5.2-codex
opencode/gpt-5.1-codex
opencode/gpt-5.1-codex-mini
opencode/gpt-5-nano
opencode/gemini-3-pro
opencode/gemini-3-flash
```

**Direct Provider:**
```
google/gemini-2.5-flash
google/gemini-2.5-pro
openai/gpt-5.2-codex
openai/gpt-5.1-codex-max
```

---

## Cost Optimization

### Quick Path (per request)

| Agent | Model | Est. Cost |
|-------|-------|-----------|
| Task Classifier | Haiku | $0.002 |
| Simple Planner | Haiku | $0.008 |
| Fast Implementer | 5.1 Mini | $0.015 |
| **Total** | | **~$0.025** |

### Deep Path Minimal (per request)

| Agent | Model | Est. Cost |
|-------|-------|-----------|
| Task Classifier | Haiku | $0.002 |
| Strategic Planner | Opus | $0.80 |
| Plan Reviewer | Sonnet | $0.15 |
| Senior Implementer | 5.2 Codex | $0.60 |
| Reviewer | Sonnet | $0.20 |
| **Total** | | **~$1.80** |

### Deep Path Full (per request)

| Agent | Model | Est. Cost |
|-------|-------|-----------|
| All above | | $1.80 |
| + Researcher | Flash | $0.10 |
| + Architect | Opus | $0.50 |
| + Testing Specialist | 5.1 Codex | $0.25 |
| + Security Advisor | Opus | $0.70 |
| **Total** | | **~$3.35** |

**Savings vs All-Premium:** 60-75% (~$8-12 → ~$1.80-3.35)

---

## Classification Criteria

### SIMPLE (Quick Path)

- 1-3 files affected
- <50 lines changed
- Well-understood patterns
- No external dependencies
- No architectural decisions
- No security-sensitive changes

### COMPLEX (Deep Path)

- 4+ files OR >50 lines
- Research needed
- External dependencies
- Architectural decisions
- Schema/API changes
- Security-sensitive code
- Vague requirements

**Rule:** When in doubt → COMPLEX

---

## Quality Gates

1. **Task Classification**: Binary SIMPLE/COMPLEX routing
2. **Plan Review**: Score 0-100 (≥80 approved, 60-79 conditional, <60 rejected)
3. **Human Approval**: Required for Deep Path before implementation
4. **Code Review**: Correctness → Security → Performance → Maintainability
5. **Security Review**: On-demand for OWASP Top 10 concerns

---

## File Structure

```
├── opencode.json              # Agent config, permissions, MCP
├── ENV_VARS.md                # Environment variable documentation
├── prompts/                   # System prompts (14 agents)
│   ├── orchestrator.md
│   ├── classifier.md
│   ├── simple-planner.md
│   ├── fast-implementer.md
│   ├── strategic-planner.md
│   ├── researcher.md
│   ├── architect.md
│   ├── plan-reviewer.md
│   ├── senior-implementer.md
│   ├── frontend-ui-ux-specialist.md
│   ├── document-writer.md
│   ├── reviewer.md
│   ├── security-advisor.md
│   └── testing-specialist.md
├── model_suggestions.md       # Model assignment rationale
├── model_alternatives.md      # Fallback chains
├── available_models.md        # All available model IDs
└── diagram.mmd                # Mermaid flow diagram
```

---

## Environment Variables

### Required

| Variable | Description |
|----------|-------------|
| `CONTEXT7_API_KEY` | API key for context7 MCP server |

### Optional

| Variable | Default | Description |
|----------|---------|-------------|
| `OPENCODE_MODEL` | `opencode/claude-sonnet-4-5` | Override default model |
| `OPENCODE_SMALL_MODEL` | `opencode/claude-haiku-4-5` | Override small model |
| `OPENCODE_DEBUG` | `false` | Enable debug logging |

---

## Emergency Fallbacks

### If All Anthropic Down

```
Orchestrator     → Gemini 3 Pro → GPT 5.2
Strategic Planner → GPT 5.2 → Gemini 3 Pro
Security Advisor → GPT 5.2 → Gemini 3 Pro
```

### If All OpenAI Down

```
Fast Implementer    → Claude Haiku 4.5 → Gemini 2.5 Flash
Senior Implementer  → Claude Sonnet 4.5 → Gemini 3 Pro
Frontend Specialist → Claude Sonnet 4.5 → Gemini 3 Flash
```

### If All Google Down

```
Researcher          → Claude Sonnet 4.5 → GPT 5.2
Documentation Writer → Claude Haiku 4.5 → GPT 5 Nano
```

---

## Best Practices

1. **Start Simple**: Implement Orchestrator + Classifier + Quick Path first
2. **Add Deep Path Incrementally**: Strategic Planner → sub-agents as needed
3. **Monitor Classification**: Track Quick/Deep routing accuracy
4. **Tune Thresholds**: Adjust complexity thresholds based on performance
5. **Log Everything**: Track agent spawning, decisions, outcomes
6. **Never Downgrade**: Keep premium models for Planner, Architect, Security

### Critical Warnings

- Never use Tier 4 for Strategic Planner - planning failures cascade
- Never use Tier 4 for Security Advisor - security failures are catastrophic
- Architectural decisions are hard to reverse - keep Architect on premium

---

## Customization

These prompts are templates. Customize for:

- **Tech Stack**: Add specific frameworks, libraries
- **Code Style**: Add project-specific style guides
- **Security**: Add compliance standards (PCI DSS, HIPAA)
- **Testing**: Customize coverage requirements
- **Docs**: Add project-specific documentation standards

---

**Version:** 2.0
**Architecture:** Adaptive Hybrid (16 Agents)
**License:** Internal Use
