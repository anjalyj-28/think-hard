# 🧠 think-hard

> *A modular reasoning framework for improving the quality of decisions, documents, and thinking — powered by Claude.*

---

## What is think-hard?

**think-hard** is a Claude skill that acts as a personal reasoning upgrade. Instead of asking Claude to "just answer," think-hard activates structured, rigorous thinking frameworks tailored to what you're actually trying to accomplish — whether that's stress-testing a business plan, prepping for an interview, fact-checking an argument, or improving a prompt.

It's built for **one person making better decisions**, not enterprise workflows or team tooling.

The skill is modular — each reasoning mode is self-contained and can be used alone or combined. Claude automatically selects the right module(s) based on what you ask.

**Doesn't Claude automatically apply adversarial thinking ?** Yes Claude does but it is shallow and inconsistent. Without this skill, Claude thinks like a smart generalist. With this skill, it thinks like a structured analyst. Same model, different output quality and consistency.
---

## Why think-hard?

Frontier LLMs like Claude are capable of much sharper reasoning than they typically default to. The gap between a generic Claude response and a genuinely rigorous one is mostly about *how you structure the thinking* — not the model itself.

think-hard closes that gap by giving Claude:
- A clear reasoning procedure for each type of problem
- Specific output formats that force precision over vagueness
- Quality standards (specificity, calibration, directness) enforced at every step

The result: responses that are sharper, more actionable, and harder to bullshit.

---

## Modules

| Module | What it does | Example trigger |
|--------|-------------|-----------------|
| 🔴 **Adversarial Thinking** | Systematically attacks a plan from multiple hostile angles, ranks threats, delivers a verdict | *"Red team my startup pitch"* |
| 🔵 **First Principles** | Strips inherited assumptions, finds bedrock truths, rebuilds from scratch | *"First principles — should we build this in-house?"* |
| 🟡 **Fact Verification** | Audits claims, flags unsourced stats, surfaces epistemic status of each assertion | *"Verify the claims in this doc"* |
| 🟢 **Tradeoff Analysis** | Maps competing values, finds the crux of a decision, pressure-tests stated priorities | *"What are the real tradeoffs between SQL and NoSQL here?"* |
| 🟠 **Risk Analysis** | Identifies, scores (likelihood × impact), and mitigates what could go wrong | *"Run a risk analysis on this launch plan"* |
| 🟣 **Resume Review** | Evaluates impact clarity, ATS compatibility, narrative coherence — with line-level edits | *"Review my resume"* |
| 🔶 **Interview Coaching** | Sharpens STAR answers, drills likely questions, runs mock interviews | *"Help me prep for a TPM interview at Google"* |
| ⚪ **Document Editing** | Improves clarity, structure, and persuasive force at light/medium/heavy edit levels | *"Edit this design doc"* |
| 🔷 **Prompt Engineering** | Diagnoses weak prompts and rewrites them for better LLM outputs | *"Improve this prompt"* |

Modules can be combined — a system design doc review might activate Adversarial + Fact Verification + Document Editing simultaneously.

---

## How to use it

Just talk to Claude naturally. think-hard triggers automatically on phrases like:

```
think hard about this        red team this
first principles             what are the tradeoffs
what could go wrong          poke holes in my plan
steelman the opposition      what am I missing
review my resume             help me prep for interviews
fact check this              improve this prompt
edit this doc                stress test this idea
challenge my assumptions     verify this
```

You don't need to name the module — Claude infers which one(s) apply.

---

## Design principles

**Modular** — Each reasoning mode is self-contained. One skill, nine tools.

**Individual-first** — Built for a single person improving their own thinking. Not a team workflow or enterprise system.

**Direct and calibrated** — Sharp feedback without reflexive negativity. If something is genuinely good, the skill says so. Adversarial doesn't mean contrarian.

**Depth-matched** — A quick sanity check gets 3 lenses. A major career or business decision gets the full treatment.

**Teaching over fixing** — The skill explains *why* something is wrong, not just what. The goal is pattern transfer, not dependency.

---

## File structure

```
think-hard/
├── README.md                        ← You're reading this
├── SKILL.md                         ← Core skill instructions (Claude reads this on trigger)
├── references/
│   ├── adversarial-lenses.md        ← Extended sub-questions for each adversarial lens
│   └── domain-variants.md           ← Domain guidance: code, business, personal, career, prompts
└── tests/
    ├── README.md                    ← Test index and scoring rubric
    └── system-design-test-cases.md  ← 5 annotated test cases with example outputs
```

---

## Test cases

The `tests/` folder includes 5 fully worked system design test cases covering:
- URL shortener at scale (adversarial)
- Microservices vs. monolith (first principles + tradeoffs)
- Redis caching for a financial dashboard (risk analysis)
- SQL vs. NoSQL for a social feed (tradeoffs + first principles)
- Full review of a notification service design doc (adversarial + fact verification + doc editing)

Each case includes a verbatim user prompt, a must-include checklist for grading, and a full example output to calibrate quality.

---

## Installation

### Via Claude Skills UI
1. Download `think-hard.skill`
2. Go to **claude.ai → Customize → Skills**
3. Upload the `.skill` file

### Via Claude Code
Place the `think-hard/` folder in `.claude/skills/` in your project — it loads automatically.

### Via GitHub
```bash
git clone https://github.com/anjalyj-28/think-hard.git
```

---

## Built by
Anjaly Joseph — Technical Program Manager with experience across Meta, Google, NewsBreak, and Symantec.  
[LinkedIn](https://www.linkedin.com/in/anjaly-joseph-0921355/)
