# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Multi-agent software engineering system for OpenCode. Uses **Adaptive Hybrid Architecture** with dual-path routing:
- **Quick Path**: Simple tasks (1-3 files, <50 lines) → Task Classifier → Simple Planner → Fast Implementer
- **Deep Path**: Complex tasks → Task Classifier → Strategic Planner → [Researcher/Architect] → Plan Reviewer → Human Approval → Senior Implementer → Reviewer → [Security Advisor]

16 specialized agents coordinated by Orchestrator. All agents are prompt-based definitions in `prompts/`.

## Architecture

```
User Request
    ↓
Orchestrator → Task Classifier
    ↓
SIMPLE ──→ Simple Planner → Fast Implementer → Done
COMPLEX ──→ Strategic Planner → Plan Reviewer → Human Approval → Senior Implementer → Reviewer → Done
```

### Agent Categories

**Core Orchestration**: Orchestrator, Task Classifier
**Quick Path**: Simple Planner, Fast Implementer
**Deep Path Core**: Strategic Planner, Researcher, Architect, Plan Reviewer, Senior Implementer, Reviewer
**Specialists**: Security Advisor, Testing Specialist, Frontend/UI-UX Specialist, Documentation Writer

### Classification Criteria

**SIMPLE**: 1-3 files, <50 lines, well-understood patterns, no external deps, no architectural decisions, no security-sensitive changes
**COMPLEX**: 4+ files OR >50 lines, research needed, external deps, architectural decisions, schema/API changes, security-sensitive, vague requirements

Conservative rule: "when in doubt → COMPLEX"

## File Structure

```
├── opencode.json              # Agent config, tool permissions, MCP integrations
├── prompts/                   # System prompts for all 16 agents (~240 KB total)
│   ├── orchestrator.md
│   ├── classifier.md
│   ├── simple-planner.md
│   ├── fast-implementer.md
│   ├── strategic-planner.md
│   ├── researcher.md
│   ├── architect.md
│   ├── senior-implementer.md
│   ├── reviewer.md
│   ├── security-advisor.md
│   ├── testing-specialist.md
│   ├── frontend-ui-ux-specialist.md
│   ├── document-writer.md
│   └── plan-reviewer.md
├── model_suggestions.md       # Model assignment rationale & tiers
├── model_alternatives.md      # Fallback model chains
└── diagram.mmd                # Mermaid flow diagram
```

## Configuration (opencode.json)

**Models**: Default `opencode/glm-4.7-free`, small model `opencode/gpt-5-nano`

**MCP Integrations**:
- `context7`: Remote MCP for advanced context (requires `CONTEXT7_API_KEY` env var)
- `gh_grep`: GitHub grep for code search

**Agent Modes**:
- `explore`: Read-only, no write/bash/task
- `build`: Edit allowed, bash asks, limited task delegation
- `plan`: Read-only, webfetch enabled, delegates to explore/architect/reviewer/librarian
- `orchestrator`: Full access, delegates to all sub-agents
- `architect`, `coder`, `librarian`, `reviewer`: Subagents with specific tool access

## Model Tiers

| Tier | Models | Use Cases |
|------|--------|-----------|
| 1 (Premium) | Claude Opus 4.5 | Strategic Planner, Architect, Security Advisor |
| 2 (Standard) | Claude Sonnet 4.5, GPT 5.2 | Orchestrator, Reviewer, Senior Implementer |
| 3 (Efficient) | GPT 5.1 Codex, Gemini 3 Flash | Researcher, Testing/Frontend Specialists |
| 4 (Fast/Cheap) | Claude Haiku 4.5, GPT 5 Nano | Task Classifier, Simple Planner |

## Quality Gates

1. **Task Classification**: Binary SIMPLE/COMPLEX routing
2. **Plan Review**: Score 0-100 (≥80 approved, 60-79 conditional, <60 rejected)
3. **Human Approval**: Required for Deep Path before implementation
4. **Code Review**: Priority order - Correctness → Security → Performance → Maintainability
5. **Security Review**: On-demand for security-sensitive code (OWASP Top 10)

## Key Design Principles

- Separation of concerns: Orchestrator coordinates, Planners plan, Implementers code, Reviewers review
- Parallel execution: Strategic Planner + Researcher can run in parallel; Senior Implementer + Documentation Writer can run in parallel
- Tool access control: Each agent has explicit permission model for read/write/edit/bash/task
- Conservative classification: Prefer Deep Path over misclassified Quick Path
