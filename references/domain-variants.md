# Domain-Specific Variants

## Code / System Architecture

**Extra adversarial lenses:**
- Failure modes — can this fail silently? What's the blast radius?
- Scalability cliffs — does this work at 10x traffic or data?
- Dependency risk — what external systems could break this?
- Reversibility — how hard is this decision to undo?
- Security surface — where is the trust boundary? What if input is adversarial?

**First principles for code:**
- What is the actual bottleneck? (measure before optimizing)
- What's the simplest thing that could work?
- What problem does this abstraction actually solve — and does it solve it better than no abstraction?

---

## Business / Strategy

**Extra adversarial lenses:**
- Competitive response — what does the incumbent do when you launch?
- Distribution moat — can you reach customers even if the product is great?
- Unit economics — does this work at scale AND at early stage?
- Regulatory exposure — is there a hidden legal risk?

**First principles for business:**
- What do customers actually want beneath what they say they want?
- What are the real constraints — capital, talent, time, attention?
- What would this look like if it were 10x simpler?

---

## Personal Decisions

**Extra adversarial lenses:**
- Reversibility — easy or hard to undo?
- Identity trap — is this decision driven by who you want to be, or who you are?
- Social pressure — are you making this for yourself or to satisfy external expectations?
- Time horizons — are you optimizing for how you'll feel in 1 week vs. 1 year vs. 10 years?

**First principles for personal decisions:**
- What do I actually want? (vs. what I think I should want)
- What are my real constraints? (vs. what I'm using as an excuse)
- What would I advise a close friend in this situation?

---

## Hiring / Career Decisions

**Resume context:**
- The 6-second test: what does a recruiter see in the first scan?
- ATS systems look for keyword match to job descriptions — target language matters
- Senior roles need evidence of scope and impact, not just output
- Gaps and transitions need to be pre-empted, not left unexplained

**Interview context:**
- Behavioral questions are really asking: "Will you behave this way here?"
- Technical screens test problem-solving approach as much as correct answers
- Culture fit questions are about values alignment — prepare genuine examples
- Questions you ask the interviewer signal what you care about

---

## Prompts / LLM Workflows

**When a prompt is underperforming, check in order:**
1. Is the task actually well-defined? (Most prompt problems are problem-definition problems)
2. Is the model the right one for this task?
3. Is context too long, too short, or irrelevant?
4. Would few-shot examples help?
5. Would chain-of-thought help?
6. Are format constraints too tight or too loose?

**Prompt red flags:**
- "Do your best" — no evaluation criterion
- "Be creative" — undefined creative direction
- No examples when the task has high stylistic specificity
- Role-setting that's too generic ("You are a helpful assistant")
