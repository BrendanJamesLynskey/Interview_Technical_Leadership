# Code Review Practices — Interview Questions

**Subject:** Technical Leadership
**Topic:** Effective Code Reviews, Review Checklists, Feedback, Reviewer Bias
**Difficulty tiers:** Fundamentals / Intermediate / Advanced

---

## Fundamentals

### Q1. What are the goals of a code review?

**Answer:**

Code review serves multiple overlapping purposes. A senior engineer should be able to name all of them and explain which ones matter in a given context.

1. **Defect detection.** Catch bugs, security issues, and correctness problems before they reach production.
2. **Knowledge sharing.** Spread context about new code across the team. Prevent single-author silos.
3. **Design feedback.** Surface architectural issues while they are cheap to change.
4. **Standards enforcement.** Maintain consistency in style, error handling, logging, and observability.
5. **Mentorship.** Teach junior engineers through concrete feedback on real work.
6. **Shared ownership.** When two engineers approve a change, the team owns it — not just the author.

**Interview insight:** Candidates who reduce code review to "finding bugs" reveal a junior mindset. Google's research (via *Software Engineering at Google*) shows that bug-finding is a side effect of review; the dominant value is knowledge sharing and design feedback.

### Q2. What is the difference between a code review and a pull request?

**Answer:**

A **pull request (PR)** is a mechanism — a unit of change proposed for merge, usually with diff viewing, CI status, and discussion threads attached.

A **code review** is the activity — the human practice of reading, understanding, and giving feedback on proposed changes.

Every healthy PR has a review; not every review happens via a PR. Pair programming is live review. Pre-commit review tools (Gerrit, Phabricator Differential) implement review without the "pull" metaphor. Mob programming treats review as continuous.

**Why this distinction matters in interviews:** Candidates who conflate the two struggle to reason about review practices outside of GitHub/GitLab workflows (e.g., trunk-based development with mandatory pair programming).

### Q3. What should a reviewer look for, in priority order?

**Answer:**

Review in descending order of cost-to-fix. Cheap-to-fix things reviewed last are fine; expensive-to-fix things missed are disasters.

1. **Correctness.** Does the code do what the description claims? Edge cases, error paths, concurrency.
2. **Design.** Does this change belong here? Is the abstraction right? Does it create coupling that will hurt later?
3. **Security.** Input validation, injection, auth, secrets, PII handling.
4. **Observability.** Can we debug this at 3am? Are logs, metrics, and traces appropriate?
5. **Tests.** Are behaviours covered? Are tests testing behaviour, not implementation?
6. **Readability.** Will a new team member understand this in six months?
7. **Consistency.** Does it match patterns elsewhere in the codebase?
8. **Style/formatting.** Delegated to linters and formatters — not humans.

**Key principle:** A reviewer's attention is finite. Spend it on the things machines can't check. If you are arguing about whitespace, your tooling is broken.

### Q4. How big should a pull request be?

**Answer:**

**Smaller is better.** The research is consistent: review quality drops sharply once a change exceeds roughly 200–400 lines of diff. Cisco's SmartBear study (2006) is the classic reference; Google and Microsoft have reported similar findings internally.

**Heuristics:**

- **Under 100 lines:** Reviewed thoroughly in a single sitting.
- **100–400 lines:** Reviewable, but requires focus. Splitting improves quality.
- **400+ lines:** Reviewers skim. Bugs slip through. Consider whether the PR is actually multiple logical changes.

**Techniques for keeping PRs small:**

- Separate refactors from behaviour changes. A refactor PR should have no behavioural diff in tests; a behaviour PR should have minimal refactoring.
- Land infrastructure (interfaces, config, DB migrations) before implementations.
- Feature-flag large features and ship them in many small, independently-reviewable pieces.
- Use stacked PRs (Phabricator, Graphite, `git absorb`) for large logical changes split into reviewable pieces.

**Interview phrasing:** "I optimise for review latency and reviewer attention, not for PR count. A 50-line PR reviewed in 20 minutes beats a 500-line PR reviewed in 20 minutes."

### Q5. What is the difference between a blocking comment and a suggestion?

**Answer:**

A **blocking comment** asserts that the PR must not merge until addressed. A **suggestion** is an opinion the author may adopt or decline.

Teams need explicit norms for distinguishing these, otherwise reviews become adversarial. Common conventions:

- **Conventional Comments** (`nit:`, `suggestion:`, `question:`, `issue:`, `praise:`) make intent explicit.
- **Prefix `nit:`** means "style preference, feel free to ignore."
- **Prefix `blocking:`** or `must-fix:` removes ambiguity.
- **"Approve with comments"** in GitHub means "I trust you to address or dismiss these."

**Why this matters:** Without explicit conventions, every comment reads as a demand. Authors become defensive; reviewers become cautious. Making the status explicit is one of the highest-leverage changes a team can make to its review culture.

```markdown
# Conventional Comments in practice

nit: consider extracting this into a helper
question: why uint32 here rather than int64?
suggestion: we could cache this — non-blocking
issue (blocking): this races with the cleanup goroutine in worker.go:42
praise: this refactor is much clearer than the previous structure
```

### Q6. What should an author do before requesting review?

**Answer:**

A senior author self-reviews before inviting anyone else. Respecting reviewer time is a leadership behaviour.

**Pre-review checklist:**

1. **Read your own diff.** Catch obvious issues — debug prints, TODOs, dead code.
2. **Run tests locally.** Don't rely solely on CI.
3. **Write the PR description.** Problem, approach, trade-offs, what to look at first. Link the design doc or ticket.
4. **Call out risk.** Migrations, feature flags, rollback plan. "This PR changes hot-path serialisation — please check the benchmarks."
5. **Anticipate questions.** If a choice looks odd, explain it in the PR description or as a self-comment on the line.
6. **Right-size the PR.** If it's over 400 lines, consider splitting it before requesting review.

**Interview insight:** Strong candidates describe self-review as a leadership behaviour. It signals respect for the reviewer and produces better review outcomes. Weak candidates treat review as the first time they look at the diff critically.

---

## Intermediate

### Q7. Give an example of a code review comment that teaches rather than dictates.

**Answer:**

Compare two comments on the same code:

```python
# The code under review
def process_records(records):
    results = []
    for r in records:
        if r.status == "active":
            results.append(transform(r))
    return results
```

**Dictating:**

> Use a list comprehension here.

**Teaching:**

> Consider a list comprehension: `[transform(r) for r in records if r.status == "active"]`.
> Comprehensions are idiomatic Python and the intent — "map these records after filtering" — reads more clearly than the imperative loop. The loop form is still correct; I'd only push for the change if we expect this pattern to recur in the module.

The teaching version:

1. Provides the alternative rather than demanding the author figure it out.
2. Explains *why* — idiom, readability, intent-revealing.
3. Calibrates severity — "I'd only push for the change if..." signals non-blocking.
4. Respects the author's judgement.

**Rule of thumb:** Every comment should answer either "what" or "why", ideally both. Comments that only say "what" feel like commands; adding "why" turns them into conversation.

### Q8. How do you give feedback on a PR where the overall approach is wrong?

**Answer:**

This is the hardest review situation. Line-by-line comments are wasted work if the whole approach is off. The cost of saying so, late, is the engineer's sunk work and morale.

**Step 1: Stop commenting inline.** Don't leave 40 nits on code that shouldn't exist.

**Step 2: Move to synchronous.** A video call or in-person conversation avoids the defensiveness of text. Tone travels badly in writing.

**Step 3: Lead with understanding.** "Walk me through the problem you're solving and why you took this approach." You may be wrong — the author may have context you lack.

**Step 4: Articulate the concern clearly.** "The approach works, but I'm worried about X. Here's why..." Frame as your concern, not their mistake.

**Step 5: Propose next steps collaboratively.** Options might include: shelving the PR and writing a short design doc; keeping part of the work; or you agreeing and approving after all.

**Prevention:** For large changes, review the design before the implementation. A 30-minute design discussion saves weeks of misdirected work. This is why ADRs and RFCs exist.

**STAR framing for interviews:**

> **S:** A teammate opened a 1,200-line PR introducing a new caching layer.
> **T:** I was the senior reviewer and realised the approach would conflict with our planned queueing redesign.
> **A:** I stopped commenting inline, booked a 30-minute call, walked through the approach with them, and suggested we pause to write an ADR. I volunteered to co-author it.
> **R:** The ADR surfaced that a different caching strategy would avoid the conflict. We reused ~40% of the original PR, landed it in three smaller pieces, and wrote down the decision for the next team that would face the same choice.

### Q9. What is reviewer bias and how do you mitigate it?

**Answer:**

Humans bring predictable biases to review. Naming them helps reviewers catch themselves.

| Bias | Manifestation | Mitigation |
|------|---------------|------------|
| **Author-name bias** | Stricter on juniors, lenient on seniors | Hide author name (some tools support this); pair reviewers; blind review |
| **Hindsight bias** | "I would have done it differently" dressed as feedback | Ask: would I block merge, or is this a preference? |
| **Anchoring** | First issue found colours perception of the whole PR | Do a full read-through before commenting |
| **Recency bias** | Recent incidents over-influence current review | Apply policies consistently, not reactively |
| **Not-invented-here** | Reject anything unfamiliar | Ask: is this objectively worse, or just different? |
| **Availability bias** | Focus on issues easy to articulate, miss harder design issues | Deliberately spend time on design before nits |
| **Fatigue** | Quality drops after many reviews in a day | Cap reviews per day; take breaks |

**Research anchor:** The "Demographics of Code Reviews" literature consistently shows women and minoritised engineers receive more critical and more pedantic reviews on equivalent changes. Teams serious about inclusion measure this — comment counts, approval rates, comment sentiment — and act on the data.

### Q10. A junior engineer disagrees with your review comment. How do you handle it?

**Answer:**

Disagreement is healthy. The response depends on what matters.

**Step 1: Assume they might be right.** Re-read your comment and their response. Seniors who hold every position regardless of new information burn junior engineers out.

**Step 2: Identify the type of disagreement.**

- **Factual:** One of you is wrong about how the code/language/system works. Resolvable with evidence.
- **Preference:** Both are valid. Lean toward the author unless there's a team standard.
- **Design:** Real judgement call. Worth a conversation.
- **Correctness:** If you're sure it's a bug, don't approve. Be explicit.

**Step 3: Calibrate your stance.**

- If it's preference, say so: "Either is fine — I'd pick X but won't block on it."
- If you're uncertain, say so: "I might be wrong here — can you walk me through why the lock is safe?"
- If you're sure, stay firm and explain *why*, not just *what*.

**Step 4: Escalate only when stuck.** Loop in a third reviewer or tech lead. Do this as "help us decide," not "tell them they're wrong."

**Key principle:** Juniors who have their objections overruled without consideration stop objecting. That's how teams accumulate tech debt and miss bugs.

### Q11. Describe a code review checklist you would actually use.

**Answer:**

Generic 40-item checklists are ignored. A practical checklist is short, team-specific, and revisited quarterly.

**Example for a backend service team:**

```markdown
## Review Checklist

### Correctness
- [ ] Does the code do what the description says?
- [ ] Error paths have tests or explicit "can't happen" comments
- [ ] Concurrency: any shared state is documented as such
- [ ] External calls have timeouts and retries where needed

### Observability
- [ ] New failure modes emit logs at appropriate levels
- [ ] New metrics (latency, errors, saturation) for new operations
- [ ] No PII in logs

### Security
- [ ] User input is validated/escaped at the boundary
- [ ] Secrets are from config/vault, not literals
- [ ] Authz checks on new endpoints

### Testing
- [ ] New behaviour has tests
- [ ] Tests describe behaviour, not implementation
- [ ] No flakiness (no sleeps, no real network)

### Rollout
- [ ] DB migrations are backwards-compatible
- [ ] Feature flags used for risky changes
- [ ] Runbook updated if operational behaviour changed
```

**Key design choices:**

- Group by concern, not by role.
- Each item is a yes/no — no wishy-washy "consider..." items.
- Fewer than 20 items — you can scan it without fatigue.
- Tied to things that have actually gone wrong for this team.

### Q12. When should a PR be approved vs. approved-with-comments vs. requested-changes?

**Answer:**

Teams need norms here, or reviewers calibrate differently and authors get whiplash.

**Approve** — you would be happy for this to merge as-is.

**Approve with comments (non-blocking)** — you trust the author to address or dismiss the comments; you don't need another review round. Use when comments are stylistic, minor, or follow-up work.

**Request changes (blocking)** — the PR should not merge until the comments are addressed. Use when:

- A correctness bug you're confident about.
- A security or compliance issue.
- The design or API is wrong and needs to change.
- You cannot understand the change enough to verify it — ask for clarification in the code or description.

**Avoid weaponising "request changes."** It's stronger than "comment" — use it when you mean it. Teams where every review blocks create review bottlenecks and frustrated authors.

**Interview insight:** Strong candidates describe this calibration explicitly. They know that review friction compounds — if every reviewer blocks every PR, velocity craters and people stop asking for review.

---

## Advanced

### Q13. How do you scale code review as a team grows from 3 to 30 engineers?

**Answer:**

Review practices that worked at 3 people break at 30. The failure modes are predictable.

**At 3 engineers:** Everyone reviews everything. Context is universal. Latency is low.

**At 10 engineers:** The original "senior" reviewers become bottlenecks. PRs sit for days. Junior engineers can't get work merged.

**At 30 engineers:** No one has full-system context. "Everyone reviews everything" becomes rubber-stamping.

**Scaling strategies:**

1. **Code ownership.** CODEOWNERS files (GitHub) or OWNERS files (Chromium-style) map directories to required reviewers. New hires don't need to know who to tag.

2. **Reviewer rotation / review queues.** Tools like Gerrit's reviewer-suggester, GitHub's auto-assignment, or internal load-balancers distribute reviews instead of always pinging the same senior.

3. **Readability review (Google model).** Separate "is this correct?" (domain expert) from "is this idiomatic?" (language expert). Decouple concerns.

4. **Tiered review depth.** Trivial changes (config, doc typos) auto-merge with a single light review. Risky changes (auth, billing) require two senior approvals plus a security reviewer.

5. **Invest in tooling.** Pre-commit hooks, linters, formatters, static analysers — everything machines can check should be checked before a human reads the diff. Every minute a human spends on style is wasted.

6. **Document standards.** If reviewers keep asking for the same thing, write it down. A style guide is cheaper than repeating the same feedback.

7. **Metrics.** Track review latency (time to first review, time to merge) and reviewer load. Teams with p95 review latency > 24 hours will see morale and velocity suffer.

**Reference:** *Software Engineering at Google* (chapter on Code Review) describes the readability-review system at scale. Michael Nygard's *Documenting Architecture Decisions* covers the decision-documentation half.

### Q14. What do you do when a senior engineer refuses to accept review comments from juniors?

**Answer:**

This is a culture problem dressed as a technical one. If unaddressed it destroys psychological safety and makes the team permanently slower.

**Diagnose first.**

- Is the senior dismissing with rationale, or dismissing without engagement? The former may be legitimate; the latter never is.
- Is it one junior, or all of them? All-juniors suggests status bias. One suggests a personality clash.
- Is this a new behaviour or a long-standing pattern?

**Interventions, in escalating order:**

1. **Name it privately.** "I've noticed you didn't engage with Priya's question on #1234. She was right about the N+1 query. I think there's a pattern here we should talk about."

2. **Ask them to mentor explicitly.** Sometimes the senior doesn't realise reviews are teaching moments. Assigning them as a mentor reframes the dynamic.

3. **Change the process.** Require two approvals for their PRs, one from someone more junior. This forces engagement.

4. **Loop in their manager.** If you're not their manager, give the manager the data. This is feedback, not tattling.

5. **Escalate if patterns persist.** If juniors are leaving or disengaging, this is a retention issue.

**As an interviewer this question probes:** Whether you see team dynamics as your problem. Senior candidates who say "that's a manager problem" are showing they don't take responsibility for team health. Staff+ candidates say "I'd own this" and describe the interventions.

### Q15. How do you measure whether your team's code review practice is healthy?

**Answer:**

You can't improve what you don't measure, but naive metrics create perverse incentives. Choose signals that are hard to game.

**Latency metrics:**

- **Time to first review.** Under 4 hours during working hours is healthy; over 24 hours is a bottleneck.
- **Time to merge.** From PR open to merge. Excludes author rework time ideally.
- **Round-trip count.** How many back-and-forth cycles? 1–3 is normal; 6+ suggests review is being used for design discussion (it shouldn't be).

**Quality metrics:**

- **Bugs caught in review vs. production.** Requires bug tagging. High review catch rate signals effective review.
- **PR revert rate.** How many merges get reverted? High rates suggest reviews aren't catching issues.
- **Change failure rate (DORA).** Of all deploys, how many fail? Correlates with review effectiveness.

**Culture metrics:**

- **Comment distribution.** If one reviewer writes 80% of comments, you have a bus factor problem.
- **Comment sentiment.** Some tools score this. Toxic reviewers are a retention risk.
- **Review load distribution.** Tracked per reviewer — if seniors are reviewing 20 PRs/week each, burnout is imminent.

**Anti-patterns to avoid:**

- **Counting comments.** Rewards pedantry.
- **Counting reviews performed.** Rewards rubber-stamping.
- **Lines-of-code reviewed per hour.** Rewards speed over depth.

**DORA reference:** *Accelerate* (Forsgren, Humble, Kim) and the DORA State of DevOps reports establish that change lead time and change failure rate correlate with organisational performance. Review latency is a major contributor to lead time.

### Q16. How do you review a PR in a language or codebase you don't know well?

**Answer:**

Senior engineers will regularly be asked to review outside their expertise — cross-team PRs, onboarding reviews, architecture cross-cuts. Strong candidates have a mental model for this.

**What you *can* still evaluate:**

- **PR description quality.** Is the problem stated? Are trade-offs discussed? Is the rollout plan sensible?
- **Test coverage.** Are new behaviours tested? Do tests test behaviours, not implementation?
- **Commit structure.** Atomic commits or one big mess?
- **Observability.** Does the change include logs/metrics/traces for the new paths?
- **Risk communication.** Does the author flag the risky parts?
- **API design.** Interfaces are often language-agnostic — are names clear? Is the contract minimal?

**What you should *not* assert:**

- Idiomaticity in a language you don't know.
- Library-specific correctness.
- Performance claims without evidence.

**Honest framing:**

> "I've left comments on the API design and rollout plan, which I can judge. I can't speak to Rust idiomatic style — I'd want someone from the platform team to sign off on the borrow-checker-heavy parts."

**Interview insight:** Admitting the limits of your review honestly is a senior behaviour. Pretending to review competently what you can't is worse than declining the review.

### Q17. When should code review be replaced or augmented by pair programming?

**Answer:**

Pair programming is continuous, synchronous review. It's not a strict replacement — each has different strengths.

| Aspect | Async Code Review | Pair Programming |
|--------|-------------------|------------------|
| Latency | Hours to days | Instant |
| Cost | One reviewer at review time | Two engineers full-time |
| Depth | Limited by diff context | Full context, live |
| Knowledge spread | Moderate | Strong |
| Onboarding | Slower | Faster |
| Deep-work compatibility | Good | Poor |
| Audit trail | Strong (written comments) | Weak (verbal) |
| Works across time zones | Yes | Requires overlap |

**When to lean on pair programming:**

- Onboarding new hires on complex systems.
- High-risk changes (security, data migration, concurrency-heavy code).
- Cross-skill pairing (senior teaching junior, or vice versa on new tech).
- Spike/exploration work where design evolves as you write.

**When async review is better:**

- Distributed or async-first teams.
- Work that needs deep thinking time.
- Changes where a durable written record (for audit, for future context) matters.

**Hybrid models:**

- Pair on the design and initial implementation; async-review the final diff.
- "Ensemble" or "mob" programming for critical changes.
- Async review by default; pair when a PR stalls in review.

**Reference:** *Extreme Programming Explained* (Beck) is the canonical source on pairing. *Software Engineering at Google* discusses the hybrid model.

### Q18. How do you review for security issues specifically?

**Answer:**

Most engineers undertrain on security review. A senior should have a lightweight mental checklist specific to the codebase's risks.

**Generic checklist:**

- **Input validation at trust boundaries.** Anything from a client, external service, or user.
- **Output encoding.** HTML escaping, JSON serialisation, log sanitisation.
- **Authorisation checks.** Not just authentication — does *this user* have permission for *this action* on *this resource*?
- **Secrets handling.** No hardcoded credentials, API keys, or tokens. Vault/KMS usage is correct.
- **PII handling.** Logs, metrics, error messages — are we leaking?
- **Dependency hygiene.** Are new dependencies from trusted sources? Are versions pinned?
- **Cryptography.** Never roll your own. Flag anything that looks hand-written.

**Framework-specific signals:**

- **SQL:** Parameterised queries, not string concatenation.
- **Web apps:** CSRF tokens, Content Security Policy, secure cookies, SameSite.
- **APIs:** Rate limiting, request size limits, timeout propagation.
- **Serialisation:** Never deserialise untrusted data into objects (Python `pickle`, Java `ObjectInputStream`, YAML `load`).

**When you're uncertain, loop in security.** Most orgs have a security team or a security-champion programme. A senior engineer knows when to escalate rather than approve based on hope.

**OWASP Top 10** is the interview-mentionable reference: injection, broken auth, sensitive data exposure, etc. Citing it shows awareness without needing to memorise specifics.

### Q19. Describe a time you received harsh code review feedback. What did you learn?

**Answer (STAR template):**

This is a recurring behavioural question. Interviewers test self-awareness and growth mindset — candidates who claim they've never received harsh feedback are flagged as either inexperienced or defensive.

**STAR structure:**

> **S:** Early in my career, I submitted a refactor of a payments module. The review came back with 40+ comments from a senior engineer, several phrased bluntly.
>
> **T:** I had to decide whether to push back, comply silently, or engage seriously.
>
> **A:** My first reaction was defensive. I took a day away from it. Re-reading the comments, I separated them into: factually correct (most), style preference (some), and genuinely dismissive (a few). I addressed the first two categories in code. For the tone issues, I booked a 1:1 with the reviewer, told them the feedback was useful but the delivery made it harder to absorb, and asked for examples of what good feedback looked like to them. I also asked my manager for feedback-giving and feedback-receiving training.
>
> **R:** The PR landed better than my original. I became more defensive about my own future PRs — writing fuller descriptions, flagging risk explicitly, self-reviewing before sending. A year later I was reviewing juniors' code and consciously chose to pair frankness with empathy. The reviewer and I still work together; we're both better at this.

**What the answer demonstrates:**

- Self-awareness (I was defensive).
- Sorting signal from noise (three categories).
- Courage (raising the tone issue directly).
- Growth (changed my own behaviour; applied it to others).
- No blame — the reviewer's critique was correct, the style was a separate issue.

### Q20. What do you do when CI passes but you don't trust the test suite?

**Answer:**

A green CI is a necessary but not sufficient condition. Senior engineers know when to not trust it.

**Signals that CI is less informative than it looks:**

- **Low coverage** on the changed code (not the whole project).
- **No tests** added for new behaviour.
- **Tests that don't actually assert** (common with mocking-heavy code).
- **Tests that mock the behaviour under test** (classic anti-pattern).
- **Flaky tests** that pass on retry — you don't know if this one passed because the code works or because the flake didn't trigger.
- **Integration tests absent** for a change that crosses service boundaries.
- **Perf regression** — unit tests pass but the change is 10x slower.

**What to do:**

1. **Comment in the review.** "I'd like to see a test for the error path in `handle_timeout`. What does the code do today if the downstream returns 429? Let's prove it."
2. **Ask the author to describe the manual testing.** Sometimes a good answer exists — not every change is fully automatable.
3. **Run it yourself in a staging environment.** Especially for changes you're the last reviewer on.
4. **Block merge until the test gap is filled.** This is what "request changes" exists for.

**Anti-pattern to avoid:** Approving because CI passes, then pointing at CI when it breaks in production. Reviewers share ownership of what merges.

### Q21. How do you handle the "LGTM from me" problem — rubber-stamping?

**Answer:**

Rubber-stamping is the silent failure of code review culture. Reviews happen, boxes are ticked, but no real scrutiny occurs. It's hard to detect because from metrics it looks healthy.

**Root causes:**

- **Time pressure.** Reviewers are over-loaded and skim.
- **Seniority asymmetry.** Reviewers outrank authors and feel awkward pushing back (or the reverse — juniors rubber-stamp senior PRs).
- **Social comfort.** You review your friends' code less critically.
- **Repetition.** The 7th similar PR in a sprint gets less attention.
- **Lack of context.** Reviewer doesn't understand the change enough to comment meaningfully.

**Interventions:**

- **Rotate reviewers.** Same two people reviewing each other forever → rubber-stamping.
- **Require two reviewers** for risky changes. One may rubber-stamp; two rarely both will.
- **Sampling / audit.** Periodically, a third party reviews a sample of merged PRs and asks "would you have approved this?".
- **Diff-hostile PRs.** If a PR is so large it can't be reviewed carefully, reject it as "please split."
- **Recognise good review.** If reviewing well is invisible work and coding is visible work, people stop reviewing well. Celebrate good reviews in public channels.

**Self-check:** If you approved something in under 60 seconds, did you understand it? If not, don't approve it. Say "I can't review this deeply right now — try @alice, she has context."

**Interview insight:** The candidate who names this problem without prompting signals review-culture maturity. Most engineers treat rubber-stamping as inevitable rather than as something to actively prevent.

### Q22. What cultural norms make code review effective across a distributed, time-zone-spread team?

**Answer:**

Async-first teams succeed or fail based on review culture. There's no water-cooler recovery when a review goes wrong.

**Critical norms:**

1. **Thorough PR descriptions are mandatory.** A PR description that requires a synchronous conversation to understand will stall for 24 hours across time zones. Templates enforce this.

2. **Self-review before requesting review.** Reviewers on the other side of the planet should not be finding debug prints.

3. **Explicit blocking/non-blocking status.** Across time zones, ambiguity costs a full cycle. `nit:`, `suggestion:`, `blocking:` prefixes are essential.

4. **Reviewer SLAs.** "First response within one working day of the reviewer's time zone" is reasonable. Longer than that, escalate or reassign.

5. **Asynchronous-by-default, synchronous-when-needed.** Know when to stop a thread and book a call. Some arguments resolve in 10 minutes live that take a week in writing.

6. **Written tone matters.** No sarcasm. No "obviously" or "just." Read comments back imagining they're from someone more senior than you — does it still feel respectful?

7. **Documented decisions.** If a review conversation changes direction, summarise the outcome in the PR or an ADR. Don't make future engineers reconstruct from 40 scrollback comments.

8. **Multiple valid reviewers.** If only one reviewer has context, one person's PTO stalls the team. Invest in context-spreading explicitly.

**Reference:** GitLab's Handbook (publicly available) and the *Async work* literature document these norms. GitLab's own culture is worth reading as a prepared example.
