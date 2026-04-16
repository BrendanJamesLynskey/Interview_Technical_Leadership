# Architecture Decisions — Interview Questions

**Subject:** Technical Leadership
**Topic:** ADRs, RFC Process, Trade-off Analysis, Build vs Buy, Technology Selection
**Difficulty tiers:** Fundamentals / Intermediate / Advanced

---

## Fundamentals

### Q1. What is an Architecture Decision Record (ADR)?

**Answer:**

An **Architecture Decision Record (ADR)** is a short document that captures a single architectural decision, its context, the options considered, and the reasoning for the chosen option.

Named by Michael Nygard in his 2011 post *Documenting Architecture Decisions*, ADRs became widespread as teams recognised that architecture lives longer than individuals, and the reasons for decisions are lost faster than the consequences.

**Minimum structure:**

```markdown
# ADR NNN: Title (short imperative verb phrase)

## Status
Proposed | Accepted | Superseded by ADR NNN | Deprecated

## Context
What is the issue we're addressing? What forces are at play — business,
technical, organisational?

## Decision
What have we decided? Short, direct, active voice.

## Consequences
What becomes easier? What becomes harder? What are we giving up?
```

**Key properties:**

- **Immutable.** You don't edit an old ADR when the decision changes. You write a new ADR that supersedes it.
- **Short.** One to three pages. If an ADR needs ten pages, split it.
- **Numbered and committed to the repo.** Lives alongside the code.
- **Present-tense factual in "Decision"; explicit about what you're losing in "Consequences."**

**Why this format works:** It forces honesty about trade-offs. Engineers who can only name the upsides of a decision haven't thought about it enough.

### Q2. What is the difference between an ADR and an RFC?

**Answer:**

Both capture technical decisions, but they serve different moments in the decision lifecycle.

| Aspect | RFC (Request for Comments) | ADR (Architecture Decision Record) |
|--------|---------------------------|-----------------------------------|
| When written | Before decision | After decision |
| Purpose | Solicit input, build consensus | Document what was decided |
| Length | Can be long | Usually short (1–3 pages) |
| Status lifecycle | Draft → Review → Accepted/Rejected | Proposed → Accepted → Superseded |
| Editable? | Yes, during review | No — supersede instead |
| Alternatives | Explored in detail | Summarised |

**Workflow in a mature team:**

1. Engineer drafts an RFC for a non-trivial decision.
2. RFC circulates; stakeholders comment, author revises.
3. A decision is reached (accept, reject, or revise-and-resubmit).
4. The outcome is summarised as an ADR committed to the repo.

Some teams use only one format. Others unify them ("RFC becomes ADR once accepted"). The important thing is not the format name but that decisions are written down, reviewed before being committed to, and preserved after.

**Reference:** Python's PEPs, Rust's RFCs, and Kubernetes Enhancement Proposals (KEPs) are public examples of long-running RFC processes. Nygard's blog post is the canonical ADR reference.

### Q3. When should you write an ADR?

**Answer:**

Not every decision deserves an ADR — and not every ADR-worthy decision gets written down. Calibration matters.

**Write an ADR when:**

- The decision is **hard to reverse.** Choosing a database, a primary language, a service boundary.
- The decision has **wide impact.** Affects multiple teams, production, or long-term maintenance.
- The decision is **non-obvious.** Future engineers will ask "why did they do it this way?"
- The decision required **non-trivial debate.** Losing options should be documented alongside the winning one.
- The decision affects **security, compliance, or cost** materially.

**Don't write an ADR for:**

- Trivial or reversible choices.
- Implementation details that don't affect the contract.
- Decisions captured better in code (naming conventions, formatting, lint config).
- Purely stylistic preferences.

**A common heuristic:** "Would a new engineer on the team, six months from now, benefit from reading this?" If yes, write the ADR. If no, don't.

**Anti-pattern:** ADR-on-everything leads to a repo full of ADRs no one reads. Scarcity preserves value.

### Q4. What is a "two-way door" vs "one-way door" decision?

**Answer:**

Jeff Bezos' framing, widely cited in engineering leadership:

- **Two-way door:** A reversible decision. You can walk through, see if you like it, and walk back. Easy to undo.
- **One-way door:** An irreversible (or nearly irreversible) decision. Walking back is expensive or impossible.

The framing matters because the level of deliberation should match the reversibility.

**Examples:**

| Two-way door | One-way door |
|--------------|--------------|
| Choice of CI runner | Choice of primary database |
| Internal API shape | Public API shape |
| Build system | Licence of open-source project |
| A/B test variant | Public cryptographic protocol choice |
| Log format | Data schema committed to GDPR disclosures |
| Team's code style | Service-boundary split |

**How this changes decision-making:**

- **Two-way doors:** Bias to action. Try it; learn; adjust. Over-deliberating wastes time.
- **One-way doors:** Slow down. Write the ADR. Review widely. Prototype. Run the pre-mortem. Get it right.

**The failure mode of one-way decisions:** Teams treat them as two-way because walking back *feels* possible ("we can always migrate later"). Migration cost compounds; later is never.

**Interview insight:** Candidates who frame decisions by reversibility — rather than by importance or by gut feel — are showing a senior mental model. It signals they've seen the consequences of collapsing the distinction.

### Q5. What is a technology radar?

**Answer:**

A **technology radar** (popularised by ThoughtWorks) is a recurring publication that tracks an organisation's relationship with technologies across four rings:

- **Adopt:** Proven, recommended for production use.
- **Trial:** Worth pursuing on non-critical projects. Gain experience.
- **Assess:** Watch; understand its potential. Not ready for production use.
- **Hold:** Do not start new work with this. Existing usage may continue but should not grow.

Organised by quadrants: Techniques, Tools, Platforms, Languages & Frameworks.

**Why it exists:** Without a shared map, teams independently evaluate the same tools, arrive at different conclusions, and fragment infrastructure. A radar gives structured guidance without micromanagement.

**Example entries:**

- *Adopt:* PostgreSQL for transactional data.
- *Trial:* gRPC for internal service-to-service calls.
- *Assess:* Rust for performance-sensitive microservices.
- *Hold:* Introducing new MongoDB installations (existing ones are fine).

**How the radar supports decisions:**

- Gives teams a default — "use what's on Adopt unless you have a reason not to."
- Surfaces risks — "Hold" tells teams to not double down on legacy choices.
- Sets expectations — "Trial" is accompanied by expected evaluation criteria and timeline.

**Reference:** ThoughtWorks Technology Radar is public and biannual. Many large organisations (Spotify, Zalando, Thoughtworks clients) publish or maintain internal equivalents.

### Q6. What goes into a build-vs-buy decision?

**Answer:**

Build-vs-buy is one of the most common technical decisions and one of the most commonly wrong. Senior engineers evaluate systematically.

**Axes to consider:**

1. **Core competence.** Is this a differentiating capability or a commodity? Build differentiators; buy commodities.
2. **Total cost of ownership (TCO).** Not just licence fees — integration, training, vendor management, migration cost when the vendor changes terms or goes out of business.
3. **Time to value.** How quickly do you need it working? Buy is usually faster to first value; build may pay off over years.
4. **Customisation needs.** Can the off-the-shelf solution fit your workflow? Or will you spend more customising than you would building?
5. **Vendor lock-in.** What's the exit cost? Data portability, format standards, migration tooling.
6. **Team capacity and skill.** Do you have the engineers to build and maintain? Can they be freed from other work?
7. **Risk profile.** Vendor viability, security track record, compliance certifications.
8. **Strategic alignment.** Does this become infrastructure others depend on?

**Example decision: authentication.**

| Criterion | Build | Buy (e.g., Auth0) |
|-----------|-------|-------------------|
| Core competence? | No, for most businesses | — |
| TCO over 3 years | ~$500k (engineers + maintenance) | ~$150k (licence + integration) |
| Time to value | 6–12 months | Weeks |
| Customisation | Full | Bounded |
| Lock-in | None | Material — migration cost high |
| Risk | Security errors | Vendor risk |

For most companies, the answer is buy. For a company whose product *is* identity (Okta, Auth0 themselves), it's build.

**Common mistakes:**

- Underestimating build cost (especially ongoing maintenance, which dominates over years).
- Ignoring opportunity cost (engineers not building other things).
- Overestimating build quality ("our auth will be better than Auth0's").
- Ignoring vendor risk (what if they 10x the price? Get acquired? Shut down?).

**Interview framing:** "My default is buy for commodities, build for differentiators. The analysis is total cost of ownership and opportunity cost, not sticker price."

---

## Intermediate

### Q7. How do you structure the "Options Considered" section of a design doc or RFC?

**Answer:**

The "Options Considered" section is where the decision is actually made. A weak section lists options; a strong one evaluates them against explicit criteria.

**Template:**

```markdown
## Evaluation Criteria

Before describing options, state what matters:
1. Latency p99 must stay under 50ms
2. Dev team learning curve — solutions requiring months of training score low
3. Operational cost — annual infrastructure budget for this capability
4. Compatibility with existing observability stack (Prometheus + Grafana)
5. Migration risk — zero data loss, < 30 min cutover

## Option A: [Name]

**Description:** One paragraph.
**How it meets criteria:** Table or bullets against each criterion.
**Pros:** 3–5 bullets.
**Cons:** 3–5 bullets — if you can't list cons, you haven't thought hard enough.
**Rough effort:** Person-weeks.

## Option B: [Name]
...

## Option C: Status quo / do nothing
Often the correct answer. Always list this.

## Recommendation
Pick one. State why. Acknowledge what you give up.
```

**Principles:**

- **Criteria before options.** Otherwise you rationalise a preferred choice.
- **Status quo is always an option.** Doing nothing has a cost but also avoids new risk.
- **Honest cons.** An RFC with all-upsides per option is a sales pitch, not a decision.
- **Explicit recommendation.** "We could do any of these" is not a decision — make one.

**Anti-pattern:** Listing 7 options equally. Readers lose focus. Usually 3 options cover the real space; more suggests the author hasn't filtered.

### Q8. How do you evaluate a new programming language or framework for adoption?

**Answer:**

Language/framework decisions are high-impact and long-lived. A framework chosen today will shape the team for 5–10 years.

**Evaluation dimensions:**

1. **Maturity and community.** Package ecosystem, documentation, Stack Overflow activity, release cadence. Look at GitHub issue age, response time.
2. **Performance characteristics.** Against your workload, not generic benchmarks. Run a realistic prototype.
3. **Operational profile.** Monitoring, profiling, memory behaviour, deployment footprint. Will on-call engineers be able to debug production issues?
4. **Hiring market.** Can you hire for it? What does the talent pool look like in your geography/market?
5. **Team skill fit.** How steep is the learning curve for your current team? Who champions it?
6. **Integration with existing stack.** Observability, service mesh, CI, deployment, IDE tooling.
7. **Licence and governance.** For frameworks — is it controlled by one company? Any licence concerns (recent ElasticSearch, Redis, MongoDB changes)?
8. **Long-term bet.** Who's investing in it? Is it gaining or losing momentum?

**Prototype before committing.** Build something non-trivial. Don't rely on demos.

**Decision horizon matters:**

- Adopting a language for a single experimental service: low risk, short horizon. Be more permissive.
- Adopting a language as the team's primary: multi-year commitment. Evaluate rigorously.

**Common failures:**

- **Hype-driven adoption.** "This is the new hotness" is not a reason. Look at five-year trajectories, not Twitter excitement.
- **Résumé-driven development.** Engineers lobby for tech they want on their résumé. Recognise the motivation; require it to meet the same bar as any other choice.
- **Status-quo bias.** "We've always used X" locks you into tech that may no longer be the best fit. Revisit periodically.

**Reference:** *Accelerate* (Forsgren et al.) notes that technology choices should enable team throughput, not just technical elegance. The tech radar format (ThoughtWorks) gives a structured way to evaluate over time.

### Q9. How do you handle a disagreement on an architectural decision?

**Answer:**

Disagreement is the normal state of architecture. How you handle it determines whether the team converges or fragments.

**Framework — "disagree and commit" (Amazon's term):**

1. **Get the disagreement on the table explicitly.** Not in hallway muttering. On the RFC, in a meeting, with both sides documented.
2. **Argue about criteria, not options.** Often disagreement is really about what you're optimising for. Surface that.
3. **Time-box the debate.** Extended disagreement wastes energy. Agree on when the decision will be made.
4. **Make the decision with the designated decision-maker.** For most architectural decisions, this is a tech lead, staff engineer, or architect.
5. **Once decided, commit.** Disagreement ends at the decision point. The team moves forward unified. Lingering dissent poisons execution.
6. **Document the disagreement.** The ADR can note that "this decision was non-unanimous; the alternative was X, preferred because Y; trade-offs Z." Future engineers see the reasoning.
7. **Schedule review.** "We'll revisit in 6 months with the following data" makes disagree-and-commit tolerable — the dissenting view isn't permanently buried.

**Things that don't work:**

- **Endless consensus-seeking.** Some decisions will never get 100% buy-in. Waiting for it means never deciding.
- **Ignoring the dissent.** If a senior engineer disagrees and you proceed without engaging, you've broken trust.
- **Voting.** Architecture is not democratic — the person with the most context and accountability decides. But they listen.
- **Appeal to authority alone.** "I'm the architect, we're doing it my way" is brittle. Give reasons.

**Reference:** *An Elegant Puzzle* (Will Larson) and *The Manager's Path* (Camille Fournier) both have sections on decision-making under disagreement. Bezos' 2017 shareholder letter popularised "disagree and commit" for technical contexts.

### Q10. What is a pre-mortem and how do you run one?

**Answer:**

A **pre-mortem** (Gary Klein) flips the post-mortem into a pre-decision exercise. Assume the project/decision failed; work backwards to identify why.

**Procedure:**

1. **Frame:** "It's six months from now. The project we're planning has failed catastrophically. What went wrong?"
2. **Everyone writes independently.** Silence prevents groupthink. Usually 10 minutes.
3. **Collect and cluster.** Group similar causes.
4. **Rank by likelihood and impact.** What are the top 3 failure modes?
5. **For each top risk, decide:**
   - Can we prevent it? (Add a mitigation to the plan.)
   - Can we detect it early? (Add a check or review gate.)
   - If it happens, can we recover? (Add a contingency.)
6. **Write the mitigations into the plan.**

**Why it works:**

- **Retrospective framing reduces social cost.** It's safer to say "the team burned out" after a hypothetical failure than to say "we'll burn the team out" in a planning meeting.
- **Forces imagining the whole arc.** Plans tend to imagine the successful path only.
- **Surfaces blindspots.** Engineers assume different failure modes; collecting them exposes the full space.

**When to use:**

- Before a major architectural decision commits.
- Before a big migration or cutover.
- Before a team reorg or tech migration.
- Before launching anything customer-facing with reputational risk.

**Interview insight:** Pre-mortems are the behaviour that distinguishes staff engineers from senior engineers. Senior engineers write good plans; staff engineers write good plans that also say "here's what I'm afraid of."

### Q11. How do you choose between a monolith, microservices, and a modular monolith?

**Answer:**

The "microservices vs monolith" debate is worth unpacking because most engineers have strong opinions and weak frameworks.

**Decision factors:**

1. **Team size and structure.** Microservices' overhead is justified when team count requires independent deployability. Under 20 engineers total, usually a monolith is enough.
2. **Deployment independence.** Do teams actually need to deploy without coordinating? If yes, service boundaries matter. If everyone ships together, monolith.
3. **Technology heterogeneity.** Do different parts of the system genuinely need different languages/runtimes? Usually not.
4. **Scaling profile.** Do different components have wildly different scaling needs? Microservices let you scale independently. Monoliths scale uniformly.
5. **Operational maturity.** Microservices require excellent observability, CI/CD, service mesh, on-call rotation. Without these, distributed systems bite.
6. **Domain boundaries.** Are the bounded contexts clear? Microservices encode domain boundaries; getting them wrong creates worse coupling than the monolith you escaped.

**Decision heuristic:**

- **Start with a monolith.** Almost always correct for early-stage systems.
- **Evolve to a modular monolith.** Strong internal boundaries (separate packages, clear interfaces), single deployable. Many of the benefits of microservices without the operational cost.
- **Extract services when the boundary is proven.** A module that has been stable, well-bounded, and team-owned for a year is a good extraction candidate.
- **Never start with microservices** unless you have strong reason — scale, team structure, or regulatory boundary.

**Sam Newman's test (from *Building Microservices*):** Could your team confidently deploy this service multiple times a day, independently? If not, microservices aren't helping.

**Interview framing:** "I default to modular monolith. I extract services only when the extraction clearly unlocks something — scaling, team ownership, deploy independence — that the monolith can't provide."

**Reference:** *Building Microservices* (Sam Newman), *Monolith to Microservices* (Newman), *Software Architecture: The Hard Parts* (Ford, Richards). Shopify's public writing on their modular monolith approach is worth citing.

### Q12. What is the difference between accidental complexity and essential complexity?

**Answer:**

From Fred Brooks' *No Silver Bullet* (1986), still the clearest framing of where engineering effort goes.

- **Essential complexity** is inherent to the problem. You cannot remove it — only model it well. A trading system is essentially complex because trading is complex.
- **Accidental complexity** is added by the implementation: bad tools, unclear abstractions, excessive coupling, over-engineered frameworks, build systems, deployment pipelines.

Brooks' argument: software productivity gains come from eliminating accidental complexity. Essential complexity requires smart modelling, but you can't engineer it away.

**Why this matters for decisions:**

- A tech choice that reduces accidental complexity is valuable — new languages, better tooling, cleaner abstractions.
- A tech choice that claims to reduce essential complexity is probably hiding it or moving it. Skepticism warranted.
- When a team says "this is too complex," identify which kind. Accidental complexity is fixable by refactoring. Essential complexity means the problem is genuinely hard; the answer is learning, not simplification.

**Signs of accidental complexity:**

- Build system fighting the code rather than supporting it.
- Multiple patterns solving the same problem (two HTTP clients, three logging systems).
- Layers of abstraction that don't earn their weight.
- Cargo-culted architecture (microservices because FAANG does; the monolith would have been fine).

**Signs of essential complexity:**

- The domain itself has intricate invariants (regulatory, mathematical, physical).
- The interactions between components reflect real-world relationships.
- The complexity survives attempted simplification — every removed piece turns out to matter.

**Interview insight:** This framing is especially powerful in architecture interviews because it gives you vocabulary for pushing back on "just use microservices" or "just use this framework" recommendations. Not every complexity-reduction claim is real.

---

## Advanced

### Q13. Describe a time you made an architectural decision that turned out to be wrong. What did you do?

**Answer (STAR template):**

Interviewers want to see self-awareness, course-correction, and judgement. "I've never made a wrong decision" is a fail.

> **S:** Two years ago I advocated for adopting a NoSQL document store for a catalogue service. The argument was schema flexibility — catalogue shape varies per product category.
>
> **T:** As the proposing engineer, I wrote the ADR, ran the benchmarks, and led the migration.
>
> **A:** Within six months the weaknesses showed. We needed transactional consistency across product and inventory that NoSQL couldn't cheaply give us. Query patterns had evolved — we now needed joins we couldn't efficiently express. Backup/restore tooling for the NoSQL store lagged our Postgres equivalent.
>
> I did several things:
> 1. **Named the problem.** In an architecture review, I said "I recommended this, and I'm now concerned it's not serving us well." Went through the evidence.
> 2. **Proposed options.** Stay and mitigate (build the consistency layer ourselves); migrate back to Postgres; or introduce a secondary Postgres for transactional data while keeping NoSQL for catalogue.
> 3. **Ran a pre-mortem on each.** The hybrid approach had distributed-transaction failure modes I couldn't explain away.
> 4. **Migrated back to Postgres.** Used dual-write, shadow-read, backfill. Ten weeks.
> 5. **Wrote a superseding ADR.** Explicit about what I'd missed in the original: I'd evaluated schema flexibility but not transactional needs, and I'd given too much weight to "future-proofing" for a query profile we were still learning.
>
> **R:** Migration completed without data loss. Latency on catalogue queries improved 30% (we'd been over-fetching to work around missing joins). More importantly, the superseding ADR and my public ownership of the reversal changed team dynamics — engineers became more comfortable proposing reversals of earlier decisions.

**What this demonstrates:**

- Willingness to admit error publicly.
- Structured response (options, pre-mortem, plan).
- Learning extracted and written down (the ADR).
- Cultural impact — modelling "it's okay to reverse."

**Anti-pattern:** Blaming others, or framing the reversal as "we learned we needed X" without acknowledging the earlier choice was flawed. Interviewers can tell.

### Q14. How do you decide when to adopt a new technology that the team is excited about but is unproven?

**Answer:**

Engineer enthusiasm is valuable — intrinsic motivation beats assigned work. But unproven tech carries real risk. The senior job is to channel enthusiasm into bounded experiments.

**Framework:**

1. **Acknowledge the interest.** Don't suppress. Engineers advocating for new tech are often also the ones who'll maintain it.

2. **Scope the bet.** Find a low-stakes place to evaluate. Not the billing system; maybe an internal tool, a non-critical service, or a specific module.

3. **Define success criteria upfront.** What does "it worked" look like? Performance targets, developer experience metrics, operational maturity. Agreed before the experiment, not invented after.

4. **Time-box the evaluation.** Usually a quarter. If it's not proven useful by then, it's either successful enough to keep or should be reverted.

5. **Require an exit plan.** If we adopt and it goes wrong, how do we walk back? Keep optionality.

6. **Share learnings.** Even if the experiment fails, the team learns. Write it up. A failed evaluation is not a wasted one.

**Questions to ask before approving the experiment:**

- Who owns this operationally if it gets to production?
- What's the hiring implication?
- What's the migration cost if we later want to rip it out?
- Is this a two-way door or one-way door?

**Example evaluation arcs:**

- **Try Rust for performance-sensitive microservice.** Bounded scope, two quarters, measured against the Go baseline. If the velocity cost is acceptable and performance wins are real, expand. Otherwise, shelve.
- **Try a new observability vendor.** One team trials for a quarter. Measured: incident response time, dashboard authoring speed, cost per metric. Decision at quarter end.

**When to say no:**

- The champion is interested but won't own the long-term maintenance.
- The risk exceeds the experiment's blast radius.
- The alternative (our current tech) isn't actually causing pain — you're adopting for fun.

**Reference:** ThoughtWorks' "Trial" ring on the Tech Radar formalises this. Many companies have an official "innovation budget" — explicit capacity for bounded experiments.

### Q15. How do you write a design doc for a system you don't fully understand yet?

**Answer:**

Writing a design doc is a thinking tool. The act of writing forces you to confront what you don't know. Strong engineers embrace this rather than avoiding it.

**Approach:**

1. **Start with the problem, not the solution.** A clear problem statement is 40% of the doc. If you can't write one, you don't understand the problem yet.

2. **Write the "Context" section first.** What do you know about the domain? Who are the users? What are the constraints — performance, scale, deadline, compliance?

3. **Enumerate unknowns explicitly.** Have a section called "Open questions" or "Things I need to learn." Honesty about gaps invites help.

4. **Sketch the solution.** Don't detail. Draw the boxes and arrows. Name the components. Identify the interfaces.

5. **Share the draft early.** "Here's my understanding so far — what am I missing?" is a powerful prompt. Reviewers fill in blind spots.

6. **Iterate the doc, not just your understanding.** Each pass adds detail and removes uncertainty. Track "answered" vs "still open" questions.

7. **Let the doc evolve into an ADR when the decision is made.** The final doc captures both what you decided and the path to that decision.

**Key mindsets:**

- **Writing is thinking.** Don't wait until you understand to write. Write to understand.
- **Public drafts beat private polish.** A shared rough draft gets better faster than a private perfect one.
- **Open questions are an asset, not a weakness.** Senior engineers show their gaps; juniors often hide them.

**Example open-questions section:**

```markdown
## Open Questions

- [ ] What's our current p99 on the write path? Need to check dashboards.
- [ ] Does the legal team have constraints on PII in event streams?
- [ ] What's the budget for the new infrastructure? Rough order of magnitude.
- [ ] Who owns the downstream consumer system? Need to loop them in.
```

**Reference:** *The Staff Engineer's Path* (Tanya Reilly) has a chapter on writing design docs as a mechanism for clarity. Google's Blog post "Design Docs at Google" is a publicly available reference.

### Q16. How do you measure the quality of architectural decisions?

**Answer:**

Decision quality is different from outcome quality. A good decision with bad outcome is still a good decision; a bad decision that got lucky is still bad.

**Measuring process quality:**

- **Was an ADR/RFC written?** For decisions with long-term impact, yes should be the norm.
- **Were options genuinely considered?** Three or more, with explicit trade-offs.
- **Were the right stakeholders involved?** Security, infra, affected teams.
- **Was reversibility understood?** One-way vs two-way flagged?
- **Was a pre-mortem run** for one-way decisions?
- **Was the reasoning documented?** Future engineers can reconstruct "why."

**Measuring outcome quality (retrospectively):**

- **Did the decision achieve its stated goals?** Measured against the criteria in the RFC.
- **What surprises emerged?** Pre-mortem calibration check.
- **Would we make the same decision today?** If not, what did we learn? Is there an ADR update needed?
- **Did downstream costs match estimates?** Migration, maintenance, performance.

**Practice — the decision retrospective:**

Periodically (quarterly, or after any major incident), review a sample of past decisions. For each:

1. Was this a good decision given what we knew at the time?
2. Did it play out the way we expected?
3. What would we do differently?

This is separate from incident post-mortems. It's about decision hygiene, not specific failures.

**Anti-pattern: outcome-only measurement.** If a bad decision gets lucky, the team learns "don't bother with ADRs, it worked." If a good decision hits bad luck, the team learns "ADRs don't help." Separate the process from the outcome.

**Reference:** *Thinking, Fast and Slow* (Kahneman) on the difference between process and outcome quality. *The Halo Effect* (Rosenzweig) on survivorship bias in business decisions.

### Q17. How do you make architectural decisions when the organisation has legacy choices you disagree with?

**Answer:**

Most senior engineers inherit an architecture. They didn't choose the database, the language, or the deployment model. Complaining is easy; making good decisions within constraints is the work.

**Framework:**

1. **Separate "I inherited this" from "I wouldn't choose it today."** The former is a fact; the latter is an opinion. Acting on opinion without facts damages trust.

2. **Understand why the legacy choice exists.** Often there's a reason that wasn't obvious from outside. Sometimes the reason is "legacy-not-good-enough-to-replace-yet." Both are valuable to know.

3. **Pick your battles.** You can credibly push back on 2–3 things per quarter, not 20. What matters most?

4. **Design around constraints, not despite them.** If the organisation is committed to Postgres, your design assumes Postgres. Using a sidecar NoSQL store is fighting the tide.

5. **Earn the right to change things.** New engineers pushing for rewrite in their first month usually lose. Engineers who've shipped well within constraints for a year earn credibility to challenge them.

6. **Write the ADR for the change, don't just complain.** If you want the org to migrate off language X to language Y, write a proper proposal. Make the case in writing. "I don't like X" is not a proposal.

7. **Accept that some things won't change.** Not every decision is worth relitigating. Some are. Know which is which.

**Framing: building a coalition.**

Large changes happen through coalitions, not heroic individual action. Find the other engineers who share the concern. Find the manager whose team would benefit. Build the case together. Staff engineers spend more time on coalitions than on code.

**Interview insight:** Candidates who answer this question by describing how they'd "force the change" reveal naïveté. Candidates who describe sequencing, earning credibility, and coalition-building are demonstrating staff-level judgement.

**Reference:** *An Elegant Puzzle* (Will Larson) on how change actually happens at scale. *The Staff Engineer's Path* on "the right fight at the right time."

### Q18. How do you approach technology selection when you have to support the decision for 10 years?

**Answer:**

Most technology decisions get reviewed sooner than they deserve. A decade-long decision deserves unusual care.

**Checklist:**

**1. Boring is better.** Dan McKinley's "Choose Boring Technology" — for each innovation token you spend, you pay operational cost. Spend them on what differentiates your product.

**2. Evaluate trajectory, not just snapshot.** Where will this tech be in 5 years? Is investment flowing in or out? Who's betting on it — one company, a foundation, a broad community?

**3. Survival rate matters.** Technologies that have survived 10 years have demonstrated durability. New shiny things have high mortality.

**4. Exit cost is a first-class criterion.** If in year 7 you need to migrate off it, how hard is that? Portable standards > proprietary protocols.

**5. Community depth.** Will you be able to hire engineers who know this in year 8? Will there be Stack Overflow answers for new versions of the language?

**6. Governance.** Is the project/product controlled by one company with their own business interests? (Recent ES/Redis/MongoDB licence changes.) Or is it foundation-governed (Kubernetes, Linux)?

**7. Operational maturity.** Runbooks, observability, SLAs — well-established tech has these. New tech often doesn't.

**8. Upgrade story.** You will upgrade this tech many times. What's the upgrade cadence? Breaking changes rate? LTS policy?

**Long-horizon examples:**

- **Choosing Postgres in 2015** — 10 years later, thriving, still the right choice for most OLTP.
- **Choosing CoffeeScript in 2013** — 5 years later, TypeScript had won; CoffeeScript users migrated under duress.
- **Choosing RethinkDB in 2015** — 2 years later, company folded. Migration was forced.
- **Choosing Kubernetes in 2018** — arguably the right bet, but "innovation tokens" spent compounded for years.

**Framing:**

> "For a 10-year decision, I weight durability, governance, and exit cost higher than peak performance or ergonomics. I'd rather use something slightly worse that I know will exist in a decade than something optimal that might not."

**Reference:** Dan McKinley, "Choose Boring Technology" (2015) is the key essay. *Designing Data-Intensive Applications* (Kleppmann) discusses long-horizon data storage decisions.

### Q19. What is the role of a "tech lead" or "architect" in decision-making, and how does it differ from a manager's role?

**Answer:**

Confusing tech leadership with engineering management creates predictable failures. They require overlapping but distinct skills.

**Manager:**
- Owns people outcomes — growth, performance, retention, hiring.
- Owns team outcomes — delivery, roadmap, stakeholder alignment.
- Has hire/fire/promote authority.
- Runs 1:1s, performance reviews, compensation conversations.

**Tech lead / architect / staff engineer:**
- Owns technical outcomes — correctness, quality, architectural coherence.
- Owns technical standards — code review, ADRs, technical direction.
- Has influence, not authority — decisions land through conviction and reasoning, not hierarchy.
- Mentors technically; partners with the manager on growth.

**Where they overlap:**

- Estimation and planning — manager owns "when," tech lead owns "how."
- Hiring — manager owns loop, tech lead owns technical bar.
- Conflict resolution — both handle it, from different angles.

**Failure modes:**

- **Manager making technical decisions alone.** Decisions lack depth; team disengages.
- **Tech lead acting as shadow manager.** Unclear accountability; manager undermined.
- **Either role operating without the other.** Technical decisions without people context, or people decisions without technical context.

**Senior engineer insight:** The strongest teams have the manager and tech lead paired — they co-own the team's success, with clear division of labour. Weak teams have one doing both (usually badly) or the two in competition.

**Reference:** *The Manager's Path* (Camille Fournier) on the handoff between IC and management tracks. *The Staff Engineer's Path* (Tanya Reilly) on the IC leadership role explicitly.

### Q20. How do you handle a situation where you've written an ADR and the team collectively wants to ignore it?

**Answer:**

This is a failure mode that separates real governance from performative governance. Writing ADRs nobody follows is worse than not writing them.

**Diagnose first:**

1. **Is the ADR wrong?** Maybe the team has information you lacked. Listen before defending.
2. **Is the ADR unknown?** Did people see it? ADRs in a forgotten folder aren't decisions.
3. **Is the ADR right but inconvenient?** Short-term convenience beats long-term discipline without enforcement.
4. **Is there enforcement?** Code review, CI checks, architectural fitness functions?

**Interventions:**

- **If the ADR is wrong:** Supersede it. Write a new one that captures what you've learned. Don't pretend the old one is authoritative.
- **If it's unknown:** Publicise. Tech talks, onboarding docs, link in PR templates.
- **If it's right but ignored:** Add enforcement. Lint rules, CI gates, mandatory reviewers. ADRs without enforcement decay.
- **If enforcement already exists and people still circumvent:** Organisational problem. Escalate to management; decision-making authority is unclear.

**Cultural undertone:**

ADRs assume a culture where written decisions are respected. In cultures where "things get decided in hallways," ADRs become advisory. If you're trying to build written-decision culture, the work isn't writing more ADRs — it's building the norm that decisions are only real when written.

**Interview framing:** "I've seen ADRs ignored; the fix is never writing more ADRs. It's either updating the ADR, adding enforcement, or building the cultural norm that written decisions are the ones that stick."

---
