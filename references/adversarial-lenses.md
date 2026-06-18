# Extended Adversarial Lenses Reference

This file provides deeper sub-questions, failure modes, and domain variants for each lens in the think-hard skill.

---

## Lens A — Assumptions Under Attack

**Sub-questions:**
- What must be true about the market, the user, the technology, the team, or the environment for this to work?
- Which assumption has the least evidence behind it?
- Are any assumptions circular (i.e., the plan succeeds because the plan succeeds)?
- Is the user assuming the future will resemble the past?

**Common failure mode:** Treating an assumption as a fact because it's been repeated often, or because it would be painful to acknowledge it might be wrong.

**Domain variant — Code/Tech:**
- Are you assuming the third-party API/service will remain stable, available, or free?
- Are you assuming users will behave the way you expect them to?

---

## Lens B — Second and Third Order Effects

**Sub-questions:**
- If this works exactly as planned, what then?
- Who else is affected who wasn't mentioned?
- Does success in this area create failure in another area (e.g., winning one customer segment alienates another)?
- Does this intervention change the behavior of the system it's intervening in (Goodhart's Law)?

**Common failure mode:** Optimizing a local metric while degrading the global system. E.g., making a single feature faster while creating architectural debt that slows everything else.

---

## Lens C — Adversarial Actors

**Sub-questions:**
- Who has a financial or competitive incentive for this to fail?
- What's the cheapest attack surface?
- Is there regulatory, legal, or political opposition that could materialize?
- If you succeed, does that create a threat that didn't exist before (e.g., a large incumbent now notices you)?

**Domain variant — Security/Code:**
- Where is the trust boundary? What happens if input is adversarial?
- What's the blast radius if credentials are compromised?
- Are you assuming users are honest?

---

## Lens D — Selection Bias / Base Rates

**Sub-questions:**
- What is the historical success rate for this type of plan/decision?
- Are the examples you're drawing on representative, or are they survivorship-biased?
- Is the reference class too narrow or too broad?
- Are you using "this feels like it could work" when you should be asking "what fraction of things that felt like this actually worked?"

**Common failure mode:** Entrepreneurs citing successful startups as evidence their startup will succeed, without accounting for the thousands that failed silently.

---

## Lens E — Missing Alternatives

**Sub-questions:**
- Is the comparison implicit or explicit? (i.e., better than what?)
- What would a 10x more experienced person do instead?
- Are you solving the symptom rather than the root cause?
- Is there a "do nothing" option that's actually better than this plan?
- What's the simplest version of this that achieves 80% of the value?

**Common failure mode:** Sunken cost reasoning — continuing with a plan because of what's already been invested rather than because it's the best path forward.

---

## Lens F — Execution Risk

**Sub-questions:**
- What has to go right simultaneously for this to succeed?
- What's the critical path? What's on it that's high-variance?
- Does this require sustained effort over time — and is there a realistic plan for that?
- Is the timeline plausible given the resources available?
- Are the people who need to execute this capable of doing so, or is there a skill gap?

**Domain variant — Personal Decisions:**
- Does this require you to behave differently than you have historically? If so, what makes you confident you'll change?

---

## Lens G — Internal Contradictions

**Sub-questions:**
- Are any two stated goals in tension with each other?
- Does the proposed method actually achieve the stated goal, or does it achieve a proxy?
- Are stated values and proposed actions misaligned?
- Does the argument prove too much? (i.e., if the argument is valid, does it also justify things the user would reject?)

**Common failure mode:** Claiming to optimize for user experience while every concrete decision optimizes for revenue. The contradiction undermines both.

---

## Lens H — Epistemic Weaknesses

**Sub-questions:**
- What evidence would change your mind? If nothing would, that's a red flag.
- Are you reasoning toward a predetermined conclusion?
- What are you most emotionally attached to in this plan — and is that attachment influencing your judgment?
- Are you overweighting recent or vivid information (availability bias)?
- Are you anchoring on the first number/frame you encountered?

**Common failure mode:** Motivated reasoning where someone constructs an argument to justify what they already want to do, rather than to discover what they should do.

---

## Domain-Specific Guidance

### Adversarial Thinking for Code / Architecture

Key extra lenses:
- **Failure modes**: What happens when this fails? Can it fail silently?
- **Scalability cliffs**: Does this work at 10x traffic/data?
- **Dependency risk**: What external systems does this depend on?
- **Reversibility**: How hard is it to undo this decision?
- **Testability**: Can you actually verify this works correctly?

### Adversarial Thinking for Business / Strategy

Key extra lenses:
- **Competitive response**: What does the competition do when you launch this?
- **Distribution moat**: Even if the product is great, can you reach customers?
- **Unit economics**: Does this work at scale? At early stage?
- **Regulatory risk**: Is there a hidden regulatory exposure?

### Adversarial Thinking for Personal Decisions

Key extra lenses:
- **Reversibility**: Is this decision easy or hard to undo?
- **Identity trap**: Are you making this decision because it's right, or because it fits a story you tell about yourself?
- **Social influence**: Are you making this decision for yourself, or to satisfy external expectations?
- **Time horizons**: Are you optimizing for how you'll feel in 1 week vs. 1 year vs. 10 years?

### Adversarial Thinking for Arguments / Debates

Key extra lenses:
- **Steelman quality**: Are you engaging with the strongest version of the opposing view?
- **Definition disputes**: Are you arguing about words rather than facts?
- **Scope creep**: Is the argument trying to prove more than the evidence supports?
- **Fallacy check**: Ad hominem, false dilemma, slippery slope, appeal to authority?
