# Test Cases Index

Test cases are organized by domain. Each file contains:
- User prompt (copy-paste ready)
- Module(s) to activate
- Must-include checklist (for grading)
- Full example output (quality bar)

## Files The below test case is for system design however could be expanded to any prompt engineering

| File | Domain | Modules Covered | Cases |
|------|--------|-----------------|-------|
| `system-design-test-cases.md` | System Design | A, B, D, E, H + combos | 5 |

## Test Case IDs

### System Design (`system-design-test-cases.md`)
- **TC-01** — Adversarial: URL shortener at scale (Module A + E)
- **TC-02** — First Principles: Microservices vs. monolith (Module B + D)
- **TC-03** — Risk Analysis: Redis caching for a financial dashboard (Module E + A)
- **TC-04** — Tradeoff Analysis: SQL vs NoSQL for a social feed (Module D + B)
- **TC-05** — Combined: Full review of a notification service design doc (Module A + C + H)

## Scoring

A response **passes** a test case if:
- Hits ≥ 80% of must-include checklist items
- Does not hallucinate technical facts (e.g., invents AWS limits that don't exist)
- Follows the module's output format
- Is specific — no vague critiques like "this might not scale"

A response **fails** if:
- Misses the highest-severity issue for that case
- Gives a verdict inconsistent with the evidence
- Produces generic advice not grounded in the specifics of the prompt

## Adding New Test Cases

To add a test case, pick a domain file (or create a new one) and follow this template:

```markdown
## TC-XX: [Module Name] — [Short description]

**Module(s):** [Letter(s)]

**User prompt:**
> [Exact prompt to send to Claude]

**Must include:**
- [ ] [Specific thing the response must contain]
- [ ] ...

**Example output:**
[Full high-quality response]
```

Domains to add next: prompt-engineering, business-strategy.
