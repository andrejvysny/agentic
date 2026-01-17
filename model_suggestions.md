# Model Suggestions for Multi-Agent System

**Version:** 1.0
**Analysis Date:** 2026-01-17
**Optimization Goals:** Performance + Cost Efficiency

---

## Executive Summary

This document maps each agent to the most appropriate model based on:
- **Task Complexity:** Reasoning depth required
- **Criticality:** Impact of errors on system
- **Speed Requirements:** Latency sensitivity
- **Cost Efficiency:** Use cheaper models where complexity allows

### Model Tier Classification

| Tier | Models | Use Case | Relative Cost |
|------|--------|----------|---------------|
| **Tier 1 (Premium)** | Claude Opus 4.5, ChatGPT 5.2 | Critical reasoning, security, architecture | $$$$$ |
| **Tier 2 (Standard)** | Claude Sonnet 4.5, ChatGPT 5.2 Codex, Gemini 3 Pro | Complex coding, planning, review | $$$ |
| **Tier 3 (Efficient)** | ChatGPT 5.1 Codex, Gemini 3 Flash | Standard coding, research, docs | $$ |
| **Tier 4 (Fast/Cheap)** | Claude Haiku 4.5, ChatGPT 5.1 Codex Mini, ChatGPT 5 Nano, Gemini 2.5 Flash | Classification, simple tasks | $ |

---

## Agent Model Assignments

### 1. Orchestrator

| Attribute | Value |
|-----------|-------|
| **Primary Model** | Claude Sonnet 4.5 |
| **Fallback Model** | Gemini 3 Pro |
| **Tier** | 2 (Standard) |

**Rationale:**
- Needs strong reasoning for routing decisions and context synthesis
- Does NOT write code - coordination only
- Must handle complex multi-agent orchestration
- Sonnet provides excellent reasoning without Opus cost
- NOT Haiku: routing errors cascade through entire system

**Key Requirements:**
- ✅ Multi-step reasoning
- ✅ Context management across agents
- ✅ Decision tree execution
- ❌ Code generation (delegates)
- ❌ Deep security analysis (delegates)

---

### 2. Task Classifier

| Attribute | Value |
|-----------|-------|
| **Primary Model** | Claude Haiku 4.5 |
| **Fallback Model** | ChatGPT 5 Nano |
| **Tier** | 4 (Fast/Cheap) |

**Rationale:**
- Binary classification task (SIMPLE vs COMPLEX)
- Well-defined criteria - pattern matching
- Speed critical - runs on every request
- Low token usage - short input/output
- Errors recoverable (conservative default to COMPLEX)

**Key Requirements:**
- ✅ Fast inference (<500ms)
- ✅ JSON output generation
- ✅ Pattern recognition
- ❌ Deep reasoning
- ❌ Code understanding beyond surface

**Cost Savings:** ~95% vs using Opus for classification

---

### 3. Simple Planner

| Attribute | Value |
|-----------|-------|
| **Primary Model** | Claude Haiku 4.5 |
| **Fallback Model** | Gemini 2.5 Flash |
| **Tier** | 4 (Fast/Cheap) |

**Rationale:**
- Creates 3-5 step plans only
- Tasks already classified as SIMPLE
- Limited scope: 1-3 files, <50 lines
- Speed prioritized over depth
- Can escalate if complexity discovered

**Key Requirements:**
- ✅ Fast planning
- ✅ Clear step generation
- ✅ File path identification
- ❌ Research capabilities
- ❌ Architectural thinking

---

### 4. Fast Implementer

| Attribute | Value |
|-----------|-------|
| **Primary Model** | ChatGPT 5.1 Codex Mini |
| **Fallback Model** | Claude Haiku 4.5 |
| **Tier** | 4 (Fast/Cheap) |

**Rationale:**
- Executes simple, well-defined plans
- Code changes are straightforward
- Self-review is basic (syntax, imports)
- Speed matters more than depth
- Codex models optimized for code

**Key Requirements:**
- ✅ Code generation (simple)
- ✅ Follow plan precisely
- ✅ Basic syntax validation
- ❌ Complex refactoring
- ❌ Architectural decisions

---

### 5. Strategic Planner

| Attribute | Value |
|-----------|-------|
| **Primary Model** | Claude Opus 4.5 |
| **Fallback Model** | ChatGPT 5.2 |
| **Tier** | 1 (Premium) |

**Rationale:**
- **Critical agent** - plans drive entire implementation
- Must think deeply about dependencies, risks, edge cases
- Spawns and coordinates Researcher + Architect
- Handles unclear requirements
- Poor planning = expensive rework

**Key Requirements:**
- ✅ Deep multi-step reasoning
- ✅ Risk assessment
- ✅ Codebase analysis
- ✅ Sub-agent coordination
- ✅ Long-context handling

**Why Premium:** Planning errors cost 10-50x to fix in implementation

---

### 6. Researcher

| Attribute | Value |
|-----------|-------|
| **Primary Model** | Gemini 3 Flash |
| **Fallback Model** | Claude Sonnet 4.5 |
| **Tier** | 3 (Efficient) |

**Rationale:**
- Information gathering and synthesis
- Web search and documentation parsing
- Does NOT make implementation decisions
- Gemini excels at search/retrieval tasks
- Speed matters for research loops

**Key Requirements:**
- ✅ Web search integration
- ✅ Source synthesis
- ✅ Documentation parsing
- ✅ Comparison generation
- ❌ Implementation decisions
- ❌ Code generation

**Why Not Premium:** Research is synthesis, not deep reasoning

---

### 7. Architect

| Attribute | Value |
|-----------|-------|
| **Primary Model** | Claude Opus 4.5 |
| **Fallback Model** | Gemini 3 Pro |
| **Tier** | 1 (Premium) |

**Rationale:**
- **Critical agent** - architectural decisions are hard to reverse
- Must evaluate complex trade-offs
- Long-term system implications
- Considers scalability, maintainability, cost
- Mistakes cause technical debt

**Key Requirements:**
- ✅ Trade-off analysis
- ✅ System design thinking
- ✅ Long-term implications
- ✅ Multi-constraint optimization
- ❌ Code implementation

**Why Premium:** Architectural mistakes = months of rework

---

### 8. Plan Reviewer

| Attribute | Value |
|-----------|-------|
| **Primary Model** | Claude Sonnet 4.5 |
| **Fallback Model** | Gemini 3 Pro |
| **Tier** | 2 (Standard) |

**Rationale:**
- Quality gate before implementation
- Scores plans on 6 dimensions (100-point scale)
- Must catch planning errors early
- Analytical but not creative
- Sonnet sufficient for evaluation

**Key Requirements:**
- ✅ Structured evaluation
- ✅ Gap identification
- ✅ Risk assessment
- ✅ Scoring consistency
- ❌ Plan creation (review only)

---

### 9. Testing Specialist

| Attribute | Value |
|-----------|-------|
| **Primary Model** | ChatGPT 5.1 Codex |
| **Fallback Model** | Claude Sonnet 4.5 |
| **Tier** | 3 (Efficient) |

**Rationale:**
- Test strategy + test code generation
- Specialized testing knowledge
- Codex models excel at test patterns
- Not security-critical (Security Advisor handles that)
- Balance of code + strategy

**Key Requirements:**
- ✅ Test code generation
- ✅ Coverage analysis
- ✅ Edge case identification
- ✅ Framework knowledge (Jest, Pytest, etc.)
- ❌ Security testing (delegates)

---

### 10. Senior Implementer

| Attribute | Value |
|-----------|-------|
| **Primary Model** | ChatGPT 5.2 Codex |
| **Fallback Model** | Claude Sonnet 4.5 |
| **Tier** | 2 (Standard) |

**Rationale:**
- **Production-quality code** is the output
- Must handle complex, multi-file implementations
- Error handling, edge cases, tests
- Codex models optimized for code generation
- 5.2 Codex > 5.1 for complex code

**Key Requirements:**
- ✅ Production-quality code
- ✅ Multi-file coordination
- ✅ Test writing
- ✅ Error handling patterns
- ✅ Following established patterns

**Why 5.2 Codex:** Best-in-class for complex code generation

---

### 11. Frontend/UI-UX Specialist

| Attribute | Value |
|-----------|-------|
| **Primary Model** | ChatGPT 5.1 Codex |
| **Fallback Model** | Claude Sonnet 4.5 |
| **Tier** | 3 (Efficient) |

**Rationale:**
- Specialized frontend code (React, Vue, etc.)
- CSS/styling patterns well-established
- Accessibility requirements are rule-based
- Component patterns are repeatable
- 5.1 Codex sufficient for frontend

**Key Requirements:**
- ✅ React/Vue/Angular/Svelte
- ✅ CSS/Tailwind/styled-components
- ✅ Accessibility (WCAG patterns)
- ✅ Responsive design
- ❌ Backend/database

**Why Not 5.2:** Frontend patterns more structured, 5.1 sufficient

---

### 12. Documentation Writer

| Attribute | Value |
|-----------|-------|
| **Primary Model** | Gemini 3 Flash |
| **Fallback Model** | Claude Haiku 4.5 |
| **Tier** | 4 (Fast/Cheap) |

**Rationale:**
- Documentation is structured writing
- OpenAPI specs are template-based
- README patterns well-defined
- Runs in parallel - speed matters
- Does NOT require deep reasoning

**Key Requirements:**
- ✅ Technical writing
- ✅ Template adherence
- ✅ Markdown/YAML generation
- ✅ Fast parallel execution
- ❌ Code generation
- ❌ Architectural decisions

**Cost Savings:** ~80% vs using Sonnet for docs

---

### 13. Reviewer

| Attribute | Value |
|-----------|-------|
| **Primary Model** | Claude Sonnet 4.5 |
| **Fallback Model** | ChatGPT 5.1 Codex |
| **Tier** | 2 (Standard) |

**Rationale:**
- Code quality gate - catches bugs pre-delivery
- Must identify anti-patterns, edge cases
- Spawns Security Advisor for sensitive code
- Needs code understanding + reasoning
- Sonnet balances code + analysis

**Key Requirements:**
- ✅ Code analysis
- ✅ Bug identification
- ✅ Pattern recognition
- ✅ Test coverage assessment
- ✅ Security trigger detection

---

### 14. Security Advisor

| Attribute | Value |
|-----------|-------|
| **Primary Model** | Claude Opus 4.5 |
| **Fallback Model** | ChatGPT 5.2 |
| **Tier** | 1 (Premium) |

**Rationale:**
- **Security is non-negotiable**
- Must find vulnerabilities humans miss
- OWASP Top 10, threat modeling, attack vectors
- Mistakes = data breaches, compliance failures
- Worth premium cost for security assurance

**Key Requirements:**
- ✅ OWASP knowledge
- ✅ Threat modeling
- ✅ Vulnerability identification
- ✅ Attack vector analysis
- ✅ Remediation guidance

**Why Premium:** Security failures are catastrophic

---

## Summary Table

| Agent | Primary Model | Tier | Rationale |
|-------|---------------|------|-----------|
| Orchestrator | Claude Sonnet 4.5 | 2 | Routing + coordination |
| Task Classifier | Claude Haiku 4.5 | 4 | Fast binary classification |
| Simple Planner | Claude Haiku 4.5 | 4 | Quick 3-5 step plans |
| Fast Implementer | ChatGPT 5.1 Codex Mini | 4 | Simple code execution |
| **Strategic Planner** | **Claude Opus 4.5** | **1** | **Critical planning** |
| Researcher | Gemini 3 Flash | 3 | Search + synthesis |
| **Architect** | **Claude Opus 4.5** | **1** | **Critical decisions** |
| Plan Reviewer | Claude Sonnet 4.5 | 2 | Quality evaluation |
| Testing Specialist | ChatGPT 5.1 Codex | 3 | Test code + strategy |
| Senior Implementer | ChatGPT 5.2 Codex | 2 | Production code |
| Frontend Specialist | ChatGPT 5.1 Codex | 3 | Frontend code |
| Documentation Writer | Gemini 3 Flash | 4 | Fast docs generation |
| Reviewer | Claude Sonnet 4.5 | 2 | Code quality gate |
| **Security Advisor** | **Claude Opus 4.5** | **1** | **Security critical** |

---

## Cost Optimization Analysis

### Quick Path Cost (per request)
| Agent | Model | Est. Cost |
|-------|-------|-----------|
| Task Classifier | Haiku | $0.002 |
| Simple Planner | Haiku | $0.008 |
| Fast Implementer | 5.1 Mini | $0.015 |
| **Total Quick Path** | | **~$0.025** |

### Deep Path Cost (per request, minimal)
| Agent | Model | Est. Cost |
|-------|-------|-----------|
| Task Classifier | Haiku | $0.002 |
| Strategic Planner | Opus | $0.80 |
| Plan Reviewer | Sonnet | $0.15 |
| Documentation Writer | Flash | $0.05 |
| Senior Implementer | 5.2 Codex | $0.60 |
| Reviewer | Sonnet | $0.20 |
| **Total Deep Path (min)** | | **~$1.80** |

### Deep Path Cost (per request, full)
| Agent | Model | Est. Cost |
|-------|-------|-----------|
| All above | | $1.80 |
| + Researcher | Flash | $0.10 |
| + Architect | Opus | $0.50 |
| + Testing Specialist | 5.1 Codex | $0.25 |
| + Security Advisor | Opus | $0.70 |
| **Total Deep Path (full)** | | **~$3.35** |

### Savings vs All-Premium
- Using all Opus/5.2: ~$8-12 per Deep Path request
- Optimized selection: ~$1.80-3.35 per Deep Path request
- **Savings: 60-75%**

---

## Model Selection Decision Tree

```
Is task SIMPLE?
├── YES → Haiku/Nano tier for all agents
└── NO →
    Is agent making irreversible decisions?
    ├── YES (Planner, Architect, Security) → Opus tier
    └── NO →
        Is agent writing production code?
        ├── YES → Codex tier (5.2 for complex, 5.1 for standard)
        └── NO →
            Is agent doing research/docs?
            ├── YES → Flash tier
            └── NO → Sonnet tier (default)
```

---

## Recommendations

### High Priority
1. **Never downgrade** Strategic Planner, Architect, Security Advisor
2. Use Haiku/Nano for Task Classifier - runs on every request
3. Use Codex variants for implementation agents

### Medium Priority
4. Consider Gemini 3 Flash for Research - excellent at synthesis
5. Documentation Writer can use cheapest tier - template-based

### Future Optimization
6. Monitor accuracy per agent, upgrade if error rates high
7. Consider fine-tuned models for high-volume agents (Classifier)
8. A/B test Sonnet vs Pro for Reviewer role

---

## Notes on Model Characteristics

### Claude Family
- **Opus 4.5:** Best reasoning, highest cost, use for critical decisions
- **Sonnet 4.5:** Excellent balance, good for code + analysis
- **Haiku 4.5:** Fast, cheap, good for classification/simple tasks

### OpenAI Codex Family
- **5.2 Codex:** Best code generation, use for Senior Implementer
- **5.1 Codex:** Strong code, more efficient, good for frontend/tests
- **5.1 Codex Mini:** Fast code, use for simple implementations
- **5 Nano:** Ultra-fast, classification only

### Google Gemini Family
- **3 Pro:** Strong reasoning, alternative to Sonnet
- **3 Flash:** Fast, excellent for search/synthesis
- **2.5 Flash:** Efficient, good for docs/simple tasks

---

*Document generated for OpenCode multi-agent architecture optimization*
