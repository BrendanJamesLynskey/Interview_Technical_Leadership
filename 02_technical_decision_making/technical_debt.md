# Technical Debt — Interview Questions

**Subject:** Technical Leadership
**Topic:** Identifying Debt, Prioritisation, Debt Quadrant, Stakeholder Communication
**Difficulty tiers:** Fundamentals / Intermediate / Advanced

---

## Fundamentals

### Q1. What is technical debt?

**Answer:**

The metaphor was coined by Ward Cunningham in 1992 to explain why teams should ship "not quite right" code to learn, then invest in correcting it — similar to financial debt taken strategically and repaid with interest.

**Cunningham's original meaning:**

> "Shipping first-time code is like going into debt. A little debt speeds development so long as it is paid back promptly with a rewrite... The danger occurs when the debt is not repaid. Every minute spent on not-quite-right code counts as interest on that debt."

**Modern expanded meaning:**

Technical debt is anything in the system that slows future change. It includes:

- Code that works but is hard to modify safely.
- Architecture that fits today but not tomorrow's requirements.
- Missing tests that require manual verification.
- Outdated dependencies that block upgrades.
- Undocumented decisions that have to be rediscovered.
- Workarounds for bugs in other systems.
- Coupling that prevents independent evolution.

**Key properties:**

- **Interest compounds.** Debt that isn't paid down becomes more expensive over time.
- **Interest is paid in velocity.** You don't see a bill — you notice that every feature takes longer than it should.
- **Some debt is strategic.** Shipping a prototype, validating a market, hitting a deadline — these can justify debt, if there's a plan to repay.
- **Some debt is accidental.** Created by inexperience, poor review, or bad tooling. No intent, no plan.

**Interview insight:** Candidates who conflate "bad code" with "technical debt" reveal a simplistic view. Debt is an economic concept — it implies deliberate trade-off and future repayment. Code that's bad because no one cared is not debt; it's negligence.

**Reference:** Cunningham's original "WyCash Portfolio Management System" paper (1992), accessible via the C2 wiki.

### Q2. What is the technical debt quadrant?

**Answer:**

Martin Fowler's **technical debt quadrant** (2009) separates debt by two axes:

- **Deliberate vs Inadvertent** (did we know at the time?)
- **Reckless vs Prudent** (did we think it through?)

|                  | Reckless                                | Prudent                                   |
|------------------|-----------------------------------------|-------------------------------------------|
| **Deliberate**   | "We don't have time for design."        | "We must ship now and deal with consequences." |
| **Inadvertent**  | "What's layering?"                       | "Now we know how we should have done it." |

**Examples:**

- **Deliberate-Reckless.** Rushing out code knowing it's bad, without planning to fix it. Technical negligence dressed as pragmatism.
- **Deliberate-Prudent.** Shipping a minimum viable implementation to validate a market, with an explicit plan to revisit if the product proves viable.
- **Inadvertent-Reckless.** Unaware of the standards or patterns that would have helped. Junior engineers without guidance.
- **Inadvertent-Prudent.** You did your best with what you knew; hindsight reveals a better approach.

**Why the quadrant matters:**

- **Different debts need different responses.** Deliberate-prudent debt should be tracked and repaid. Inadvertent-prudent debt is learning.
- **Reckless debt is a cultural/hiring/process problem.** Not just a code problem. If deliberate-reckless is common, the incentives are wrong.
- **In conversations with stakeholders, naming the quadrant changes the discussion.** "This is deliberate-prudent debt we took on last year" is different from "we have some bad code we need to fix."

**Reference:** Martin Fowler, "Technical Debt Quadrant" (2009), martinfowler.com/bliki/TechnicalDebtQuadrant.html.

### Q3. How do you identify technical debt in a codebase?

**Answer:**

Debt is partly quantitative, partly qualitative. A senior engineer uses multiple lenses.

**Quantitative signals:**

- **Code complexity.** Cyclomatic complexity, cognitive complexity, deep nesting. Tools: SonarQube, radon, lizard, Code Climate.
- **Test coverage.** Not as a single number — coverage of modified lines matters more.
- **Lint/warning counts.** Are there 200 ignored warnings?
- **Dependency age.** How many major versions behind are your dependencies? Any with known CVEs?
- **Build time.** Slow builds indicate architectural bloat.
- **Deploy frequency per service.** Services that rarely ship often indicate change aversion from debt.
- **Bug density.** Bugs per KLOC, per module, over time. Where are they clustered?
- **Hotspot analysis.** Files that are both frequently changed and complex (Adam Tornhill's *Code as a Crime Scene*).

**Qualitative signals:**

- **Developer survey.** "Rate on 1–5: how easy is it to change this module?"
- **Review complaints.** PRs that consistently generate anxious reviewer comments.
- **On-call pain.** Which systems generate the most incident traffic?
- **Onboarding friction.** What do new hires find confusing? They see clearly what insiders have normalised.
- **Surprise factor.** How often does changing one thing break something unrelated?
- **"We don't touch that."** Any area engineers are afraid of is debt.

**Combined view:**

- The highest-priority debt sits at the intersection — high-complexity + high-change + high-bug-density + team-reported-pain.
- Quantitative alone misses context; qualitative alone is subjective. Use both.

**Anti-pattern:** Debt registers composed of "things that annoyed engineer X in October." Rigour comes from multiple signals pointing at the same area.

### Q4. How do you measure the "interest" on technical debt?

**Answer:**

Cunningham's metaphor only works if interest is real. You can't justify repayment without quantifying the cost of not repaying.

**Signals of debt interest:**

1. **Velocity on debt-heavy areas.** Story points (or equivalent) per unit of time, compared to cleaner areas.
2. **Time-to-ship on changes in the area.** First commit to production — in hours, days, or weeks.
3. **Review cycles.** Debt-heavy PRs take more rounds. Track this if review tools allow.
4. **Bug rate post-change.** Debt-heavy changes break things more often.
5. **Onboarding time.** New hires productive in debt-heavy areas vs clean areas.
6. **Engineer sentiment.** "How confident are you changing this module?" on a periodic survey.
7. **On-call burden.** Pages per module per quarter.

**Translating to money:**

- **Engineer-hours spent fighting the debt.** Rough: 10 engineers × 20% of time lost to debt friction × fully-loaded cost = material number.
- **Opportunity cost.** Features not shipped because engineers are stuck fighting debt.
- **Incident cost.** Outages attributable to unpaid debt, with direct revenue impact.

**Example framing for stakeholders:**

> "Our pricing module has a change-failure-rate of 22% versus 8% elsewhere. We estimate 30% of pricing-team engineering time is lost to debt friction. That's roughly £300k/year in engineer cost alone, not counting customer-visible incidents."

**Why this works:** Engineering leaders are comfortable with quantified discussions. Unquantified complaints about "bad code" are harder to action.

**Interview insight:** Candidates who talk about debt in monetised or velocity-measured terms signal senior-level thinking. "The code is ugly and we should fix it" does not.

### Q5. How do you differentiate good code-quality practice from over-engineering?

**Answer:**

The dividing line is "is this solving a known problem or an imagined one?"

**Good code-quality practice:**

- **Addresses demonstrated pain.** "We've had three bugs from concurrent access to this map; let's add proper synchronisation."
- **Solves a current requirement.** "This needs to handle 10x traffic by Q3 — here's a proportionate design."
- **Enables upcoming known work.** "We'll be adding payment methods monthly; an extensible structure is warranted."
- **Correct for the stage.** Startup MVP vs enterprise platform vs regulated fintech require different baselines.

**Over-engineering:**

- **Solves speculative problems.** "We might need to support 1M users." (Are you at 1k?)
- **Premature abstraction.** Interfaces with one implementation; strategy pattern for one strategy; factory for one product.
- **Framework creation for minor duplication.** Rule of three before extracting.
- **Architecture shopping.** Choosing microservices for résumé reasons.
- **Gold-plating.** Features and configurability no one asked for.

**YAGNI ("You Aren't Gonna Need It") as heuristic:**

- Build for what you need now.
- Structure so you *can* extend, but don't build the extension.
- "I added the abstraction when the second case arrived" is a stronger story than "I designed for 10 cases and got 1."

**Trade-off honesty:**

- Under-engineering saves short-term cost; pays long-term cost in rewrites.
- Over-engineering pays short-term cost in complexity; rarely pays off because the imagined future doesn't arrive.
- Net: over-engineering is more common than under-engineering and more expensive.

**Interview framing:** "I try to build for the problem I have, with structure that won't block the problems I can see coming. Not the imagined problems of five years out."

**Reference:** Kent Beck on YAGNI (Extreme Programming writings). *A Philosophy of Software Design* (John Ousterhout) discusses the trade-off between depth and flexibility.

### Q6. What are the different "types" of technical debt?

**Answer:**

Debt is not one thing. A useful taxonomy helps prioritise:

**Code debt.** Poor structure, duplication, outdated patterns within the code itself.

**Design/architectural debt.** Choices at module and service level that no longer fit. Tight coupling, wrong service boundaries, missing abstractions.

**Test debt.** Missing coverage, flaky tests, tests that test implementation rather than behaviour, brittle fixtures.

**Documentation debt.** Missing ADRs, outdated READMEs, undocumented tribal knowledge.

**Infrastructure debt.** Outdated tooling, slow CI, manual deploy steps, configuration drift, unsupported platforms.

**Dependency debt.** Libraries on old versions, transitive dependencies with CVEs, reliance on unmaintained packages.

**Knowledge debt.** Bus factor of 1 on critical systems; onboarding friction; missing runbooks.

**Process debt.** Manual steps that should be automated; missing deploy gates; ad-hoc incident response.

**Data debt.** Duplicated data; schema drift; PII concerns; lack of lineage.

**Why this matters:**

- **Different debts have different owners and different repayment mechanics.** Code debt is developer time; infrastructure debt may need platform team.
- **Prioritisation differs.** Security CVE debt is more urgent than cosmetic code debt.
- **Communication with stakeholders is easier when specific.** "We have test debt in the checkout module" is actionable; "we have technical debt" is not.

**Interview insight:** Candidates who enumerate debt types show they've thought about the problem. Candidates who reduce everything to "bad code" have narrower experience.

---

## Intermediate

### Q7. How do you prioritise paying down technical debt?

**Answer:**

Debt repayment has opportunity cost — every hour on debt is an hour not on features. Rigorous prioritisation is essential.

**Prioritisation factors:**

1. **Interest rate.** How much does this debt cost per week? High-interest debt is urgent.
2. **Blast radius.** How much of the system is affected? Code debt in a hot module > cold module.
3. **Blocker potential.** Does this debt prevent planned work? Migration blockers are high priority.
4. **Risk asymmetry.** Does this debt have catastrophic failure modes? Security debt often does.
5. **Cost of repayment.** High-value, low-cost wins first. The 80/20 applies.
6. **Optionality.** Repaying debt now opens doors later; delaying narrows them.

**Prioritisation frameworks:**

**ICE (Impact, Confidence, Ease):** Score each debt item 1–10 on each axis, multiply. Works for fast-moving teams.

**RICE (Reach, Impact, Confidence, Effort):** (Reach × Impact × Confidence) / Effort. Used by Intercom and others.

**Weighted Shortest Job First (WSJF):** Cost-of-delay divided by job size. From SAFe but useful outside it.

**Matrix-based prioritisation:**

|                 | High impact | Low impact |
|-----------------|-------------|------------|
| **Low cost**    | Do now      | Maybe      |
| **High cost**   | Plan carefully | Skip    |

**Practical rules of thumb:**

- **Always do** the tiny wins. A single-day refactor with lasting benefit should not wait for a quarter planning cycle.
- **Bundle with features.** Debt in areas you're touching for features is cheap to repay.
- **Block scheduled time** for bigger items. 10–20% capacity is a common heuristic.
- **Use incidents as forcing functions.** Post-incident action items often fund debt repayment that wouldn't otherwise be approved.

**Anti-pattern:** "Let's do a technical debt sprint" followed by no structural prioritisation. Without filters, the loudest complainer's debt wins.

**Reference:** *Accelerate* (Forsgren et al.) on capacity allocation. *The Goal* (Goldratt) on theory-of-constraints — fix the bottleneck first.

### Q8. How do you communicate technical debt to non-technical stakeholders?

**Answer:**

Most stakeholders don't speak "refactoring." They speak deadlines, revenue, risk. Translate.

**Principles:**

1. **Use business language.** Debt reduces velocity; reduced velocity delays features; delayed features lose revenue or competitive position.

2. **Quantify where you can.** "We estimate 25% of engineering time in this area goes to fighting debt" is more persuasive than "the code is complicated."

3. **Frame as risk.** "This dependency has known vulnerabilities and goes out of support in 6 months" speaks to risk managers.

4. **Tie to specific business outcomes.** "Without addressing this, we can't ship the new checkout flow" is actionable.

5. **Show options and trade-offs.** Do nothing (what happens?), do X (cost, benefit), do Y (cost, benefit). Let them decide.

6. **Avoid jargon.** "Refactor," "coupling," "abstraction" land poorly. "Make it easier to change," "separate concerns," "clean up" land better.

**Example framing:**

> **Weak:** "We have a lot of technical debt in the billing module that we need to address."
>
> **Strong:** "Changes to the billing module take 3x longer than they did a year ago, and we've had two customer-visible incidents this quarter traceable to the same root cause. Without investment, we estimate we'll miss the Q3 pricing launch. We've scoped two options: a 6-week cleanup (80% confidence we hit Q3) or a 3-week patch (50% confidence, debt increases). I recommend the cleanup."

**What makes this work:**

- Measurable claim.
- Link to customer impact.
- Link to business deadline.
- Clear options with probabilities.
- Recommendation with reasoning.

**Interview insight:** The ability to frame technical concerns in business language is what distinguishes staff engineers from senior engineers. Practice this translation — it's a muscle.

**Reference:** *The Art of Influence Without Authority* (Bradford & Cohen). *Staff Engineer: Leadership Beyond the Management Track* (Will Larson).

### Q9. When is it correct to take on technical debt deliberately?

**Answer:**

Deliberate debt is legitimate — the original Cunningham metaphor is about deliberate, strategic debt. Taking on debt is a judgement call, not a sin.

**Good reasons to take on debt:**

1. **Market timing.** Ship to validate a hypothesis. If the product fails, the "right" implementation would have been wasted. If it succeeds, you have revenue to fund the rewrite.

2. **Learning.** You don't yet know the right design. Ship the naive version; learn from production; redesign with knowledge.

3. **External deadlines.** Regulatory, competitive, partnership — some deadlines are fixed. Debt can be the cost of meeting them.

4. **Cost-of-delay dominates cost-of-debt.** If every week of delay costs more than the eventual repayment, debt is economically rational.

5. **Uncertain future.** If the module may be replaced or the feature sunset, investment doesn't pay off.

**Conditions that make debt legitimate:**

- **It's deliberate, not accidental.** The team knows the trade-off and agrees.
- **It's documented.** An ADR or tech-debt register records what was skipped and why.
- **It has a repayment plan.** Not necessarily a date, but conditions under which it should be addressed.
- **It's bounded.** The damage doesn't spread. The debt is in a contained area.

**Conditions that make debt dangerous:**

- **It's reckless.** No plan, no budget, no awareness.
- **It's permanent.** "We'll fix it later" where later never arrives.
- **It's contagious.** Debt in one area forces debt in neighbouring areas.
- **It compromises safety-critical properties.** Security, data integrity, correctness in financial flows.

**Interview framing:** "I take on debt deliberately when the business case is clear, I document it, and I own the repayment plan. I push back on reckless debt — debt taken without acknowledgment is the kind that accumulates and kills teams."

### Q10. How do you maintain a "tech debt register" and is it worth it?

**Answer:**

Tech debt registers are attempted by most teams. Most fail. The failure modes are predictable.

**Why most registers fail:**

- **Write-only.** Items added, never reviewed.
- **Growing without bound.** 300 items, all important, none actionable.
- **Misses real debt.** Tribal knowledge never gets written down.
- **No owner.** Items in limbo forever.
- **Not tied to planning.** Register is parallel universe from the roadmap.

**What makes a register work:**

1. **Regular review cadence.** Quarterly pruning. Items not touched in 6 months: close them or elevate them.
2. **Each item has an owner.** Not just a reporter.
3. **Each item has rough cost and rough impact estimates.** Prioritisation needs data.
4. **Integrated with planning.** Every quarter, some register items get into the plan.
5. **WIP limit.** Maximum 30-50 active items. More = signal that the team is drowning.
6. **Closure criteria.** What does "done" look like for each item?

**Alternative approaches:**

- **Debt issues in the main tracker.** Same tool as features, with a tag/label. Forces the trade-off explicit at planning.
- **"Tech debt Friday"** or similar bounded time.
- **Tactical register + strategic themes.** Individual items in the tracker; high-level debt themes in an architecture doc.

**What to write for each item:**

```markdown
### [Debt] Refactor order-queue to use shared abstraction

**Category:** Architecture
**Owner:** @alice
**Severity:** Medium (blocks queue migration in Q3)
**Cost of repayment:** ~2 weeks, one engineer
**Cost of not repaying:** Adds ~1 week to each queue-related feature
**Proposed approach:** See ADR-073

**Status:** Accepted, scheduled for Q2
```

**Interview insight:** Candidates who describe debt registers that actually work share details like review cadence, WIP limits, and closure criteria. Candidates who describe "we have a Jira board" reveal they haven't thought past setup.

### Q11. How do you handle debt that crosses team boundaries?

**Answer:**

Debt in a single team's codebase is hard. Debt in a shared module is the hardest organisational problem in debt management.

**Why cross-team debt festers:**

- **No single owner.** "Everyone's responsibility" = no one's responsibility.
- **Change breaks others.** Fixing it requires coordination with teams who may not prioritise.
- **Incentive mismatch.** The fixing team bears cost; benefits go to all.
- **Conflict aversion.** Nobody wants to be the team that "slowed everyone down" for cleanup.

**Interventions:**

1. **Clarify ownership.** Often shared code ends up owned by a platform team, or rotated ownership. Ambiguity kills.

2. **Social contracts.** Shared library changes follow a contract — deprecation period, migration guide, support window.

3. **Coordinate migrations.** Run as a mini-program: a shared plan, named owner per consumer team, weekly check-ins.

4. **Carrot over stick.** Help consumer teams migrate; don't just throw work over the fence. Migration PRs against their repo, not just an email.

5. **Raise to cross-team forum.** Architecture review board, platform council — whatever forum exists for cross-team technical decisions.

6. **Sunset date, not sunset hope.** "We'll migrate when we can" never happens. "Old interface goes away 2026-06-30" focuses minds.

**Example scenario:**

> Shared `auth` library is 3 major versions behind. Three consumer teams. None willing to migrate alone. Debt compounds.

> **Resolution:** Platform team takes ownership. Publishes migration guide. Commits to writing the first consumer team's migration PR as proof of concept. Sets a sunset date 6 months out. Offers office hours. After 4 months, two consumers are migrated; the third lags. Weekly cross-team standup ensures visibility. Third consumer migrates in month 5.

**Interview insight:** Cross-team debt is a leadership test, not a coding test. Answers that emphasise coordination, incentive alignment, and explicit contracts score higher than answers about the technical work alone.

**Reference:** *Team Topologies* (Skelton & Pais) on stream-aligned vs platform vs enabling teams. *An Elegant Puzzle* (Will Larson) on migrations.

### Q12. What is "architectural fitness function" and how does it help manage debt?

**Answer:**

An **architectural fitness function** (Building Evolutionary Architectures, Ford/Parsons/Kua) is an automated check that verifies an architectural property holds. It's the way you prevent debt from returning after you pay it down.

**Examples:**

- **Test coverage cannot decrease on changed code.**
- **No code in module A may import from module B.** (Enforces module boundary.)
- **No function exceeds cyclomatic complexity 15.**
- **New public API methods require docstrings.**
- **Build time stays under 10 minutes.**
- **p99 latency on critical paths stays under 100ms** (from production monitoring).

**How to implement:**

- **Static analysis.** Linters, custom AST checks, dependency graph verifiers.
- **CI gates.** Fail the build if a fitness function violates.
- **Continuous monitoring.** Production metrics as fitness functions — SLOs.
- **Manual architecture reviews.** For properties too nuanced to automate.

**Why this matters for debt:**

- **Debt repayment without fitness functions is temporary.** You clean up; a year later, you're back.
- **Fitness functions encode the lessons.** What you fixed today, you prevent tomorrow.
- **They make architecture decisions enforceable.** ADR + fitness function is the enforcement pair.

**Example in Python:**

```python
# import-linter config
[importlinter]
root_packages = myapp

[importlinter:contract:layers]
name = Layered architecture
type = layers
layers =
    myapp.web
    myapp.services
    myapp.domain
    myapp.infrastructure
# Fails CI if higher layers import from lower incorrectly
```

**Example in multiple languages via Semgrep:**

```yaml
rules:
  - id: no-http-in-domain
    patterns:
      - pattern: requests.get(...)
    paths:
      include:
        - "myapp/domain/"
    message: Domain layer must not make HTTP calls; use repository.
    severity: ERROR
```

**Interview insight:** Fitness functions are a senior/staff-level concept. Candidates who name them (or describe the idea by any name) signal familiarity with *Building Evolutionary Architectures* or equivalent thinking.

---

## Advanced

### Q13. Describe a time you successfully paid down significant technical debt. Walk through the approach.

**Answer (STAR template):**

> **S:** Our notification service had grown organically over five years. It supported email, SMS, push, and webhooks via a single class with 4,000 lines of branching logic. Adding a new channel took roughly 3 weeks; every change carried regression risk in other channels.
>
> **T:** I was tech lead on the team. The backlog had three new channel integrations queued; at the existing pace, they'd miss the business commitment.
>
> **A:**
> 1. **Quantified the pain.** 40% of PRs to the service required more than 3 review rounds. Change failure rate was 18%. Channel-specific changes caused regressions in other channels 2–3 times per quarter.
> 2. **Wrote an ADR** proposing a channel-plugin architecture. Each channel becomes a separate module implementing a common interface. Explicit non-goal: change the public API.
> 3. **Got stakeholder buy-in.** Framed as "ship three channels on time with 40% less risk, with 6 weeks of upfront investment." Product agreed.
> 4. **Characterisation tests first.** Two engineers spent a week writing tests that pinned current behaviour across channels. Coverage on the service went from 35% to 78%.
> 5. **Strangler Fig migration.** Extracted email first — highest-traffic channel. Feature-flagged the new path; shadow-compared output for two weeks; flipped; monitored. No regressions. Then SMS, push, webhooks in sequence.
> 6. **Added fitness functions.** Import-linter rule: channel modules cannot import each other. Test: every channel module must implement the full interface. CI enforces.
> 7. **Shared the learning.** Wrote a postmortem-style doc on what worked, presented at engineering all-hands.
>
> **R:** Migration took 7 weeks total against a 6-week estimate. Time-to-add-channel dropped from 3 weeks to 4 days. Change failure rate in notifications dropped from 18% to 6%. The three new channels shipped on schedule. Two engineers who worked on the migration each led similar migrations the following year, using the playbook.

**What this demonstrates:**

- Business framing.
- Quantified before/after.
- Technique literacy (strangler fig, characterisation tests, fitness functions).
- Ownership of stakeholder communication.
- Scaling through knowledge sharing.
- Honest estimate-vs-actual (7 weeks against 6).

### Q14. What do you do when leadership refuses to allocate time for debt repayment?

**Answer:**

This is a common and important test. Strong candidates neither concede quietly nor fight unproductively.

**Diagnose first:**

1. **Is the refusal informed?** If leadership doesn't understand the cost of the debt, the problem is communication. Go back to Q8.
2. **Is the refusal a priority call?** If they understand and genuinely believe other work matters more, accept it. They may be right.
3. **Is the refusal reflexive?** Some orgs structurally never approve debt work. That's a cultural issue.

**Tactics when refusal is based on incomplete information:**

- **Quantify in business terms.** Show the interest cost.
- **Propose bounded, measurable experiments.** "Give me 2 weeks; here's the metric I'll move."
- **Link to near-term outcomes.** "This debt blocks Q3's feature. Paying it down now saves us in Q3."
- **Use the next incident as forcing function.** When debt causes an outage, post-incident action items can fund repayment.

**Tactics when leadership is reasonably saying no:**

- **Work debt into feature work.** Opportunistic repayment doesn't need approval.
- **Scout Rule.** Each PR leaves the area slightly better.
- **Pick battles.** You won't win them all. Pick the most important.

**Tactics when the culture is the problem:**

- **Build coalition.** Find other senior engineers and managers who share concerns.
- **Make the invisible visible.** Dashboards, metrics, monthly reports on debt impact.
- **Escalate data, not complaints.** "Engineering velocity in module X has dropped 40%" is data.
- **Reconsider the role.** If leadership fundamentally won't invest in engineering health, career risk rises over time.

**The honest answer in interviews:**

> "I'd start by improving my communication. If they still decline after I've made the case clearly in business terms, I'd work debt into feature work opportunistically. I'd also use incidents as forcing functions. If none of that worked and the culture structurally refused engineering investment, I'd consider whether the environment is one I want to stay in long-term — that's valuable data too."

**Interview insight:** Candidates who pretend they always get what they want are unconvincing. Candidates who describe working within constraints while advocating for change — and being honest about what to do if nothing changes — show maturity.

### Q15. How do you avoid creating technical debt while shipping quickly?

**Answer:**

Speed and quality are not always opposites. Beck's "Make it work, make it right, make it fast" sequences them — but the "make it right" step is non-optional even under pressure.

**Practices that enable speed without creating debt:**

1. **Automated testing.** Tests mean you can ship quickly *and* safely. They are the opposite of debt.

2. **Trunk-based development with short-lived branches.** Small, frequent merges reduce integration debt.

3. **Feature flags.** Ship infrastructure first behind a flag; enable when ready. No "big bang" launches.

4. **Continuous deployment.** Small, frequent releases. Debt can't hide in large batches.

5. **Pair/mob programming for high-stakes work.** Second pair of eyes catches the shortcuts tired brains take.

6. **Strong types and strict linters.** Catch categories of bugs at author-time, not review-time.

7. **Good abstractions already in place.** New features that plug into a clean design ship fast *and* clean. Prior investment pays off.

8. **Code review that actually happens.** Review catches the shortcuts.

**Practices that create debt even when shipping quickly:**

- Disabling tests to make CI green.
- Commenting out assertions.
- Catching and ignoring exceptions.
- Copy-pasting instead of refactoring to shared code.
- Ignoring warnings.
- Deferring monitoring and observability.

**The paradox of pressure:**

Under pressure, teams feel they "can't afford" quality practices and cut them. The result is slower shipping, because debt compounds. The data in *Accelerate* (Forsgren et al.) shows this empirically: high-performing teams ship *faster* and with *lower* defect rates, because they maintain quality practices.

**Interview framing:**

> "I push back on 'we don't have time for tests.' Not having tests doesn't save time; it just moves time from now to later, with interest. If the deadline is real and we have to cut something, I'd cut scope before cutting tests."

**Reference:** *Accelerate* on the correlation between quality practices and velocity. *Working Effectively with Legacy Code* (Feathers) on the compounding cost of skipping tests.

### Q16. What is the concept of "technical debt compound interest" and how do you explain it to stakeholders?

**Answer:**

Debt interest doesn't just accumulate linearly. It compounds — debt in one area makes debt in adjacent areas more likely, and existing debt makes new features harder to add cleanly.

**Mechanisms of compounding:**

1. **Debt begets debt.** Adding features to a debt-heavy area requires shortcuts (you can't refactor first when the code is incomprehensible). New shortcuts add more debt.

2. **Broken-windows effect.** Teams develop lower standards when surrounded by debt. "The whole area is a mess anyway" licenses more mess.

3. **Onboarding cost increases.** New engineers in a debt-heavy codebase take longer to become productive; some leave. Remaining team's load increases; they accumulate more debt.

4. **Change aversion.** Teams become afraid to change debt-heavy code. Necessary changes get deferred. Deferred changes become larger and riskier.

5. **Dependency rot.** Old dependencies can't be updated because they break debt-filled code. New features require new dependencies. You end up with two versions of everything.

6. **Knowledge loss.** As the team turns over, the "why" behind debt is lost. What was once "we know this is ugly but it was necessary" becomes "no one knows why this is like this."

**Stakeholder framing:**

> "Technical debt is like financial debt — but worse, because the interest rate itself compounds. Module X's debt today adds roughly 10% to the cost of changes there. If we don't invest, in a year it will be 20%, in two years 40%. At some point the cost exceeds what we can afford and we're forced into a rewrite, which is 10x more expensive than repairing along the way."

**Quantitative example:**

Base feature cost in clean code: 5 engineer-days.
Debt interest: 20% (adds 1 day per feature).
Unrepaired, interest rate grows at 10%/year.

| Year | Interest rate | Cost per feature | Features per team-year |
|------|---------------|------------------|----------------------|
| 0    | 20%           | 6 days           | 40                   |
| 1    | 22%           | 6.1 days         | 39                   |
| 2    | 24%           | 6.2 days         | 38                   |
| 3    | 27%           | 6.3 days         | 37                   |
| 4    | 30%           | 6.5 days         | 36                   |
| 5    | 33%           | 6.6 days         | 36                   |

Over 5 years, that's 15 features lost without any single year looking bad.

**Interview insight:** This kind of quantitative reasoning — even with made-up numbers — is more persuasive than qualitative concern. It gives stakeholders something to argue with, which is better than complaint.

### Q17. How do you prevent technical debt from accumulating as a team grows?

**Answer:**

Debt rate grows faster than team size if unmanaged. A team of 30 can produce more than 10x the debt of a team of 3. Prevention requires system-level investment.

**Structural prevention:**

1. **Code ownership (CODEOWNERS or equivalent).** Prevents "everyone's problem" diffusion. Every directory has a responsible team.

2. **Architecture review process.** Non-trivial designs go through review before implementation. Catches bad abstractions early.

3. **Fitness functions in CI.** Automated enforcement of architectural properties. Prevents regressions.

4. **Shared standards and style guides.** Written, enforced by linters, reviewed during onboarding.

5. **Tech radar.** Common vocabulary for what's adopted, what's on trial, what's on hold.

6. **Onboarding curriculum.** New engineers learn "how we build" explicitly.

**Cultural prevention:**

1. **Quality is non-negotiable in review.** Reviewers uphold the bar regardless of deadline pressure.

2. **Refactoring is part of feature work.** Not a separate activity requiring approval.

3. **Incidents surface debt.** Post-incident reviews tie incidents to underlying debt; action items drive repayment.

4. **Recognition for cleanup work.** If only feature shipping is rewarded, debt accumulates. Celebrate good refactors and test additions publicly.

5. **Blameless culture.** If admitting "I took a shortcut" has career cost, debt gets hidden. Hidden debt is the worst kind.

**Scaling-specific patterns:**

1. **Platform team.** Shared infrastructure, libraries, CI/CD. Prevents each team reinventing — and degrading — the wheel.

2. **Enabling team (Team Topologies).** Temporarily embeds with product teams to raise quality bar.

3. **Technical strategy.** A written document on where the architecture is heading — so 30 teams pull in the same direction instead of 30 directions.

4. **Deprecation tooling.** As things age, you need first-class support for deprecating and removing them. Without this, old things linger forever.

**Reference:** *Team Topologies* (Skelton & Pais) for organisational patterns at scale. *Accelerate* for the quality-velocity correlation. *The Staff Engineer's Path* (Reilly) for how technical strategy works.

### Q18. What's the role of an engineering manager in managing technical debt?

**Answer:**

Engineers identify and repay debt; managers create the conditions under which debt management is possible. The manager's role is organisational, not hands-on-technical.

**Manager responsibilities:**

1. **Capacity allocation.** Commit a percentage of team capacity to debt/quality work. Without this, feature pressure crowds everything out.

2. **Shield from excess pressure.** When stakeholders demand "ship now, fix later" repeatedly, the manager pushes back upstream. Engineers can't always do this effectively alone.

3. **Advocate for investment.** With leadership, with product, with finance. Explain why investment in engineering health pays off.

4. **Hold the quality bar in reviews and hiring.** Hire engineers who care about quality; promote engineers who demonstrate it.

5. **Make debt visible.** Dashboards, quarterly reviews, metrics tied to OKRs.

6. **Run retrospectives seriously.** Identify what practices create debt; change them.

7. **Remove blockers.** If engineers say "we can't fix X because Y team owns it," the manager works with Y team's manager.

**Manager pitfalls:**

- **Treating debt as an engineering complaint to manage.** Debt is an organisational risk, not a morale issue.
- **Always prioritising features.** Teams burn out; top engineers leave; velocity collapses long-term.
- **Hero-worshipping engineers who ship fast without quality.** The team follows what's rewarded.
- **Over-correcting into "pure quality."** Teams exist to ship value. All-debt-all-the-time is also a failure.

**How senior engineers partner with managers:**

- Give them data they can take to their boss. Not complaints; numbers.
- Propose bounded experiments the manager can "sell."
- Surface debt early, not in crisis.
- Respect the full picture — they see pressures engineers don't.

**Interview insight:** Senior candidates with strong manager-partnership instincts describe this partnership actively. Candidates who see managers as obstacles reveal a gap they'll face as they become staff engineers, where manager-partnership is the core of the role.

**Reference:** *The Manager's Path* (Camille Fournier). *An Elegant Puzzle* (Will Larson) — especially chapters on engineering strategy.

---
