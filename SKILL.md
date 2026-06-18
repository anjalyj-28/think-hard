---
name: think-hard
description: >
  A modular reasoning framework for improving the quality of decisions, documents, and thinking
  made with frontier LLMs — for an individual person. Use this skill whenever the user wants
  higher-quality, more rigorous reasoning applied to any task. Covers: adversarial thinking,
  first principles reasoning, fact verification, tradeoff analysis, risk analysis, resume reviews,
  interview coaching, document editing, and prompt engineering improvement.

  Trigger phrases include: "think hard", "think harder", "use think-hard", "red team this",
  "first principles", "steelman", "poke holes", "what could go wrong", "verify this",
  "what are the tradeoffs", "review my resume", "help me prep for interviews", "improve this
  prompt", "edit this doc", "stress test", "challenge my assumptions", "what am I missing".

  Also trigger proactively when the user presents a plan, document, or argument with evident
  blind spots, overconfidence, or where higher reasoning quality would materially improve the output.
---

# Think Hard — Modular Reasoning Framework

A general-purpose reasoning upgrade for frontier LLMs. Think Hard is a collection of reasoning
modules that can be applied individually or in combination to improve the quality of decisions,
documents, and thinking for an individual person.

---

## Module Selection

Read the user's request and activate the most relevant module(s). Modules can be combined.

| Module | When to use |
|--------|-------------|
| [A] Adversarial Thinking | Stress-testing plans, arguments, proposals |
| [B] First Principles | Breaking problems down to ground truth |
| [C] Fact Verification | Checking claims, sources, accuracy |
| [D] Tradeoff Analysis | Comparing options with competing values |
| [E] Risk Analysis | Identifying and sizing what could go wrong |
| [F] Resume Review | Evaluating and improving resumes |
| [G] Interview Coaching | Preparing answers, drilling questions |
| [H] Document Editing | Sharpening clarity, structure, persuasion |
| [I] Prompt Engineering | Improving prompts for better LLM outputs |

---

## Module A — Adversarial Thinking

Systematically attack an idea from multiple hostile angles, then synthesize a verdict.

**Steps:**
1. **Restate** the idea charitably in 1-3 sentences
2. **Steelman** — articulate the strongest version before attacking it
3. **Apply lenses** (use what's relevant, skip what isn't):
   - *Assumptions* — which hidden assumptions could be wrong?
   - *Second-order effects* — what unintended consequences follow?
   - *Adversarial actors* — who benefits from this failing, and how would they exploit it?
   - *Base rates* — what does the historical success rate say?
   - *Missing alternatives* — what better path is being ignored?
   - *Execution risk* — where is the single most likely point of failure?
   - *Internal contradictions* — does any part of the argument contradict another?
   - *Epistemic weaknesses* — is this motivated reasoning or confirmation bias?
4. **Rank** top 3 objections by severity × likelihood
5. **Verdict** — fundamentally flawed / conditionally viable / solid with gaps
6. **Crux** — the one thing that matters most to address

See `references/adversarial-lenses.md` for extended sub-questions per lens.

**Output format:**
```
## Steelman
[1-2 sentences]

## Adversarial Analysis
### [Lens]: [One-line summary]
[2-4 sentences, specific and sharp]

## Top 3 Threats
1. [Threat] — [Why it matters]
2. ...
3. ...

## Verdict
[Fundamentally flawed / Conditionally viable / Solid with gaps]
**Crux**: [One thing to address]
**Recommendation**: [What to do]
```

---

## Module B — First Principles Reasoning

Break a problem down to its foundational truths, then rebuild from the ground up.

**Steps:**
1. **Identify the question** being asked beneath the stated question
2. **Strip assumptions** — list everything being taken for granted
3. **Find bedrock** — what is undeniably true about this domain? (physics, incentives, math, human nature)
4. **Rebuild** — starting only from bedrock, what solution or conclusion follows?
5. **Compare** — how does the first-principles answer differ from conventional wisdom?
6. **Flag** where the reasoning is uncertain vs. where it's solid

**Useful questions:**
- What problem is this actually solving?
- What would a physicist / economist / anthropologist say about this?
- If I had to explain this to a smart 12-year-old, what's the core mechanic?
- What constraints are artificial vs. real?

---

## Module C — Fact Verification

Rigorously examine the factual claims in a piece of reasoning or content.

**Steps:**
1. **Extract all factual claims** (stats, attributions, causal claims, dates, names)
2. **Classify each claim** by verifiability:
   - *Checkable* — can be verified with a source
   - *Contextual* — true in some conditions, false in others
   - *Unfalsifiable* — too vague to verify
3. **Flag high-risk claims** — those that are load-bearing AND likely wrong or unsourced
4. **Note epistemic status** — what's established vs. contested vs. speculative
5. **Recommend** corrections, qualifications, or source lookups

**Watch for:**
- Statistics cited without sources
- Correlation presented as causation
- Anecdotes generalized to populations
- Expert consensus vs. fringe positions conflated
- Outdated data presented as current

---

## Module D — Tradeoff Analysis

Surface and structure the real tradeoffs in a decision with competing values.

**Steps:**
1. **Name the decision** and the options being weighed
2. **Identify the values in tension** (e.g., speed vs. quality, autonomy vs. alignment, cost vs. safety)
3. **Map consequences** of each option across each value dimension
4. **Find the crux** — which tradeoff dominates? What's the one thing the user is implicitly optimizing for?
5. **Pressure-test the weights** — are the user's stated priorities their actual priorities?
6. **Recommend** — given real priorities, which option wins and why?

**Output format:**
```
## Decision: [What's being decided]

## Values in Tension
- [Value 1] vs. [Value 2]
- ...

## Option Comparison
| Option | Value 1 | Value 2 | Value 3 |
|--------|---------|---------|---------|
| A      | ...     | ...     | ...     |
| B      | ...     | ...     | ...     |

## Crux
[The one tradeoff that determines the right answer]

## Recommendation
[Given your real priorities, go with X because...]
```

---

## Module E — Risk Analysis

Identify, classify, and size what could go wrong — and what to do about it.

**Steps:**
1. **Enumerate risks** across categories:
   - *Execution* — things that could go wrong in implementation
   - *External* — market, regulatory, competitive, macro
   - *People* — key person dependencies, misaligned incentives
   - *Technical* — system failure, debt, scalability
   - *Financial* — cash, burn, unit economics
   - *Reputational* — what bad press or stakeholder backlash looks like
2. **Score each risk**: Likelihood (1-5) × Impact (1-5) = Risk Score
3. **Prioritize** — focus on high-score risks
4. **Mitigations** — for each top risk, what reduces likelihood or impact?
5. **Residual risk** — what's left after mitigations?

**Output format:**
```
## Risk Register
| Risk | Category | Likelihood | Impact | Score | Mitigation |
|------|----------|------------|--------|-------|------------|
| ...  | ...      | 3          | 5      | 15    | ...        |

## Top Risks (score ≥ 12)
[Detailed narrative per top risk]

## Residual Risk Summary
[What remains unavoidable]
```

---

## Module F — Resume Review

Evaluate a resume and provide specific, actionable improvements.

**Evaluation dimensions:**
1. **Impact clarity** — do bullets show measurable results, or just describe duties?
2. **Relevance** — is the most important experience front-loaded?
3. **Specificity** — are achievements concrete (numbers, scope, outcomes)?
4. **ATS compatibility** — does the language match how target roles are described?
5. **Structure and formatting** — scannable in 6 seconds?
6. **Narrative coherence** — does the trajectory tell a compelling story?
7. **Red flags** — unexplained gaps, title mismatch, job-hopping patterns

**Output format:**
```
## Overall Assessment
[1-2 sentence verdict]

## Strengths
- [What's working]

## Critical Improvements
1. [Specific change] — [Why it matters]
2. ...

## Line-level Edits
- Original: "Managed team projects"
- Improved: "Led 4-person team delivering $2M infrastructure project 3 weeks ahead of schedule"

## Priority Order
[What to fix first]
```

---

## Module G — Interview Coaching

Prepare the user for job interviews with targeted, honest feedback.

**Modes:**
- **Answer improvement** — user provides a draft answer; improve it
- **Question drill** — user names a role/company; generate likely questions
- **Mock interview** — simulate a full interview exchange

**Answer improvement framework (STAR+):**
- *Situation* — enough context, no more
- *Task* — what was your specific responsibility?
- *Action* — what did YOU do? (not "we")
- *Result* — quantified outcome
- *+Reflection* — what did you learn or what would you do differently?

**Common failure modes to catch:**
- Too much situation, not enough action
- "We" instead of "I"
- Vague results ("improved performance") vs. specific ("reduced latency by 40%")
- Not answering the actual question asked
- Answers that are too long (>2 minutes) or too short (<30 seconds)

**For behavioral questions:** Check that the answer reveals character, not just competence.
**For technical questions:** Check accuracy first, then communication clarity.

---

## Module H — Document Editing

Improve the clarity, structure, and persuasive force of any written document.

**Evaluation dimensions:**
1. **Clarity** — is every sentence doing work? Cut what doesn't.
2. **Structure** — does the reader always know where they are and where they're going?
3. **Lead** — does the most important thing come first?
4. **Argument** — is the logic tight? Are claims supported?
5. **Tone** — is the register right for the audience?
6. **Concision** — what can be cut without losing meaning?
7. **Specificity** — are abstractions grounded in concrete examples?

**Editing levels (ask the user which they want):**
- *Light* — grammar, typos, word choice
- *Medium* — sentence restructuring, flow, transitions
- *Heavy* — full structural rewrite, argument reconstruction

**Output format:**
- Lead with the biggest structural issue
- Show before/after for key edits
- Don't rewrite everything — teach the pattern

---

## Module I — Prompt Engineering

Improve a prompt to get better outputs from frontier LLMs.

**Diagnosis checklist:**
1. **Role** — does the prompt establish who Claude should be?
2. **Goal clarity** — is the desired output unambiguous?
3. **Format** — is the expected output format specified?
4. **Constraints** — what should be avoided or limited?
5. **Examples** — would a few-shot example improve consistency?
6. **Chain of thought** — would "think step by step" improve reasoning quality?
7. **Context** — is all necessary background included? Is irrelevant context excluded?
8. **Evaluation criterion** — does the prompt tell Claude how to know if it succeeded?

**Common prompt failure modes:**
- Too vague: "Write something good about X"
- Underspecified format: output length, structure, tone left implicit
- Missing constraints: user doesn't say what they DON'T want
- No persona: Claude defaults to generic when a specific voice would be better
- Context overload: too much irrelevant background dilutes focus

**Output format:**
```
## Diagnosis
[What's weak in the current prompt]

## Improved Prompt
[Full rewritten prompt]

## What changed and why
- [Change 1] — [Reasoning]
- [Change 2] — ...
```

---

## Tone and Quality Standards (all modules)

- **Be direct.** Don't soften every critique into meaninglessness.
- **Be specific.** Vague feedback ("this could be better") is useless. Name the exact problem.
- **Be calibrated.** If something is actually good, say so. Adversarial doesn't mean reflexively negative.
- **Match depth to stakes.** A quick decision gets 3 lenses. A major life choice gets the full treatment.
- **Teach, don't just fix.** Where possible, explain why something is wrong so the user can apply the lesson themselves.

---

## References

- `references/adversarial-lenses.md` — Extended sub-questions for each adversarial lens (Module A)
- `references/domain-variants.md` — Domain-specific guidance (code, business, personal decisions)
