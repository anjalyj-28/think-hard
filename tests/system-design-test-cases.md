# Test Cases: System Design

These test cases validate think-hard across multiple modules applied to system design problems.
Each case includes: the user prompt, which module(s) to activate, the key things a good response
must include, and a full example output to calibrate quality.

---

## TC-01: Adversarial Thinking — URL Shortener at Scale

**Module(s):** A (Adversarial), E (Risk)

**User prompt:**
> I'm designing a URL shortener service (like bit.ly). Here's my plan: a single Postgres database
> stores the mapping from short code to long URL. I'll use a base62 encoding of an auto-increment
> integer ID as the short code. The app tier is stateless and horizontally scalable. I think this
> is solid — what am I missing?

**Must include:**
- [ ] Identifies the auto-increment integer as a single point of failure and predictability risk
- [ ] Calls out that Postgres becomes the bottleneck at high read volume (cache missing)
- [ ] Notes short codes are enumerable/guessable (security issue)
- [ ] Raises the redirect latency question (how many hops?)
- [ ] Offers a concrete alternative or mitigation for each issue
- [ ] Verdict is "conditionally viable" not "fundamentally flawed" — the core idea is sound

**Example output:**

---

### Steelman
A stateless app tier backed by a single Postgres instance is a reasonable starting point: simple to reason about, easy to deploy, and Postgres handles millions of rows without complaint. Auto-increment IDs are collision-free by construction, and base62 encoding gives compact URLs. This is a perfectly defensible v1.

### Adversarial Analysis

**Assumptions — Postgres will handle the read load**
URL shorteners are almost entirely read-heavy (redirects >> writes). At scale, a single Postgres instance will saturate on read IOPS well before it struggles with storage or writes. Without a caching layer (Redis in front), every redirect hits the DB. At 10K RPS that's survivable; at 100K RPS you're in trouble. The plan doesn't mention caching at all.

**Adversarial Actors — Short codes are enumerable**
Base62 of sequential integers means codes are guessable: `aaaaab`, `aaaaac`, etc. A scraper can harvest your entire link database by incrementing through IDs. This exposes private/internal links that users assumed were obscure. If any customer is using this for sensitive internal URLs, that's a data exposure. Consider random code generation or HMAC-based codes instead.

**Execution Risk — Auto-increment ID is a write bottleneck and coordination point**
If you ever shard or move to multiple writers, Postgres auto-increment breaks — you get ID collisions. Even before that, the single sequence is a serialization point under high write load. For a URL shortener write load is modest, but the architectural assumption (one DB forever) will box you in. Snowflake IDs or UUIDs from the app layer are more portable.

**Missing Alternatives — No analytics layer considered**
URL shorteners almost always need click tracking (per-link analytics, referrer data, geo). If analytics are a requirement — even future ones — logging redirects to Postgres synchronously will tank your redirect latency. You need an async event pipeline (Kafka → ClickHouse or similar) from day one, because retrofitting it is expensive.

**Second-order Effects — Redirect chain latency**
A redirect adds one full round trip for the user. If your servers are US-only and users are global, you're adding 150-200ms to every click. A CDN-level redirect (push the mapping to edge nodes) would eliminate this, but the current architecture makes that impossible — everything hits origin.

### Top 3 Threats
1. **No cache layer** — Postgres read bottleneck will manifest early; easily fixed with Redis but absent from the plan
2. **Enumerable short codes** — Security and privacy risk that's cheap to exploit and hard to retrofit once you have existing links
3. **Single-region origin redirect** — Latency penalty for global users; CDN-based redirect requires architecture change

### Verdict
**Conditionally viable.** The core design is fine for low-to-medium scale. Three concrete fixes before launch:

**Crux**: Add Redis in front of Postgres for redirect reads — this alone buys you 10-50x headroom.

**Recommendation**:
1. Add Redis cache for `short_code → long_url` mapping (TTL: indefinite for non-expiring links)
2. Switch to random 7-8 char base62 codes (generated in app, collision-checked against DB)
3. Log redirects asynchronously (fire-and-forget to a queue) rather than synchronously

---

## TC-02: First Principles — "We need a microservices architecture"

**Module(s):** B (First Principles), D (Tradeoffs)

**User prompt:**
> My team lead says we need to migrate from our monolith to microservices. We're a 6-person startup
> with a 3-year-old Rails monolith doing about 50K users. Think hard about whether this is the
> right call.

**Must include:**
- [ ] Questions whether microservices actually solve the problems the team has
- [ ] Surfaces the real costs of microservices that are usually underestimated (ops complexity, distributed systems failures, latency)
- [ ] Uses first principles to identify what problem they're actually trying to solve
- [ ] References base rate / selection bias (survivorship bias from Netflix/Amazon stories)
- [ ] Recommends a concrete alternative (modular monolith, strangler fig) before full migration
- [ ] Verdict leans skeptical but not dismissive — context-dependent

**Example output:**

---

### First Principles: What problem are we actually solving?

Before evaluating the solution, let's find the problem. The claim "we need microservices" is a
solution in search of a stated problem. Common real problems that microservices get prescribed for:

- **Deployment bottlenecks** — teams blocked on each other to ship
- **Scaling bottlenecks** — one part of the system needs 100x more resources than others
- **Technology diversity** — different components need different languages/runtimes
- **Team scaling** — organization is growing and needs autonomous squads

At 6 people and 50K users, ask: which of these do you actually have?

A 3-year Rails monolith at your scale is almost certainly **not** bottlenecked on any of these.
50K users is modest. 6 people can coordinate in a standup. You're probably not saturating compute
on a single service.

### Tradeoff Analysis

**Decision:** Microservices migration vs. alternatives

**Values in tension:** Developer velocity vs. future scalability | Simplicity vs. team autonomy | Short-term pain vs. long-term flexibility

| Dimension | Monolith (status quo) | Microservices | Modular Monolith |
|-----------|----------------------|---------------|------------------|
| Deploy complexity | Low | Very high | Low |
| Operational overhead | Low | High (k8s, service mesh, distributed tracing) | Low |
| Team coordination | Easy at 6 people | Unnecessary at 6 people | Easy |
| Failure modes | Simple | Distributed (partial failures, timeouts, cascading) | Simple |
| Scale headroom | Fine to ~500K users with good indexes | Unlimited but overkill | Fine to ~500K users |
| Migration cost | None | 12-24 months engineering time | 2-4 months |

**Selection bias warning:** Every microservices success story you've heard (Netflix, Amazon, Uber)
is from companies that had the problem microservices solve — hundreds of engineers, massive scale,
distinct scaling profiles per service. You're not Netflix. The monoliths that work fine at your
scale don't write blog posts about it.

**The real costs usually underestimated:**
- Network calls replace in-process calls — latency increases, and now calls can fail
- You need distributed tracing, centralized logging, service discovery from day one
- Every "simple" feature now requires coordinating deploys across N services
- Debugging a production issue requires correlating logs across 8 services instead of one

### Crux
The question isn't "microservices or not" — it's "what specific pain are we trying to fix?"

### Recommendation
1. **Name the actual problem** — if it's "the codebase is a mess," that's a modular monolith problem, not a microservices problem
2. **Try a modular monolith first** — enforce module boundaries in Rails (Packwerk, or just disciplined namespacing). Same deployment, cleaner architecture, zero ops overhead
3. **Extract one service as a proof of concept only if** you have a clear scaling or team-autonomy reason that can't be solved by the monolith
4. **Revisit microservices at 50 engineers or 5M users**, whichever comes first

---

## TC-03: Risk Analysis — Caching Strategy for a Financial Dashboard

**Module(s):** E (Risk), A (Adversarial)

**User prompt:**
> We're adding Redis caching to our financial dashboard. Plan is: cache all API responses for
> 5 minutes, use cache-aside pattern, invalidate on write. Users are institutional traders looking
> at portfolio positions. Run a risk analysis on this.

**Must include:**
- [ ] Flags stale data risk in a financial context as high-severity (traders acting on wrong positions)
- [ ] Questions whether 5-minute TTL is calibrated to data freshness requirements
- [ ] Identifies cache stampede risk (TTL expiry under load)
- [ ] Notes that cache-aside + invalidate-on-write has race conditions
- [ ] Asks about cache warming / cold start
- [ ] Risk register has at least 4 risks with scores

**Example output:**

---

### Risk Register

| Risk | Category | Likelihood | Impact | Score | Mitigation |
|------|----------|------------|--------|-------|------------|
| Trader acts on stale position data | Data integrity | 3 | 5 | 15 | Lower TTL for position data; add staleness indicator in UI |
| Cache stampede on TTL expiry | Technical | 3 | 4 | 12 | Probabilistic early expiry or mutex lock on cache miss |
| Write invalidation race condition | Technical | 4 | 3 | 12 | Use write-through or versioned cache keys |
| Cold start / cache miss storm on deploy | Technical | 4 | 3 | 12 | Cache warming script on startup |
| Redis goes down, no fallback | Reliability | 2 | 5 | 10 | Circuit breaker pattern; fallback to DB reads |
| Wrong data cached per user (multi-tenancy) | Security | 2 | 5 | 10 | Cache keys must include user/org ID |

### Top Risks (score ≥ 12)

**1. Stale position data (score: 15)**
Institutional traders make decisions on live portfolio positions. A 5-minute TTL means a position
that was closed 4 minutes ago still shows as open — a trader could act on a ghost position. At
institutional scale, this isn't a UX bug, it's a liability issue. Financial data caching requires
per-field TTL calibration: market prices might tolerate 30 seconds; position data should be
near-real-time or explicitly marked as delayed with a timestamp.

**2. Cache stampede (score: 12)**
When a hot cache key expires, all requests simultaneously find a cache miss and hammer the
upstream API or DB. Under your institutional load profile, if 200 traders are all looking at the
same portfolio view and the TTL fires, you get a thundering herd. Fix: use probabilistic early
expiry (jitter the TTL by ±10%) or a mutex (only one request regenerates the cache; others wait).

**3. Write invalidation race condition (score: 12)**
Cache-aside with invalidate-on-write has a classic race: (1) write to DB, (2) delete cache key,
(3) concurrent read repopulates cache from DB *before* the write commits or replicates. Result:
stale data back in cache immediately after invalidation. Fix: use write-through caching (update
cache atomically with DB write) or append a version number to cache keys and let old keys expire.

**4. Cold start cache miss storm (score: 12)**
On deploy or Redis restart, every request is a cache miss simultaneously. For a trading dashboard
opened at market open (9:30am), this creates a spike. Fix: implement a cache warming script that
pre-populates the most-accessed keys after startup, before serving traffic.

### Residual Risk Summary
Even with mitigations, caching financial data carries inherent tension between freshness and
performance. The architectural recommendation is: **don't apply one TTL to all data types.**
Tier your caching strategy:
- Reference data (instrument names, account metadata): 60 min TTL
- Aggregated P&L / reporting: 5 min TTL with staleness watermark in UI
- Live positions: no cache, or WebSocket push with cache only as fallback

---

## TC-04: Tradeoff Analysis — SQL vs. NoSQL for a Social Feed

**Module(s):** D (Tradeoffs), B (First Principles)

**User prompt:**
> I'm building a social feed (like Twitter's home timeline). Should I use Postgres or Cassandra?
> I expect 1M users eventually. Help me think through the tradeoffs.

**Must include:**
- [ ] Distinguishes between fan-out-on-write vs fan-out-on-read models before picking a DB
- [ ] Notes that DB choice is downstream of data model choice
- [ ] Identifies the specific access patterns (write fan-out, read hotspots for celebrities)
- [ ] Doesn't just compare SQL vs NoSQL generically — makes it specific to feeds
- [ ] Gives a concrete recommendation based on scale and team size

**Example output:**

---

### First Principles: What does a social feed actually need to do?

Before picking a database, fix the data model. There are two fundamentally different architectures
for social feeds, and the DB choice flows from that decision — not the other way around.

**Model 1 — Fan-out on write (push)**
When user A posts, you immediately write a copy of the post into each follower's feed.
- Read: O(1) — just fetch the pre-built feed
- Write: O(followers) — expensive for celebrities (Lady Gaga = 80M writes per post)
- Good for: read-heavy workloads, low-latency feed loads

**Model 2 — Fan-out on read (pull)**
Store posts once. When a user loads their feed, fetch posts from everyone they follow and merge.
- Read: O(following count) — expensive at load time
- Write: O(1) — just one write
- Good for: write-heavy workloads, celebrity problem is moot

Twitter uses a hybrid: fan-out on write for most users, fan-out on read for celebrities.

### Tradeoff Analysis

**Decision:** Postgres vs. Cassandra for social feed backend

**Crux first:** The crux isn't SQL vs NoSQL. The crux is: do you want to optimize reads or writes?
Cassandra is a write-optimized, partition-key-access store. Postgres is a flexible, query-optimized
relational DB. Pick based on your data model, not hype.

| Dimension | Postgres | Cassandra |
|-----------|----------|-----------|
| Feed read (pre-built) | Fast with index | Fast with partition key |
| Arbitrary queries | Excellent | Poor (must know access pattern upfront) |
| Write throughput | Good to ~50K wps | Excellent, linear scale |
| Operational complexity | Low | High (tuning, compaction, consistency levels) |
| Schema flexibility | High | Low (schema changes are painful) |
| Team familiarity | Very common | Specialist knowledge required |
| Good at 1M users? | Yes, with read replicas | Yes, but overkill |

### Recommendation

At 1M users (eventual), start with **Postgres + Redis**:
- Postgres stores posts and the social graph
- Redis stores pre-built feed lists per user (sorted sets, trimmed to last 1000 posts)
- Fan-out on write in the background (async job per post)
- Add a `celebrity` flag: skip fan-out for accounts with >100K followers, do fan-out on read for those

Move to Cassandra only when:
- You're doing >100K writes/second AND
- You have an ops team comfortable with distributed systems AND
- The access patterns are stable enough to model as partition keys

Most products never hit that threshold. Cassandra at 1M users is usually engineering complexity
bought before it's needed.

---

## TC-05: Combined Modules — Review a System Design Doc

**Module(s):** A + C + H (Adversarial + Fact Verification + Document Editing)

**User prompt:**
> Here's an excerpt from a system design doc I wrote for our notification service. Can you
> think hard on it and give me a full review?
>
> *"We will use a push notification system built on AWS SNS. SNS guarantees at-least-once delivery
> and is infinitely scalable. The service will send 10M notifications per day. Since SNS handles
> all the complexity, our app logic will be simple. We plan to store notification state in DynamoDB
> with a TTL of 7 days."*

**Must include:**
- [ ] Flags "infinitely scalable" as a claim that needs qualifying
- [ ] Challenges "at-least-once" — explains what this means for duplicate notifications to users
- [ ] Notes missing: idempotency handling on the consumer side
- [ ] Notes missing: failure/retry visibility (dead letter queues, alerting)
- [ ] Notes missing: per-user notification preferences and opt-outs
- [ ] Improves the writing — removes vague claims, adds specificity

**Example output:**

---

### Document Review: Notification Service Design

**Adversarial Analysis**

**Fact Verification — "SNS guarantees at-least-once delivery"**
Technically accurate, but the implication that this is safe is not. At-least-once means users
*will* receive duplicate notifications under retry conditions. At 10M/day, even a 0.1% duplicate
rate is 10,000 duplicate push notifications daily. The doc doesn't mention idempotency handling
on the consumer side — you need deduplication logic (idempotency keys stored in DynamoDB checked
before send) or users will get the same notification twice. This is a known, documented SNS
behavior, not an edge case.

**Fact Verification — "Infinitely scalable"**
SNS has published limits: 30,000 sends/second per topic for mobile push, with a default account
limit of 3,000/s (soft limit, raisable). At 10M/day that's ~116/second average, well within
limits — but if your traffic is bursty (e.g., a product launch), you need to verify you've
requested limit increases. "Infinitely scalable" is marketing language; the real answer is
"scales well within our use case after limit increase requests."

**Assumptions — "SNS handles all the complexity"**
SNS handles *delivery* complexity. It does not handle: per-user notification preferences (opted
out?), notification deduplication, observability into delivery failures, dead-letter queues for
undeliverable messages, or rate limiting per user (don't spam a user with 50 notifications in
5 minutes). The doc assumes "simple app logic" but there's meaningful application-layer work
not accounted for.

**Missing Alternatives — No mention of failure visibility**
What happens when a notification fails to deliver? SNS publishes delivery status to CloudWatch,
but only if you configure it. The doc doesn't mention DLQs, alerting on delivery failure rates,
or retry policies. At 10M/day, even a 1% failure rate is 100K missed notifications — you need
to know about this.

### Top 3 Issues
1. **Duplicate notifications** — At-least-once delivery requires idempotency logic, which is absent
2. **No failure visibility** — Missing DLQ configuration and delivery failure alerting
3. **Missing user preferences** — No mention of opt-out handling (also legally required in many jurisdictions under CAN-SPAM / GDPR)

### Document Edit: Improved Version

**Before:**
> We will use a push notification system built on AWS SNS. SNS guarantees at-least-once delivery
> and is infinitely scalable. The service will send 10M notifications per day. Since SNS handles
> all the complexity, our app logic will be simple. We plan to store notification state in DynamoDB
> with a TTL of 7 days.

**After:**
> We will use AWS SNS for push notification delivery, targeting 10M notifications/day (~115/s
> average). SNS provides at-least-once delivery — our application layer will implement
> idempotency checks (deduplication key stored in DynamoDB, checked before each send) to prevent
> duplicate notifications to users.
>
> SNS handles delivery retry and fan-out. Our application layer is responsible for: per-user
> preference management (opt-in/opt-out), notification rate limiting per user, and delivery
> observability. We will configure SNS delivery status logging to CloudWatch and a dead-letter
> queue (SQS) for undeliverable messages, with alerting if the DLQ depth exceeds threshold.
>
> Notification state and idempotency keys will be stored in DynamoDB with a 7-day TTL. Account-
> level SNS throughput limits will be raised via AWS support ahead of launch.

**What changed:**
- Removed "infinitely scalable" — replaced with concrete numbers and the limit raise requirement
- Changed "at-least-once" from a feature to a constraint requiring a mitigation
- Made "simple app logic" honest — named the things the app layer must own
- Added DLQ, alerting, and preference management as explicit requirements

---

## How to Use These Test Cases

1. **Manual test**: Give the user prompt to Claude with think-hard skill active. Compare output to "must include" checklist.
2. **Calibration**: Use the example output as the quality bar — not as a template to match exactly.
3. **Regression**: After editing SKILL.md, re-run all 5 prompts to confirm quality doesn't degrade.
4. **Scoring**: A response passes if it hits ≥ 80% of must-include items and doesn't hallucinate technical facts.
