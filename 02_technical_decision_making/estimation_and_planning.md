# Estimation and Planning — Interview Questions

**Subject:** Technical Leadership
**Topic:** Breaking Down Work, Estimation Techniques, Handling Uncertainty, Scope Negotiation
**Difficulty tiers:** Fundamentals / Intermediate / Advanced

---

## Fundamentals

### Q1. Why is software estimation hard?

**Answer:**

Software estimation is not the same as estimating a construction project. Key reasons:

1. **Novelty.** Most software work has elements you haven't done before. Truly repetitive work gets automated away; what's left is novel.

2. **Hidden complexity.** The specification omits details the code must address. "Just add a field" may mean DB migration, API change, downstream consumer updates, documentation.

3. **Unknown unknowns.** You don't know what you don't know. The hard bug that appears in week 3 wasn't predictable in week 0.

4. **Integration cost grows superlinearly.** Two systems have one integration; three systems have three; ten systems have forty-five. Estimates based on single-system complexity miss this.

5. **Dependencies on other teams.** Your estimate depends on their estimate, which depends on their dependencies.

6. **Pressure distorts estimates.** Engineers under pressure to "finish quickly" underestimate. Engineers who've been burned before over-estimate defensively.

7. **Feedback loops.** Building a thing changes what you think the thing should be. Early prototypes reveal requirements that should have been in the original spec.

**The fundamental research finding (Magne Jørgensen et al.):** Experts systematically underestimate software work, and their confidence in their estimates is uncorrelated with accuracy. Adding "experience" doesn't fix this; structured techniques do.

**Interview insight:** Candidates who answer "you just need to break it down further" have a shallow model. Senior engineers acknowledge estimation is genuinely hard, and describe techniques that *manage* uncertainty rather than eliminate it.

**Reference:** Steve McConnell, *Software Estimation: Demystifying the Black Art* (2006). Magne Jørgensen's research on estimation psychology.

### Q2. What is the difference between an estimate, a target, and a commitment?

**Answer:**

Conflating these is the root cause of most estimation dysfunction.

- **Estimate:** A probabilistic statement about effort or duration. "50% confidence: 3 weeks; 90% confidence: 6 weeks."
- **Target:** A business-desired delivery date. "We'd like this by end of Q2."
- **Commitment:** A promise made to stakeholders, with consequences if missed. "We will ship this by July 31."

**Why this matters:**

- **Estimates are inputs; commitments are decisions.** You estimate, then a decision-maker chooses whether to commit — possibly with scope/resource adjustments.
- **Targets are wishes, not data.** "We want it by Q2" doesn't affect how long the work takes.
- **Confusing them creates pressure to give unrealistic estimates.** Engineers asked "how long?" with a target in mind report the target and hope.

**Example conversation:**

> **PM:** "How long will the new reporting system take?"
> **Engineer:** "Estimate: 50% chance of 8 weeks, 90% chance of 14 weeks. The 14-week scenario involves migrating the underlying data store, which we'd discover around week 4 if it happens."
> **PM:** "We need it in 10 weeks for the customer launch."
> **Engineer:** "10 weeks is a 70% chance on current scope. Options: (a) commit to 10 weeks with risk of slip, (b) descope the reporting module's advanced filtering to improve confidence, (c) extend to 14 weeks for 90% confidence."

This conversation respects all three concepts.

**Interview insight:** Candidates who can articulate this distinction show they've survived the "why are our estimates always wrong?" post-mortem cycle and understand how to manage it.

**Reference:** McConnell, *Software Estimation*, chapter "Estimates, Targets, and Commitments" is the seminal discussion.

### Q3. What is "story points" and why do teams use them?

**Answer:**

**Story points** are a relative sizing unit — not hours, not days. They measure effort relative to other stories the team has completed.

**The theory:**

- Humans are bad at absolute estimates but decent at relative estimates. "Is this harder than the X we did last sprint?" is easier than "How many hours?"
- Story points are per-team; they don't transfer across teams or time periods.
- Velocity (points per sprint) is an observed property, not a committed one.

**Common scales:**

- **Fibonacci-ish:** 1, 2, 3, 5, 8, 13, 20, 40, 100. Gaps prevent false precision.
- **T-shirt sizes:** XS, S, M, L, XL. Useful for high-level roadmaps.
- **Linear:** 1–10. Rarely used — invites arguing about "is this a 6 or a 7?"

**Problems with story points in practice:**

- **Treated as hours.** Managers demand "how many points per week?" and convert them back to hours, defeating the purpose.
- **Compared across teams.** Team A's 5 ≠ Team B's 5; velocity comparisons are meaningless.
- **Gamed.** When performance depends on velocity, points inflate. Team "improves" by re-pointing the same work higher.
- **False precision.** 13-point vs 8-point arguments waste time.

**Alternatives:**

- **No estimation (#NoEstimates movement).** Break stories into similar sizes; count stories. Avoids the estimation overhead entirely.
- **Ranges with confidence.** "2-5 days at 80% confidence" is more honest than "3 points."
- **T-shirt sizing** for rough roadmap-level planning; detailed estimation only for near-term work.

**Interview framing:** "Story points are useful inside a team if treated as relative sizing for forecasting. They're harmful when used for performance evaluation or cross-team comparison. I've seen both, and I'm explicit about which we're doing."

### Q4. What is a spike or research task?

**Answer:**

A **spike** is a time-boxed investigation to reduce uncertainty before committing to an estimate or design. Named by XP (Kent Beck) after "spiking through" a problem to prove feasibility.

**When to use a spike:**

- Before estimating unfamiliar work.
- When technical feasibility is uncertain.
- When multiple approaches exist and you need data to choose.
- When integration with an unknown external system is required.

**Characteristics of a good spike:**

1. **Time-boxed.** "2 days to answer X." Not open-ended research.
2. **Has a specific question.** "Can we achieve <100ms p99 with this approach?" or "Does the vendor's API support our use case?"
3. **Produces a decision artifact.** A doc, a working prototype, or a recommendation — not just "we learned stuff."
4. **Throwaway code is fine.** The output is the decision, not the prototype code.

**Anti-patterns:**

- **Endless spikes.** "Another week on the spike" is a sign the spike question was too broad or the team is avoiding commitment.
- **Spike becomes production code.** The spike gets "just one more feature" and suddenly is in the critical path. Prototypes rarely survive productionisation; delete and rewrite intentionally.
- **No output.** Engineer disappears for a week; nothing documented. Wasted time.

**Example:**

> **Question:** Can we migrate authentication to vendor X without breaking our SSO integration with partner Y?
> **Spike (3 days):** Build a minimal prototype. Authenticate a test user through X. Verify SSO handshake with Y's sandbox.
> **Output:** 1-page doc — what works, what doesn't, recommended path.

**Interview insight:** Spikes show you take uncertainty seriously. "I'd estimate it after a 3-day spike" is a stronger answer to "how long?" than a made-up number.

### Q5. What is a "pre-mortem" and how does it apply to planning?

**Answer:**

A **pre-mortem** (Gary Klein) imagines a future failure of the project and asks "what went wrong?" Already covered in the architecture decisions section; here, applied to planning specifically.

**Planning pre-mortem procedure:**

1. Assume: "6 months from now, this plan has failed catastrophically. We missed by 3x and lost key engineers."
2. Individually, everyone lists what likely caused the failure.
3. Cluster the causes.
4. Identify top risks.
5. Add mitigations to the plan — either prevention, early detection, or contingency.

**Common planning pre-mortem findings:**

- "We assumed team X would deliver on time; they didn't."
- "The library we depended on had a breaking change."
- "The scope crept as stakeholders added requirements."
- "Our lead engineer left; no one else had context."
- "The production environment differed from staging in ways we didn't anticipate."
- "Security review uncovered issues late, forcing redesign."
- "We didn't account for holiday/PTO coverage."
- "The MVP satisfied no one — too thin to replace old system, too expensive to justify."

**What to do with the findings:**

- **Add buffer** for high-probability risks.
- **Identify early-warning indicators** for each risk.
- **Write contingency plans** for the most severe risks.
- **Get commitments** from dependency teams before they become blockers.
- **Document assumptions** so they're reviewable later.

**Why this matters:**

Plans that imagine only the success path are fragile. Plans that have imagined failure are robust — or at least aware of their fragility.

**Interview insight:** Describing pre-mortem use in planning signals staff-level thinking. Most teams don't do this; those that do catch risks early enough to act.

### Q6. How do you break down a large project into estimable pieces?

**Answer:**

Large projects are not estimable directly. The skill is breaking them down enough that each piece is estimable, then rolling up.

**Approach — work decomposition:**

1. **Start with the outcome.** What does "done" look like for users?
2. **Identify major phases or milestones.** Architecture, implementation, migration, launch.
3. **Within each phase, identify deliverables.** Each deliverable is a testable, demonstrable piece.
4. **Break deliverables into stories.** Each story is estimable (typically < 1 week).
5. **Identify dependencies.** What must happen before what.
6. **Identify uncertainty.** Some stories are knowable; others require spikes first.

**Heuristics:**

- **The "8-hour test":** If a story can't be completed in a day by one engineer, it's probably too big — subdivide.
- **Vertical slices, not horizontal layers.** A story that touches frontend, backend, and DB is more valuable than "build the database layer." Vertical slices deliver user value at each completion.
- **Explicitly defer integration.** Integration is usually harder than components. Plan for it separately.

**Decomposition example:**

> **Project:** Add multi-currency support to checkout.
>
> **Phases:**
> 1. Currency representation in the data model.
> 2. Pricing display in product pages.
> 3. Checkout flow (currency selection, conversion display).
> 4. Order storage and receipts.
> 5. Reporting / analytics.
> 6. Rollout and cutover.
>
> **Within Phase 3:**
> - Currency selector UI (2 days)
> - Currency conversion service (spike: 2 days, then 5 days implementation)
> - Cart display in selected currency (3 days)
> - Checkout validation for supported currencies (2 days)
> - Integration tests for cross-currency flows (3 days)

**Anti-patterns:**

- **Decomposing only the known parts.** Hard or unknown parts get a vague "and then we integrate" line. Those are the parts that kill estimates.
- **Over-decomposing.** 200 tickets for a 6-week project creates management overhead without improving accuracy.

**Interview insight:** Senior engineers decompose with a "vertical slice" instinct. Junior engineers decompose by architectural layer. The difference: vertical slices deliver incremental value; horizontal layers deliver integration problems.

---

## Intermediate

### Q7. What is "cone of uncertainty" and how do you use it in planning?

**Answer:**

The **cone of uncertainty** (Barry Boehm, popularised by Steve McConnell) describes how estimate precision improves as a project progresses.

**The cone:**

| Project phase              | Optimistic–pessimistic range |
|----------------------------|------------------------------|
| Initial concept            | 0.25x – 4x                   |
| Approved product definition| 0.5x – 2x                    |
| Requirements complete      | 0.67x – 1.5x                 |
| UI design complete         | 0.8x – 1.25x                 |
| Detailed design complete   | 0.9x – 1.1x                  |

**What this means:**

- An estimate "3 months" at project start could be anywhere from 3 weeks to a year.
- The cone only narrows as work proceeds — uncertainty doesn't shrink from thinking harder, it shrinks from doing.
- Estimates at the start of a project will be wrong. That's not a team failure; it's a property of the work.

**Implications for planning:**

1. **Re-estimate as you learn.** The initial estimate is the least accurate you'll ever have. Update it.
2. **Report in ranges, not points.** "6–10 weeks, 80% confidence" honours the cone.
3. **Stage commitments.** Commit to the next phase, not the whole thing, when uncertainty is high.
4. **Don't demand precision early.** Asking for a ±10% estimate at project start is asking for a number that can't exist.

**The cone is not symmetric in practice.** Laurent Bossavit's *The Leprechauns of Software Engineering* argues that the cone narrows more slowly than Boehm's data suggested, and that uncertainty often widens again in the middle as surprises emerge. The idealised cone is a lower bound on uncertainty.

**Interview framing:**

> "I'd give a range proportional to where we are in the cone. At kickoff, a 4x range is honest. After the first milestone, I can narrow. Asking for point estimates at the start invites inaccurate commitments."

**Reference:** McConnell, *Software Estimation*. Bossavit, *The Leprechauns of Software Engineering* (pushback on naive cone use).

### Q8. How do you handle scope creep during a project?

**Answer:**

Scope creep is inevitable. The choice is whether to manage it or be managed by it.

**Sources of scope creep:**

- **Requirements discovery.** As you build, you learn what was really needed. Usually legitimate.
- **Stakeholder additions.** "While you're in there..." requests. Usually small individually, devastating cumulatively.
- **Engineer gold-plating.** Internal drive to "do it right" adds unscoped work.
- **Adjacent opportunities.** Refactoring, tool improvements, documentation that seem natural but weren't in scope.

**Management techniques:**

1. **Keep the original scope visible.** A living "scope" document or ticket with explicit in-scope and out-of-scope items.

2. **Treat changes as requests, not additions.** Each change goes through the same triage as any other work — cost, value, impact on schedule.

3. **"Yes, and..." rather than "yes."** "Yes, we can add dark-mode — it adds two weeks to the timeline. Do we push back the launch, cut something else, or defer?"

4. **Explicit trade-offs.** Show the cost. Stakeholders routinely accept scope cuts when they see the cost of additions.

5. **Defer to next iteration.** "Good idea — logged for v2." Legitimises the idea without accepting it now.

6. **Periodic re-plan.** If scope is shifting substantially, a full re-plan is more honest than pretending the original plan still applies.

**Anti-patterns:**

- **Silent absorption.** Adding scope without telling stakeholders. Plan quietly fails; relationships damaged when it does.
- **Rigid refusal.** "It's not in scope" blocks all changes; legitimate changes get frustrated.
- **Engineer self-authorisation.** An engineer adds scope on their own judgment without visibility. Other stakeholders surprised later.

**Example language:**

> "That's a reasonable addition. Our current timeline assumes the original scope; adding it would push us by approximately 3 days. Options:
>
> (a) Extend the launch date by 3 days.
> (b) Swap it in for a lower-priority item we'd planned (X or Y).
> (c) Defer to the next release.
>
> Your call — what would you prefer?"

**Interview insight:** Candidates who manage scope through explicit trade-off conversations demonstrate staff-level stakeholder skills. Candidates who either silently absorb or reflexively refuse reveal weaker judgement.

### Q9. How do you estimate work that depends on other teams?

**Answer:**

Cross-team dependencies are the single largest source of missed estimates. The mitigation is structural, not individual.

**Techniques:**

1. **Identify dependencies early.** Explicit dependency list in the plan. Named teams, named contacts, specific asks.

2. **Get their estimate.** Don't guess their timeline — ask. "When could you ship X?" If they can't commit, that's data.

3. **Build slack for their variability.** If they say 4 weeks, don't plan as if you'll have it in week 4. Plan for "their 4 weeks" to be 5–6 of your weeks due to handoff cost.

4. **Decouple where possible.** If team B's API is the dependency, can you work against a mock or spec? Can you deliver everything except the integration in parallel?

5. **Raise risks early.** If the dependency team is slipping, your plan is at risk. Surface the risk upward, not after the slip.

6. **Offer help.** "We could help write the integration tests, or contribute a PR to team B's repo" can move their timeline.

7. **Track the dependency like a ticket.** Regular check-ins, visible status, escalation path.

**Escalation protocol:**

- **Week -4 from needing it:** Friendly check-in with the team.
- **Week -2:** If unclear, a status meeting; risks raised.
- **Week -1 if not ready:** Escalate to managers. This is legitimate; waiting until the miss date is not.
- **If it slips:** Options for your team (partial delivery, descope, delay).

**Never do:**

- Assume the dependency will land on time without confirmation.
- Plan around the dependency landing *before* you need it (no buffer).
- Hide the risk until it becomes a miss.
- Blame the dependency team publicly at miss time — you had weeks to raise it.

**Interview example:**

> "On one project, our migration depended on a shared platform team landing a feature. They'd committed to 'end of Q2.' I set up weekly 15-minute syncs and tracked their progress. In week 8 of a 12-week plan, their work was at 40% — clearly not tracking to end of Q2. I raised with my manager and theirs. We agreed on a reduced integration scope that unblocked us. The full integration shipped a quarter later but our launch wasn't blocked."

**Reference:** *Team Topologies* (Skelton & Pais) on managing team dependencies. *The Goal* (Goldratt) on theory-of-constraints — your pace is constrained by your slowest dependency.

### Q10. What is Hofstadter's Law and how does it apply to software estimation?

**Answer:**

**Hofstadter's Law:** "It always takes longer than you expect, even when you take into account Hofstadter's Law."

The law is self-referential — humour with truth. Even adjusting for expected overrun, you're probably still optimistic.

**Why the law holds:**

1. **Planning fallacy.** Humans systematically underestimate future tasks (Kahneman & Tversky). The bias doesn't go away with awareness.

2. **Optimism bias.** When we imagine the plan, we imagine the smooth execution path. We don't imagine the bugs, the re-scopes, the dependencies, the PTO.

3. **Asymmetric feedback.** Estimates that were too long are rarely noted; estimates that were too short become "misses." This biases memory and experience toward optimism.

4. **Unknown unknowns.** By definition you can't estimate them. They arrive anyway.

**Practical adjustments:**

1. **Apply a multiplier.** Some teams multiply initial estimates by 1.5-2x as a baseline. Crude but better than nothing.

2. **Estimate with reference class forecasting.** Rather than "how long should this take?" ask "how long has similar work taken in the past?" The outside view (Kahneman) beats the inside view.

3. **Include buffer explicitly.** Buffer for uncertainty is legitimate, not padding. Call it out: "20% buffer for integration and unknowns."

4. **Estimate components; beware of integration.** Individual-task estimates usually don't add up to the observed duration because integration is separate and slippery.

5. **Track actuals against estimates.** If the team consistently runs at 1.4x, factor that in next time.

**The paradox for seniors:** If you always inflate estimates, stakeholders perceive you as slow. If you always under-promise, you lose credibility. The answer is calibrated estimation — honest ranges with confidence levels.

**Reference:** Douglas Hofstadter, *Gödel, Escher, Bach* (1979). Kahneman & Tversky on the planning fallacy. Flyvbjerg on reference class forecasting.

### Q11. How do you estimate work with significant unknowns?

**Answer:**

The honest answer is: you can't accurately estimate until you've reduced the unknowns. The technique is to estimate how much reduction is possible and sequence the work to maximise learning early.

**Techniques for estimating under uncertainty:**

1. **Spike first, then estimate.** Time-box a research phase. Commit to the remaining work only after.

2. **Three-point estimation (PERT).** For each task, estimate Optimistic (O), Most Likely (M), Pessimistic (P). Expected = (O + 4M + P) / 6. Gives a probability-weighted estimate.

3. **Monte Carlo simulation.** For a large project, simulate task durations drawn from distributions. Produces a distribution of project completion dates, not a single point.

4. **Ranges with confidence.** "50% chance 6 weeks; 90% chance 12 weeks" is more honest and more actionable than "9 weeks."

5. **Rolling wave planning.** Detailed plan for the next 2 weeks; rough plan for the next 2 months; direction only for the year. Commit detail only as you learn.

6. **Incremental delivery.** Deliver the smallest useful slice first. Learn from real usage. Plan the next slice with knowledge.

**When the unknowns are extreme:**

- **Reframe as research rather than delivery.** "We don't know enough to estimate — we need a quarter of learning before we can commit to dates."
- **Use outcome-based framing.** "By Q3 we'll know whether approach X is viable, and have shipped at least the proof of concept."
- **Agree on decision gates, not delivery dates.** "At the end of phase 1, we'll decide whether to continue."

**Stakeholder communication:**

> "We don't know enough to give a reliable estimate. We can commit to a 2-week discovery phase that ends with: a prototype, a go/no-go recommendation, and an estimate for the remaining work. That's the honest path. Giving a number now would be a guess."

**Interview insight:** Candidates who give confident estimates for genuinely uncertain work reveal naivete. The senior answer is honest uncertainty with a plan to reduce it.

**Reference:** *Software Estimation* (McConnell) on three-point and Monte Carlo. *Noise* (Kahneman, Sibony, Sunstein) on calibrated ranges vs point estimates.

### Q12. How do you handle the pressure to commit to an aggressive timeline?

**Answer:**

Pressure to commit is predictable. The responses split into honest ones and ones that store up problems.

**The failure modes — what not to do:**

1. **Commit to the target and hope.** The timeline was unreasonable; you miss; credibility damaged.
2. **Refuse without offering alternatives.** "We can't do that" with no path forward leaves stakeholders no options.
3. **Pad secretly.** Give an inflated estimate so you can meet it easily. Trust erodes over time.
4. **Agree to insane hours.** Destroys team, produces bugs, unsustainable.

**The honest response:**

1. **Separate the estimate from the commitment.** "The estimate is X. You want Y. Let's discuss how to close the gap."

2. **Offer trade-offs.**
   - **Scope:** "We can hit Y with reduced scope. Here's what drops out."
   - **Resources:** "We can hit Y with one more engineer for 4 weeks."
   - **Quality:** "We can hit Y but we'd skip integration testing — here's the risk."
   - **Date:** "We can't hit Y for the full scope. We can hit it for the minimum viable cut."

3. **Be specific about consequences.** "If we commit to Y, the risks are A, B, C — each with probability X%."

4. **Don't commit alone.** Bring your manager or tech lead into the conversation. Commitments made under pressure by one person often aren't realistic; a pair of people is harder to pressure.

5. **Explicitly flag uncertainty.** "I estimate 10 weeks at 80% confidence. If we commit to 8 weeks, I'd put confidence at 40%. Is that acceptable to the business?"

6. **Record the commitment.** An email, a document, a ticket — something that captures what was agreed with what trade-offs. Memory is unreliable.

**Language that helps:**

- "Here's what I can commit to."
- "Here's what would be required to commit to your preferred date."
- "My confidence in that date is X% — is that acceptable for this work?"
- "Let me come back with options."

**Interview STAR example:**

> **S:** Product wanted a major feature in 6 weeks. Our estimate was 12 weeks.
>
> **T:** I was the tech lead; pressure was landing on me.
>
> **A:** I brought three options: (1) 12 weeks original scope; (2) 6 weeks reduced scope — showed exactly what dropped; (3) 8 weeks original scope with two borrowed engineers from a partner team. I declined to commit to 6 weeks full scope; I said confidence would be under 30%.
>
> **R:** Product chose option 2. We shipped on time with the reduced scope. The dropped items shipped two months later. Trust in our estimates grew because we'd told the truth upfront.

**Interview insight:** The ability to negotiate scope rather than negotiate time is a leadership skill. Staff engineers are comfortable reshaping asks; junior engineers feel obliged to accept them.

---

## Advanced

### Q13. Describe a time your estimate was significantly wrong. What did you do?

**Answer (STAR template):**

> **S:** We estimated a data-migration project at 6 weeks. At week 4 we'd completed about 20% of the planned work. The 6-week estimate was clearly wrong.
>
> **T:** As the tech lead, I needed to raise the slip, understand the cause, and re-plan.
>
> **A:**
> 1. **Stopped the denial.** Ran a half-day retro with the team on where the time went. Three surprises: (a) the legacy schema had undocumented invariants that our migration had to preserve; (b) the downstream consumers needed code changes we hadn't scoped; (c) our test data was cleaner than production, so we discovered edge cases late.
> 2. **Re-estimated.** Detailed decomposition of remaining work, based on what we now knew. Result: 8 more weeks, ±2.
> 3. **Raised with stakeholders.** Same week, before the miss landed on anyone by surprise. Framed as: "Original estimate was wrong; here's why; here's the new estimate; here's what we'd change about our estimation process next time."
> 4. **Offered options.** Continue to full migration (8 weeks); or deliver a partial migration for the two highest-value systems first and tackle the others in Q+1 (4 weeks).
> 5. **Stakeholder chose the partial approach.** We shipped phase 1 in 4 weeks. Phase 2 took 6 weeks the following quarter, more accurate because we'd learned.
> 6. **Post-project retro.** Documented the lessons on estimation under schema uncertainty. Fed into our estimation checklist for similar work.
>
> **R:** Original commitment was missed but business impact was minimised. The team's estimation for subsequent migrations was more accurate because we'd learned what to ask. I was slightly embarrassed but my manager specifically noted that the early, honest raise was more valuable than a correct original estimate would have been.

**What this demonstrates:**

- Honesty — miss raised promptly.
- Cause analysis — understanding why.
- Re-planning — not wishful thinking.
- Stakeholder management — options, not bad news alone.
- Learning — post-project feedback into process.

**Interview insight:** Candidates without stories like this either haven't worked long enough or aren't telling the truth. Senior candidates have many; they're comfortable sharing one.

### Q14. How do you estimate a project across multiple teams and quarters?

**Answer:**

Cross-team, multi-quarter projects combine all the hard estimation problems — large scope, multiple dependencies, long horizon, high uncertainty.

**Approach:**

1. **Start with outcomes, not tasks.** What does success look like in 6 months? 12 months? Write it in terms of user/business outcomes, not engineering work.

2. **Major milestones, then work within.** 3–5 milestones, each quarter-sized. Each milestone is a checkpoint with clear deliverables.

3. **Team-level ownership of each piece.** Each team owns specific milestones. Cross-team work has named interface owners.

4. **Rolling wave planning.** Detailed for the next quarter; rough for next two; direction for the rest. Re-plan quarterly.

5. **Explicit assumptions and dependencies.** "This plan assumes team X delivers Y by Q2. If not, the plan changes — here's how."

6. **Risk register.** Major risks listed with probability, impact, mitigation, owner. Reviewed monthly.

7. **Leading indicators.** What metrics tell you the plan is on track or not, before the deadline arrives?

**Communication model:**

- **Daily within team.** Standup for the current work.
- **Weekly across teams.** Program-level status, dependencies, blockers.
- **Monthly with leadership.** Milestone progress, risk updates, re-plans if needed.
- **Quarterly.** Full re-plan. Scope reviewed, estimates updated, commitments revisited.

**Common failure modes:**

- **Waterfall-style commitments at month 0.** "We commit to this scope at this date 12 months out." Will be wrong.
- **Hope-based coordination.** Teams assume others will do their part without active tracking. They won't.
- **Single point of failure.** One engineer or one team holds the whole plan together. If they leave or slip, everything slips.
- **Status theatre.** Green reports masking real slips. Requires culture where raising problems is safe.

**Example framing:**

> "For a 12-month cross-team project, I'd commit to the outcomes and the next quarter's milestones specifically. For quarters 2-4, I'd provide target dates with explicit uncertainty ranges. I'd re-plan quarterly, which sounds like 'the plan keeps changing' but is actually the plan acknowledging real learning."

**Interview insight:** Candidates who commit to 12-month dates with precision reveal inexperience. Candidates who describe the rolling-wave structure explicitly, with scheduled re-planning, signal staff-level program management.

**Reference:** *An Elegant Puzzle* (Will Larson) on engineering strategy and migration management. *Shape Up* (Ryan Singer / 37signals) for a different, smaller-scale approach.

### Q15. How do you estimate work for a team you don't know well?

**Answer:**

Senior engineers often review estimates from other teams, or lead programs involving unfamiliar teams. You can't estimate for them directly, but you can structure the estimate-review to produce trustworthy numbers.

**Approach:**

1. **Talk to the team.** Not their manager. The engineers who'll do the work.

2. **Ask about past similar work.** "When did you last do something like this? How long did it take?" Reference class forecasting.

3. **Ask what they're uncertain about.** The uncertainty is usually bigger than the known work.

4. **Ask about dependencies.** Their dependencies, not just yours.

5. **Ask about team capacity.** Are they otherwise committed? PTO? Recent departures?

6. **Don't project your team's velocity onto theirs.** Their stack, skills, processes differ.

7. **Ask for ranges, not points.** "Best case, most likely, worst case" is more useful than a single number.

8. **Sanity-check with a second source.** Someone outside the team who has context — their manager, a former member, a sister team.

**Red flags in someone else's estimate:**

- Confidence without history. "We think 4 weeks" with no reference to past similar work.
- No mention of risk. Every estimate has risk; if they don't mention any, they haven't thought about it.
- No dependencies listed. Every non-trivial piece of work has dependencies.
- Single-point estimate without confidence level.
- Suspiciously round numbers. "Four weeks" from every team for every project.

**What to do with their estimate:**

- **Add buffer.** You don't know their calibration; 20-30% is reasonable default.
- **Track milestones, not just final delivery.** Slipping at mid-project is a signal.
- **Build explicit check-ins.** Weekly or biweekly sync to catch drift.
- **Plan for their slip.** What does your team do if they're 2 weeks late? 4?

**Interview insight:** The question tests whether you respect team differences or assume universal velocity. Candidates who describe active engagement with unfamiliar teams — curiosity, questions, respect for their context — show program-management maturity.

### Q16. What is "continuous planning" and how does it differ from quarterly planning?

**Answer:**

**Quarterly planning** sets a plan for the quarter; the team executes against it; adjustments happen at the next quarterly cycle. Common in companies that want predictability and external commitments.

**Continuous planning** treats the plan as a living document, updated as the team learns. The quarterly milestone is a checkpoint, not the only planning moment.

**Contrasts:**

| Aspect | Quarterly | Continuous |
|--------|-----------|------------|
| Plan update frequency | Once per quarter | Weekly or per sprint |
| Commitment granularity | Quarter-level | Sprint or weekly |
| Surprise tolerance | Low (plan held) | High (plan adapts) |
| Stakeholder expectation | Deliverables by date | Direction and rolling forecast |
| Team autonomy | Lower | Higher |

**When quarterly planning works:**

- Regulated or contractually committed deliveries.
- Clear, low-uncertainty work.
- External stakeholders who need date-based commitments.
- Small teams where the overhead of replanning isn't worth the flexibility.

**When continuous planning works:**

- High-uncertainty work — research, new products, exploration.
- Rapidly changing markets or requirements.
- Mature teams comfortable with rolling forecasts.
- Cultures that value learning over commitment.

**Hybrid models (most common):**

- **Quarterly themes, weekly priorities.** Quarter sets direction; week-level planning adjusts based on learning.
- **Rolling wave.** Detailed for the next 2-4 weeks; rough for the rest of the quarter.
- **OKRs with adaptive execution.** Objectives are set quarterly; key results tracked continuously.

**The common failure mode:**

Teams say they do continuous planning but actually do quarterly commitments dressed up. If the commitment can't be updated without org drama, it's quarterly planning regardless of label.

**Interview insight:** Candidates who describe their planning style explicitly, and explain when they'd choose each, signal awareness of the trade-offs. "We just plan continuously" without qualification often masks unstructured work.

**Reference:** *Agile Estimating and Planning* (Mike Cohn) on the spectrum between plan-driven and adaptive. *Shape Up* (Singer) for a specific alternative to sprint-based planning.

### Q17. How do you estimate research or exploratory work?

**Answer:**

Research work has structurally different estimation properties than delivery work. You don't know when you'll find what you're looking for, or if you will.

**Techniques:**

1. **Time-box, don't scope-box.** Commit to spending X weeks on the research; accept that the output depends on what you learn.

2. **Decision gates, not deliverables.** "By week 4, we'll know whether approach A is viable." Binary outcomes are easier to estimate than quality outcomes.

3. **Explicit hypothesis.** Research is testing a belief. "We believe approach X will achieve Y by Z measure." Estimate the time to refute or confirm, not the time to build.

4. **MVP / spike / prototype.** Smallest thing that proves or disproves the hypothesis. Not production quality.

5. **Kill criteria.** What would make us stop? "If we can't hit 100ms p99 in the prototype, the approach is wrong." Pre-agreed kill criteria prevent sunk-cost fallacy.

6. **Incremental commitments.** "Fund phase 1 for 4 weeks. Decide on phase 2 based on phase 1 outputs."

**What NOT to do:**

- Give a date for "it'll work."
- Pretend research is execution — pad with unknowns and hope for the best.
- Hide research as delivery work because stakeholders don't fund research.

**Example framing to stakeholders:**

> "We can't commit to a date for 'add ML-based recommendations' because we don't yet know if it'll improve click-through enough to justify the complexity. What we can commit to:
>
> - 4 weeks of exploration.
> - At the end: a report with (a) whether the approach is viable, (b) if yes, estimated cost to productionise, (c) if no, what we learned and what to try instead.
>
> Funding: 1.5 engineers × 4 weeks = 6 engineer-weeks. Budget: £30k. We commit to the decision at week 4."

**Interview insight:** Candidates who force research into delivery-style estimation reveal they haven't managed R&D work. The senior approach is to negotiate different evaluation criteria for different work types.

**Reference:** Gerhard Hartmann's research on R&D estimation. *Lean Startup* (Eric Ries) on build-measure-learn cycles, which map naturally onto research estimation.

### Q18. How do you handle estimation in a team that has consistently missed estimates?

**Answer:**

Chronic missing signals something is systematically wrong. "Try harder" doesn't fix systemic issues.

**Diagnose:**

1. **Are estimates actually wrong, or are commitments inflated?** Sometimes the team estimates correctly but stakeholders pressure them to commit to less. Different problem.

2. **Is it consistent direction?** Consistently under? Consistently over? Or high variance?

3. **Which types of work?** Features may estimate well; migrations or integrations poorly. Or vice versa.

4. **Is it one person or the team?** Different interventions apply.

5. **Are dependencies realistic?** Often "we missed" is "a dependency slipped."

**Common causes:**

- **Optimism bias.** Team assumes smooth execution. Solution: structured estimation (three-point), retrospective calibration.
- **Hidden work.** Estimates ignore review, testing, deployment, documentation. Solution: include full definition-of-done in estimates.
- **Pressure to underestimate.** Manager or stakeholder demands low numbers. Solution: change the culture upstream.
- **Scope creep not tracked.** Work grew; estimates didn't. Solution: explicit scope-change protocol.
- **Unknowns treated as knowns.** Estimate assumes everything goes right. Solution: spike first or range estimates.
- **Team churn.** New members reset velocity. Solution: explicit ramp-up time.
- **Technical debt.** Estimates assume clean code; reality is messy. Solution: debt-aware estimation.

**Interventions:**

1. **Track estimate vs actual, and learn.** Can't improve what you don't measure.

2. **Retrospectives focused on estimation.** Dedicated time, not a bullet in the general retro.

3. **Reference class forecasting.** "Last 5 projects of this size took X, Y, Z. This one is probably similar."

4. **Estimate with ranges and confidence.** "Most likely 4 weeks; worst case 7 weeks" breaks the single-number habit.

5. **Change the culture.** If the manager punishes slips, the team learns to pad. If the manager punishes "too much padding," the team learns to commit to unrealistic dates. The culture needs honest estimates with honest follow-up.

6. **Post-project analysis.** Actually do it; learn from it; write it down.

**Anti-pattern:** "We need to get better at estimation" as a recurring goal with no structural change. Estimation accuracy doesn't improve from willpower.

**Interview insight:** A candidate who's led estimation improvement can describe specific interventions that worked. Vague answers ("we did retros") suggest the problem wasn't solved.

**Reference:** McConnell's *Software Estimation* has a chapter on estimation calibration. *Noise* (Kahneman et al.) on reducing systematic estimation error.

---
