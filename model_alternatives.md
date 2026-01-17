# Model Alternatives for Multi-Agent System

**Version:** 1.0
**Date:** 2026-01-17
**Purpose:** Fallback model chains when primary model unavailable

---

## Quick Reference

| Agent | 1st Choice | 2nd Choice | 3rd Choice | 4th Choice |
|-------|------------|------------|------------|------------|
| Orchestrator | Claude Sonnet 4.5 | Gemini 3 Pro | ChatGPT 5.2 | Claude Opus 4.5 |
| Task Classifier | Claude Haiku 4.5 | ChatGPT 5 Nano | Gemini 2.5 Flash | Gemini 2.0 Flash Lite |
| Simple Planner | Claude Haiku 4.5 | Gemini 2.5 Flash | ChatGPT 5.1 Codex Mini | Gemini 3 Flash |
| Fast Implementer | ChatGPT 5.1 Codex Mini | Claude Haiku 4.5 | Gemini 2.5 Flash | ChatGPT 5.1 Codex |
| Strategic Planner | Claude Opus 4.5 | ChatGPT 5.2 | Gemini 3 Pro | Claude Sonnet 4.5 |
| Researcher | Gemini 3 Flash | Claude Sonnet 4.5 | Gemini 2.5 Pro | ChatGPT 5.2 |
| Architect | Claude Opus 4.5 | Gemini 3 Pro | ChatGPT 5.2 | Claude Sonnet 4.5 |
| Plan Reviewer | Claude Sonnet 4.5 | Gemini 3 Pro | ChatGPT 5.2 | Claude Opus 4.5 |
| Testing Specialist | ChatGPT 5.1 Codex | Claude Sonnet 4.5 | ChatGPT 5.2 Codex | Gemini 3 Pro |
| Senior Implementer | ChatGPT 5.2 Codex | Claude Sonnet 4.5 | ChatGPT 5.1 Codex Max | Gemini 3 Pro |
| Frontend Specialist | ChatGPT 5.1 Codex | Claude Sonnet 4.5 | ChatGPT 5.2 Codex | Gemini 3 Flash |
| Documentation Writer | Gemini 3 Flash | Claude Haiku 4.5 | Gemini 2.5 Flash | ChatGPT 5 Nano |
| Reviewer | Claude Sonnet 4.5 | ChatGPT 5.1 Codex | Gemini 3 Pro | ChatGPT 5.2 Codex |
| Security Advisor | Claude Opus 4.5 | ChatGPT 5.2 | Gemini 3 Pro | Claude Sonnet 4.5 |

---

## Detailed Fallback Chains

### 1. Orchestrator

| Priority | Model | Rationale | Trade-offs |
|----------|-------|-----------|------------|
| **1st** | Claude Sonnet 4.5 | Best reasoning + coordination balance | - |
| **2nd** | Gemini 3 Pro | Strong reasoning, good multi-step | Slightly less nuanced synthesis |
| **3rd** | ChatGPT 5.2 | Excellent reasoning | Higher cost than Sonnet |
| **4th** | Claude Opus 4.5 | Best reasoning available | Overkill cost for coordination |
| **5th** | Gemini 2.5 Pro | Good reasoning | Older generation |

**Never Use:** Haiku, Nano, Flash Lite - insufficient reasoning for routing

---

### 2. Task Classifier

| Priority | Model | Rationale | Trade-offs |
|----------|-------|-----------|------------|
| **1st** | Claude Haiku 4.5 | Fast, cheap, accurate classification | - |
| **2nd** | ChatGPT 5 Nano | Ultra-fast, cheapest | Slightly less accurate |
| **3rd** | Gemini 2.5 Flash | Fast, efficient | Different output formatting |
| **4th** | Gemini 2.0 Flash Lite | Fastest available | Basic classification only |
| **5th** | ChatGPT 5.1 Codex Mini | Fast, code-aware | Overkill for classification |

**Acceptable Upgrade:** Gemini 3 Flash if accuracy issues detected

---

### 3. Simple Planner

| Priority | Model | Rationale | Trade-offs |
|----------|-------|-----------|------------|
| **1st** | Claude Haiku 4.5 | Fast, clear plans | - |
| **2nd** | Gemini 2.5 Flash | Fast, structured output | Less nuanced |
| **3rd** | ChatGPT 5.1 Codex Mini | Code-aware planning | Slightly slower |
| **4th** | Gemini 3 Flash | More capable | Higher cost |
| **5th** | Claude Sonnet 4.5 | Excellent quality | Overkill for simple plans |

**Never Use:** Opus, 5.2 - massive overkill for 3-5 step plans

---

### 4. Fast Implementer

| Priority | Model | Rationale | Trade-offs |
|----------|-------|-----------|------------|
| **1st** | ChatGPT 5.1 Codex Mini | Fast code, efficient | - |
| **2nd** | Claude Haiku 4.5 | Fast, good code quality | Less code-specialized |
| **3rd** | Gemini 2.5 Flash | Fast, capable | Less code-focused |
| **4th** | ChatGPT 5.1 Codex | Better code quality | Higher cost |
| **5th** | Gemini 3 Flash | Strong overall | More than needed |

**Escalation Path:** If task too complex → escalate to Senior Implementer

---

### 5. Strategic Planner

| Priority | Model | Rationale | Trade-offs |
|----------|-------|-----------|------------|
| **1st** | Claude Opus 4.5 | Best deep reasoning | - |
| **2nd** | ChatGPT 5.2 | Excellent reasoning | Different style |
| **3rd** | Gemini 3 Pro | Strong planning | Less nuanced risk assessment |
| **4th** | Claude Sonnet 4.5 | Good reasoning | May miss edge cases |
| **5th** | Gemini 2.5 Pro | Capable | Older generation |

**Critical Warning:** Never use Tier 4 models - planning failures cascade

**Minimum Acceptable:** Claude Sonnet 4.5 / Gemini 3 Pro

---

### 6. Researcher

| Priority | Model | Rationale | Trade-offs |
|----------|-------|-----------|------------|
| **1st** | Gemini 3 Flash | Best search/synthesis | - |
| **2nd** | Claude Sonnet 4.5 | Strong synthesis | Higher cost |
| **3rd** | Gemini 2.5 Pro | Good research | Older generation |
| **4th** | ChatGPT 5.2 | Excellent | Overkill cost |
| **5th** | Gemini 3 Pro | Very capable | Higher cost than needed |

**Note:** Gemini models preferred for research due to search integration

---

### 7. Architect

| Priority | Model | Rationale | Trade-offs |
|----------|-------|-----------|------------|
| **1st** | Claude Opus 4.5 | Best architectural reasoning | - |
| **2nd** | Gemini 3 Pro | Strong system design | Less nuanced trade-offs |
| **3rd** | ChatGPT 5.2 | Excellent reasoning | Different architectural style |
| **4th** | Claude Sonnet 4.5 | Good architecture | May miss long-term implications |
| **5th** | Gemini 2.5 Pro | Capable | Older generation |

**Critical Warning:** Architectural decisions are hard to reverse

**Minimum Acceptable:** Gemini 3 Pro / Claude Sonnet 4.5

---

### 8. Plan Reviewer

| Priority | Model | Rationale | Trade-offs |
|----------|-------|-----------|------------|
| **1st** | Claude Sonnet 4.5 | Balanced analysis | - |
| **2nd** | Gemini 3 Pro | Strong evaluation | Different scoring style |
| **3rd** | ChatGPT 5.2 | Excellent analysis | Higher cost |
| **4th** | Claude Opus 4.5 | Best quality | Overkill for review |
| **5th** | ChatGPT 5.1 Codex | Good code understanding | Less strategic view |

**Acceptable Downgrade:** Gemini 3 Flash if cost critical (monitor quality)

---

### 9. Testing Specialist

| Priority | Model | Rationale | Trade-offs |
|----------|-------|-----------|------------|
| **1st** | ChatGPT 5.1 Codex | Strong test generation | - |
| **2nd** | Claude Sonnet 4.5 | Good test + strategy | Less code-specialized |
| **3rd** | ChatGPT 5.2 Codex | Best code quality | Higher cost |
| **4th** | Gemini 3 Pro | Capable | Less test-focused |
| **5th** | ChatGPT 5.1 Codex Max | High quality | Expensive for tests |

**Framework-Specific Notes:**
- Jest/Vitest: Any Codex model preferred
- Pytest: Claude models work well
- Go testing: Gemini models capable

---

### 10. Senior Implementer

| Priority | Model | Rationale | Trade-offs |
|----------|-------|-----------|------------|
| **1st** | ChatGPT 5.2 Codex | Best production code | - |
| **2nd** | Claude Sonnet 4.5 | Excellent code quality | Different patterns |
| **3rd** | ChatGPT 5.1 Codex Max | High quality | Slightly older |
| **4th** | Gemini 3 Pro | Strong code | Less code-specialized |
| **5th** | ChatGPT 5.1 Codex | Good code | Less sophisticated |

**Critical Warning:** This agent writes production code

**Minimum Acceptable:** ChatGPT 5.1 Codex / Claude Sonnet 4.5

---

### 11. Frontend/UI-UX Specialist

| Priority | Model | Rationale | Trade-offs |
|----------|-------|-----------|------------|
| **1st** | ChatGPT 5.1 Codex | Strong React/Vue/etc | - |
| **2nd** | Claude Sonnet 4.5 | Good frontend code | Less framework-specific |
| **3rd** | ChatGPT 5.2 Codex | Best code | Higher cost than needed |
| **4th** | Gemini 3 Flash | Fast, capable | Less UI-specialized |
| **5th** | ChatGPT 5.1 Codex Mini | Fast | May miss accessibility |

**Framework-Specific Notes:**
- React: ChatGPT Codex models preferred
- Vue: Claude models work well
- Angular: Any Tier 2+ model
- Svelte: Gemini 3 Flash capable

---

### 12. Documentation Writer

| Priority | Model | Rationale | Trade-offs |
|----------|-------|-----------|------------|
| **1st** | Gemini 3 Flash | Fast, good docs | - |
| **2nd** | Claude Haiku 4.5 | Fast, clear writing | Less technical depth |
| **3rd** | Gemini 2.5 Flash | Efficient | Older generation |
| **4th** | ChatGPT 5 Nano | Ultra-fast | Basic docs only |
| **5th** | Claude Sonnet 4.5 | Excellent quality | Overkill cost |

**Documentation-Specific Notes:**
- OpenAPI/Swagger: Any Flash model
- README: Haiku/Flash sufficient
- ADRs: Consider Sonnet for complex decisions
- JSDoc/docstrings: Any Tier 3+ model

---

### 13. Reviewer

| Priority | Model | Rationale | Trade-offs |
|----------|-------|-----------|------------|
| **1st** | Claude Sonnet 4.5 | Best code analysis balance | - |
| **2nd** | ChatGPT 5.1 Codex | Strong code understanding | Less strategic |
| **3rd** | Gemini 3 Pro | Good analysis | Different focus |
| **4th** | ChatGPT 5.2 Codex | Excellent code | Higher cost |
| **5th** | Claude Opus 4.5 | Best quality | Overkill for review |

**Note:** Must be capable of triggering Security Advisor appropriately

---

### 14. Security Advisor

| Priority | Model | Rationale | Trade-offs |
|----------|-------|-----------|------------|
| **1st** | Claude Opus 4.5 | Best security reasoning | - |
| **2nd** | ChatGPT 5.2 | Excellent security analysis | Different threat model style |
| **3rd** | Gemini 3 Pro | Strong analysis | Less OWASP-focused |
| **4th** | Claude Sonnet 4.5 | Good security | May miss subtle vulns |
| **5th** | ChatGPT 5.1 Codex Max | Code-aware security | Less threat modeling |

**Critical Warning:** Security failures are catastrophic

**Minimum Acceptable:** Gemini 3 Pro / Claude Sonnet 4.5

**Never Use:** Any Tier 4 model for security review

---

## Fallback Decision Logic

```
Primary Model Unavailable?
│
├── Check 2nd choice availability
│   ├── Available → Use 2nd choice
│   └── Unavailable → Continue to 3rd
│
├── Check 3rd choice availability
│   ├── Available → Use 3rd choice, log warning
│   └── Unavailable → Continue to 4th
│
├── Check 4th choice availability
│   ├── Available → Use 4th choice, log alert
│   └── Unavailable →
│       │
│       ├── Critical Agent (Planner/Architect/Security)?
│       │   └── HALT - Escalate to human
│       │
│       └── Non-Critical Agent?
│           └── Use any available Tier 2+ model
```

---

## Cross-Provider Compatibility Notes

### Claude → OpenAI Migration
| Claude Model | OpenAI Equivalent | Notes |
|--------------|-------------------|-------|
| Opus 4.5 | ChatGPT 5.2 | Similar reasoning depth |
| Sonnet 4.5 | ChatGPT 5.1 Codex Max | Good balance |
| Haiku 4.5 | ChatGPT 5 Nano | Fast/cheap tier |

### Claude → Gemini Migration
| Claude Model | Gemini Equivalent | Notes |
|--------------|-------------------|-------|
| Opus 4.5 | Gemini 3 Pro | Different style, capable |
| Sonnet 4.5 | Gemini 3 Pro | Very similar capability |
| Haiku 4.5 | Gemini 3 Flash | Fast tier equivalent |

### OpenAI → Gemini Migration
| OpenAI Model | Gemini Equivalent | Notes |
|--------------|-------------------|-------|
| ChatGPT 5.2 Codex | Gemini 3 Pro | Less code-specialized |
| ChatGPT 5.1 Codex | Gemini 3 Flash | Faster, less code focus |
| ChatGPT 5 Nano | Gemini 2.0 Flash Lite | Ultra-fast tier |

---

## Emergency Fallback Chains

### If All Anthropic Models Down
```
Orchestrator     → Gemini 3 Pro → ChatGPT 5.2
Strategic Planner → ChatGPT 5.2 → Gemini 3 Pro
Plan Reviewer    → Gemini 3 Pro → ChatGPT 5.2
Reviewer         → ChatGPT 5.1 Codex → Gemini 3 Pro
Security Advisor → ChatGPT 5.2 → Gemini 3 Pro
```

### If All OpenAI Models Down
```
Fast Implementer    → Claude Haiku 4.5 → Gemini 2.5 Flash
Senior Implementer  → Claude Sonnet 4.5 → Gemini 3 Pro
Frontend Specialist → Claude Sonnet 4.5 → Gemini 3 Flash
Testing Specialist  → Claude Sonnet 4.5 → Gemini 3 Pro
```

### If All Google Models Down
```
Researcher          → Claude Sonnet 4.5 → ChatGPT 5.2
Documentation Writer → Claude Haiku 4.5 → ChatGPT 5 Nano
```

---

## Model Health Monitoring

### Recommended Checks
1. **Latency:** Alert if >2x normal response time
2. **Error Rate:** Alert if >5% failures
3. **Quality Degradation:** Sample output quality checks
4. **Rate Limits:** Track remaining quota

### Automatic Failover Triggers
- API timeout (>30s for standard, >60s for Opus)
- 5xx errors (immediate failover)
- Rate limit hit (failover + backoff)
- Quality score drop >20% (alert + manual review)

---

## Cost Impact of Fallbacks

### Fallback Cost Multipliers (vs Primary)

| Agent | 2nd Choice | 3rd Choice | 4th Choice |
|-------|------------|------------|------------|
| Orchestrator | 1.0x | 1.5x | 3.0x |
| Task Classifier | 0.8x | 1.0x | 0.5x |
| Simple Planner | 1.0x | 1.2x | 1.5x |
| Fast Implementer | 1.0x | 1.0x | 2.0x |
| Strategic Planner | 1.2x | 0.8x | 0.5x |
| Researcher | 1.5x | 1.2x | 2.0x |
| Architect | 0.8x | 1.2x | 0.5x |
| Plan Reviewer | 1.0x | 1.5x | 3.0x |
| Testing Specialist | 1.5x | 2.0x | 1.0x |
| Senior Implementer | 0.8x | 1.5x | 0.8x |
| Frontend Specialist | 1.5x | 2.0x | 1.2x |
| Documentation Writer | 0.8x | 0.6x | 0.3x |
| Reviewer | 0.8x | 1.0x | 1.5x |
| Security Advisor | 1.2x | 0.8x | 0.5x |

---

*Document generated for OpenCode multi-agent failover configuration*
