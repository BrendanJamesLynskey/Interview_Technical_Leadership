# Refactoring and Legacy Code — Interview Questions

**Subject:** Technical Leadership
**Topic:** Safe Refactoring, Strangler Fig, Characterisation Tests, Incremental Migration
**Difficulty tiers:** Fundamentals / Intermediate / Advanced

---

## Fundamentals

### Q1. What is refactoring, and what is it not?

**Answer:**

**Refactoring**, as Martin Fowler defines it, is "a disciplined technique for restructuring an existing body of code, altering its internal structure without changing its external behaviour."

The key properties:

- **Behaviour-preserving.** The tests (especially observable behaviour) do not change.
- **Small steps.** Each refactoring is small enough to verify quickly.
- **Tests as safety net.** Refactoring without tests is rewriting with hope.

**What refactoring is not:**

- **Not rewriting.** A rewrite changes behaviour or starts from scratch. Refactoring is incremental.
- **Not redesign.** Major architectural change is "re-architecting" — refactoring is the low-level technique used to get there safely.
- **Not "cleanup sprints."** Refactoring is woven into feature work, not scheduled as a separate activity.
- **Not bug fixing.** A bug fix changes behaviour. Refactoring doesn't.

**Why the distinction matters in interviews:** Engineers who say "I refactored the auth module" when they mean "I rewrote it" reveal imprecise thinking. Seniors distinguish these because the risk profile is radically different.

**Reference:** Martin Fowler, *Refactoring: Improving the Design of Existing Code* (2nd ed., 2018).

### Q2. What is legacy code?

**Answer:**

Two useful definitions compete:

- **Michael Feathers' definition** (from *Working Effectively with Legacy Code*, 2004): "Code without tests." The key property is not age, it's untestability.
- **Pragmatic definition:** Code that works, is in production, and is painful to change safely.

Both definitions point at the same thing: the problem with legacy code is that you can't make changes confidently. Age, language, or style don't matter — your ability to change it without fear does.

**What makes code legacy in practice:**

- No tests, or tests that don't reflect actual behaviour.
- Implicit coupling — changes ripple unexpectedly.
- Lost context — the people who wrote it are gone, and the "why" is undocumented.
- Fragile deployments — changes require manual steps or tribal knowledge.

**The senior engineer's reframe:** Every new line of code you write today is tomorrow's legacy code, unless it's well-tested, well-documented, and decoupled. Legacy-ness is created continuously, not inherited.

### Q3. What is a characterisation test and when do you use one?

**Answer:**

A **characterisation test** is a test that documents what a piece of legacy code *currently does* — not what it was intended to do.

The procedure (Feathers):

1. Call the code under test with realistic inputs.
2. Let it fail the assertion.
3. Read the failure output — this is what the code actually does.
4. Put that output into the assertion.
5. Commit the test.

You now have a regression safety net. You can refactor with confidence that any change in behaviour will trigger a test failure.

```python
# Legacy function with unclear behaviour
def calculate_rate(customer_type, amount, region):
    # 200 lines of nested conditionals
    ...

# Characterisation test
def test_calculate_rate_enterprise_uk_small():
    # What does the code do? Run it and see.
    result = calculate_rate("enterprise", 50, "UK")
    assert result == 4.75  # Pinned to current behaviour

def test_calculate_rate_consumer_us_large():
    result = calculate_rate("consumer", 50000, "US")
    assert result == 127.5

# Now we can refactor with a safety net.
```

**Important caveat:** Characterisation tests pin *current* behaviour — including current bugs. You might later realise `calculate_rate("consumer", 50000, "US")` should return 125, not 127.5. That's a separate change, made visible by the existing test.

**When NOT to use characterisation tests:** When the behaviour is so clearly a bug that preserving it would be worse than changing it. This is a judgement call — usually better to pin it, refactor, then fix, in three separate commits.

### Q4. What is the Strangler Fig pattern?

**Answer:**

Named by Martin Fowler after the strangler fig tree, which gradually grows around and replaces its host tree. The pattern is used to replace legacy systems incrementally.

**The mechanism:**

1. Put the legacy system behind a facade (proxy, API gateway, load balancer).
2. Build new functionality as a separate service/module, accessed through the same facade.
3. Migrate existing functionality piece by piece — route traffic for a feature to the new implementation.
4. Retire the legacy system once nothing routes to it.

```
Before:             During:                 After:
                                            
 Client              Client                  Client
   |                   |                       |
   v                   v                       v
 Legacy             Facade/Proxy           Facade/Proxy
                    /      \                   |
                Legacy   New System        New System
                   \      /
                 Shared DB / events
```

**Why this works:**

- **No big-bang migration.** Risk is bounded per-route.
- **Always in production.** You can pause, roll back, or abandon at any stage.
- **Tests in anger.** Each migrated route is validated with real traffic.
- **Learning informs later migrations.** You get smarter as you go.

**Real-world examples:**

- Amazon's migration from monolith to services (early 2000s).
- The Guardian's move off Oracle (publicly written up by Graham Tackley).
- Shopify's storefront renderer replacement.

**Contrast with rewrite:** A rewrite pauses feature work, takes 18 months, and often fails to reach parity. The strangler fig ships value continuously.

### Q5. What is the Mikado Method?

**Answer:**

The **Mikado Method** (Ola Ellnestam, Daniel Brolund) is a technique for planning large refactorings by working *backwards* from the goal.

**Procedure:**

1. Write down the goal — "extract PaymentGateway into its own module."
2. Attempt the change naively.
3. Observe what breaks.
4. Revert the change.
5. Note each breakage as a prerequisite — a child node on the Mikado graph.
6. Take the next-level prerequisites and repeat.
7. Keep expanding until you reach a prerequisite that can be done *today* without breaking anything else.
8. Start there. Work back up the tree, checking each change in.

**The key insight:** Each commit is tiny and green. You never hold a 3-week branch. The hard thinking is done upfront on the graph; the coding itself is routine.

```
Goal: Extract PaymentGateway
├── Remove direct DB access from checkout module
│   ├── Introduce repository interface
│   ├── Migrate existing callers
│   └── Add characterisation tests for current behaviour
├── Move config out of global
│   └── Introduce config injection
└── Split tests that cover multiple modules
```

**When to use it:** For any refactoring that "I tried and it spiralled." The Mikado Method turns spirals into a plan.

### Q6. What is the Scout Rule and how does it apply to legacy code?

**Answer:**

**Scout Rule** (Robert C. Martin, *Clean Code*): "Always leave the campground cleaner than you found it."

Applied to code: every time you touch a file, make it slightly better than you found it. Rename a confusing variable. Extract a function. Add a missing test. Delete a dead comment.

**Why this works at scale:**

- No budget required — it's part of normal work.
- Improvements compound. Over a year, a hotspot file gets touched 50 times; 50 small improvements is substantial.
- No "cleanup sprint" politics — nobody has to justify the time.
- Reviewers normalise expecting small improvements alongside changes.

**Limits of the Scout Rule:**

- It doesn't substitute for planned refactoring of large issues.
- It can spread scope creep into PRs — reviewers should push back on PRs that mix too many concerns.
- It can't rescue code that's beyond incremental repair.

**In interviews:** Candidates who cite the Scout Rule signal they take everyday craftsmanship seriously. Candidates who claim to refactor only during dedicated "tech debt sprints" reveal a worse model.

---

## Intermediate

### Q7. How do you refactor code with no tests?

**Answer:**

Feathers' advice: you can't refactor safely without tests, and you often can't write tests without refactoring first. The resolution is a careful sequence:

**Step 1: Identify a "seam."** A seam is a place where you can change behaviour without editing code at that spot — usually by substituting a dependency. Common seams: function arguments, constructors, subclass overrides, linker-level substitution.

**Step 2: Break the minimum dependency to get the code into a test harness.** This might mean extracting an interface, introducing dependency injection, or extracting a pure function.

**Step 3: Write characterisation tests.** Pin current behaviour. You're not judging whether behaviour is correct — just capturing it.

**Step 4: Refactor with the safety net.** Now Fowler-style refactoring is safe.

**Step 5: Add proper tests.** Replace the characterisation tests with tests that describe intended behaviour.

**Example:**

```python
# Legacy: tightly coupled to real email server
class OrderService:
    def confirm_order(self, order_id):
        order = Database.get(order_id)
        order.status = "confirmed"
        Database.save(order)
        SMTPClient("smtp.example.com").send(
            to=order.customer_email,
            subject="Order confirmed",
            body=f"Your order {order.id} is confirmed"
        )
```

**Step 1 — add a seam:**

```python
class OrderService:
    def __init__(self, db=None, mailer=None):
        self.db = db or Database
        self.mailer = mailer or SMTPClient("smtp.example.com")

    def confirm_order(self, order_id):
        order = self.db.get(order_id)
        order.status = "confirmed"
        self.db.save(order)
        self.mailer.send(...)
```

**Step 2 — characterisation test:**

```python
def test_confirm_order_sends_email():
    fake_db = FakeDB({"o1": Order(id="o1", customer_email="x@y.com")})
    fake_mailer = FakeMailer()
    service = OrderService(db=fake_db, mailer=fake_mailer)
    service.confirm_order("o1")
    assert fake_db.get("o1").status == "confirmed"
    assert fake_mailer.sent == [("x@y.com", "Order confirmed", "Your order o1 is confirmed")]
```

**Step 3 — now refactor freely.**

**Reference:** Feathers, *Working Effectively with Legacy Code* (2004). The chapter "Dependency-Breaking Techniques" is the canonical reference for seams.

### Q8. When do you recommend a rewrite vs. incremental refactoring?

**Answer:**

Rewrites are rarely the right answer, but occasionally they are. The framing should be explicit.

**Incremental refactoring is preferred when:**

- The existing system works and serves users.
- You can isolate subsystems behind interfaces.
- The business can tolerate slower feature velocity but not a pause.
- The team understands the system well enough to refactor safely.
- The problem domain is stable.

**Rewrite may be justified when:**

- The existing system cannot meet a step-change in requirement (scale, compliance, architecture).
- The platform is end-of-life (language, runtime, framework unsupportable).
- The team no longer understands it, and reverse-engineering is prohibitively expensive.
- It's a prototype that escaped — "throwaway quality" is too deep to fix in place.
- Cost of incremental change has become higher than cost of rewrite *plus* parity.

**The rewrite playbook (if justified):**

1. **Define the MVP scope.** Not feature parity — the subset that unblocks users.
2. **Keep the old system running.** Run both in parallel for a long time.
3. **Instrument comparison.** Shadow traffic, diff responses, measure for real.
4. **Migrate users incrementally.** Feature flags, account cohorts, geographic rollout.
5. **Plan for failure.** You will run both systems longer than planned. Budget for it.

**Famous cautionary tales:**

- Netscape's rewrite (chronicled by Joel Spolsky — "Things You Should Never Do") — cited as the collapse that let Microsoft win the browser war.
- Many finance industry "green field" rewrites that never reach parity.

**Interview insight:** When asked "would you recommend a rewrite?", the strong answer is "almost always no, but here's when I'd consider it, and here's how I'd de-risk it." Candidates who default to "rewrite" reveal naivety about migration cost.

### Q9. How do you decide which parts of a legacy system to refactor first?

**Answer:**

Use a **change-frequency × cost-of-change matrix**, sometimes called the hotspot analysis.

**Axes:**

- **Change frequency.** From git log — which files are modified most often?
- **Complexity or pain.** Cyclomatic complexity, bug density, review time, developer-reported friction.

**Quadrants:**

| | Low change | High change |
|---|---|---|
| **Low pain** | Leave alone | Leave alone |
| **High pain** | Low ROI to refactor | **Refactor first** |

The quadrant to attack is high-change, high-pain. Low-change code, even if ugly, costs nothing to leave alone. High-change, low-pain code is working.

**Practical commands:**

```bash
# Files most frequently changed, last 6 months
git log --since="6 months ago" --pretty=format: --name-only \
  | grep -v '^$' | sort | uniq -c | sort -rn | head -20
```

Combine with a complexity tool (e.g., `lizard` for multi-language, `radon` for Python):

```bash
radon cc -s -a src/
```

The intersection — frequently changed *and* complex — is where refactoring pays off.

**Reference:** Adam Tornhill, *Your Code as a Crime Scene* (2015). The book expands this idea into "behavioural code analysis."

### Q10. What is the difference between tactical and strategic refactoring?

**Answer:**

A useful vocabulary (popularised by John Ousterhout and various staff engineers):

**Tactical refactoring** is the Scout Rule in action — small improvements made while doing other work. Rename a variable, extract a helper, tighten a type hint. No separate planning, no separate review.

**Strategic refactoring** is planned, sometimes sprint-long or multi-sprint work that changes architecture or removes a category of problem. Requires explicit prioritisation and stakeholder buy-in.

**Examples:**

| Tactical | Strategic |
|----------|-----------|
| Rename confusing variable | Replace global config with dependency injection |
| Extract 20-line helper | Split monolith into services |
| Add missing type hint | Migrate from REST to gRPC |
| Delete dead branch | Replace in-house queue with Kafka |

**Why the distinction matters:**

- Tactical refactoring should not require approval — it's part of "doing the job well."
- Strategic refactoring *should* require approval — it's a bet with opportunity cost.
- Mixing them creates review friction. A "small PR" that includes strategic refactoring is hard to review.
- Teams that do only tactical never pay down systemic debt. Teams that do only strategic never keep the ground clean.

**Interview insight:** When discussing a refactoring success story, make clear which it was. Strategic refactoring stories are stronger for staff-level interviews — they demonstrate influence and planning. Tactical stories are better for senior-level interviews that value craftsmanship.

### Q11. What is "refactoring to patterns" and when is it appropriate?

**Answer:**

Joshua Kerievsky's *Refactoring to Patterns* (2004) argues that patterns are refactoring destinations, not starting points. You refactor *toward* a pattern when the code's duplication or rigidity justifies it — not because you decided at design time you'd use Strategy.

**Example — "Replace Conditional Dispatcher with Command":**

```python
# Starting code — smelly dispatcher
def handle_event(event):
    if event.type == "create":
        # 30 lines
    elif event.type == "update":
        # 40 lines
    elif event.type == "delete":
        # 20 lines
    # ... adding new event types requires modifying this function
```

Refactor step by step:

1. Extract each branch into its own function.
2. Collect the functions into a dispatch table.
3. Promote the dispatch table to a registry of Command objects.
4. The registry now follows the Command pattern — and adding an event type no longer modifies the dispatcher.

**When it's appropriate:**

- When the existing code shows the pain the pattern solves (duplication, violations of Open-Closed, scattered changes for each feature).
- When you've refactored halfway and the next step is naturally a pattern.

**When it's inappropriate:**

- When there's no pain — pattern for pattern's sake.
- When the pattern adds indirection without flexibility you'll actually use.

**Anti-pattern:** Pattern-hungry engineers who refactor simple code into three classes, a factory, and a strategy interface "in case we need it." YAGNI still applies.

### Q12. How do you keep refactoring safe during active feature development?

**Answer:**

Refactoring a hot area while features are in flight is high risk for conflicts and regressions. Techniques to de-risk it:

**1. Trunk-based with small PRs.** Long-lived branches conflict with refactoring. Keep changes on trunk, behind flags if needed.

**2. Parallel change (expand-contract).** Introduce the new API alongside the old. Migrate callers one by one. Remove the old only when all callers are gone.

```python
# Step 1: add new method, keep old
class UserService:
    def get_user(self, user_id):  # legacy
        return self._get(user_id)

    def find_user(self, user_id):  # new, preferred
        return self._get(user_id)

# Step 2: migrate callers incrementally over days/weeks

# Step 3: deprecate
class UserService:
    def get_user(self, user_id):
        warnings.warn("Use find_user", DeprecationWarning)
        return self.find_user(user_id)

    def find_user(self, user_id):
        return self._get(user_id)

# Step 4: delete when safe
```

**3. Feature flags.** New code paths run under a flag, compared against old. Rolled out per-user, per-cohort, or percentage.

**4. Dark launches.** Execute the new implementation in production, compare its output to the old, but don't use its result. Catches divergence with zero user risk.

**5. Automated refactoring tools.** `gofmt`, `rustfmt`, language-specific tools like `rope` (Python), `Rascal`, `Eclim`, IDE refactorings. Mechanical refactoring is less error-prone than hand-edited.

**6. Merge often.** Refactor in tiny, mergeable steps. A three-week branch to refactor a module will be incompatible with main by the time you finish.

**Reference:** *Continuous Delivery* (Humble & Farley) treats parallel change as fundamental. *Accelerate* (Forsgren et al.) shows trunk-based development correlates with high-performing teams.

---

## Advanced

### Q13. Describe a time you led a large-scale refactoring. What was the approach?

**Answer (STAR template):**

Behavioural answers should be specific. A generic "we just refactored piece by piece" is weak. Structure:

> **S:** Our order-service codebase had evolved into 80,000 lines with a 60-minute test suite and intermittent production issues. Velocity had halved year-on-year.
>
> **T:** As the senior engineer, I was asked to propose and lead improvement without stopping feature delivery.
>
> **A:**
> 1. **Quantified the pain.** Extracted hotspots from git log and bug-tracker data. Top 10 files held 70% of bugs and were modified weekly.
> 2. **Wrote an ADR.** Proposed targeting the top 5 hotspots with module extraction. Explicit non-goals: full rewrite, microservice split, language migration.
> 3. **Secured a 20% capacity allocation** from engineering leadership for two quarters, framed as "reduce change failure rate by 40%."
> 4. **Started with characterisation tests.** Wrote ~200 tests pinning current behaviour of the top hotspot — a 6,000-line pricing module. Tests ran in 3 minutes.
> 5. **Applied the Mikado Method.** Built a dependency graph; started with the leaves — extracting three pure functions, then a repository interface, then a command handler.
> 6. **Used parallel change.** New module sat alongside the old; callers migrated over six weeks; old code deleted after shadowing confirmed identical behaviour for two weeks.
> 7. **Published weekly updates.** Stakeholders saw test-coverage trend, bug trend, velocity trend.
>
> **R:** Over two quarters, change failure rate on the pricing module dropped from 22% to 6%. Mean time to review on PRs touching pricing halved. Two junior engineers led migrations of subsequent hotspots using the playbook we'd established.

**What this demonstrates:**

- Data-driven prioritisation (hotspot analysis).
- Framing for stakeholders (ADR + metrics).
- Technique literacy (Mikado, characterisation tests, parallel change).
- Scaling through others (juniors leading next iterations).
- Measurable outcomes (CFR, review time).

### Q14. How do you convince a skeptical manager that refactoring is worth the time?

**Answer:**

The manager is skeptical because they've seen refactoring stall features without evidence of payoff. The cure is evidence, not advocacy.

**Frame in business terms:**

- **Not:** "The code is ugly."
- **Instead:** "Our change failure rate on this module is 24%. Industry elite is under 15%. Here's the work I'd do to move it, and the expected ROI."

**Quantifiable metrics managers care about:**

- **DORA change failure rate.** Percentage of deploys that cause incidents.
- **Lead time.** PR open to production.
- **MTTR.** Recovery time after failure.
- **Feature velocity trend.** Is it falling? Where?
- **Bug count per module.** Where are bugs concentrated?
- **On-call incident frequency.** Where are pages coming from?

**Propose small, measurable experiments.**

Rather than "give me a refactoring quarter," propose "give me two weeks to reduce test flakiness in module X. Success metric: flake rate from 8% to under 2%." Managers approve bounded, measurable work.

**Budget the work in with features.**

- "This feature is 5 points. I recommend 8 because the area needs cleanup I'll do while I'm in there. Alternative: ship at 5 points and we'll still have the debt."

**Be honest about trade-offs.**

- "Not doing this doesn't kill us. But it's getting worse, and the cost rises each quarter."

**Reference:** *Accelerate* (Forsgren, Humble, Kim) provides the DORA framework. Managers familiar with the research are easier to convince; managers who aren't can be pointed to it.

### Q15. What are the risks of refactoring and how do you mitigate them?

**Answer:**

A mature answer names the failure modes explicitly.

**Risk 1: Scope creep.**

A "quick rename" becomes a module restructure that ships two weeks late. *Mitigation:* Explicit scope in the PR description. Aggressive self-policing — a separate PR is almost always better.

**Risk 2: Behaviour change.**

Ostensible "refactoring" silently changes behaviour. *Mitigation:* Characterisation tests first. Shadow / dark-launch if possible. Parallel change rather than replacement.

**Risk 3: Merge conflicts with active feature work.**

Long-running refactor branches become unmergeable. *Mitigation:* Trunk-based development. Small, frequent merges. Communicate with the team before starting big refactors in shared areas.

**Risk 4: Priority drift.**

Refactoring starts, then features get dropped in, and the refactor is abandoned half-done — leaving the codebase worse than before. *Mitigation:* Complete small increments to a stable checkpoint at each step. Never leave the code in an intermediate, incoherent state.

**Risk 5: Over-abstraction.**

Refactoring for "flexibility we might need" creates indirection without value. *Mitigation:* Refactor for known current pain, not speculative future needs. YAGNI.

**Risk 6: Context loss.**

Refactoring erases comments, history, or invariants that were there for a reason. *Mitigation:* Before deleting anything weird, find out why it's there. `git blame` and the original PR discussion. Preserve invariants as tests if you can.

**Risk 7: Opportunity cost.**

Time spent refactoring is time not spent on features/bug-fixes/customer research. *Mitigation:* Prioritise honestly. Not everything deserves refactoring.

**Risk 8: Political cost.**

The engineer associated with "the rewrite that failed" carries reputational damage. *Mitigation:* Preserve optionality. Never make a refactor a point of no return. Strangler fig, not big-bang.

### Q16. How do you refactor database schema changes safely in production?

**Answer:**

Schema changes are the highest-stakes refactoring most engineers do. Mistakes are visible, often irreversible, and can take the service down.

**The golden rule: every schema change is backwards-compatible at the step it ships.**

**Patterns:**

**Add column — safe if nullable or has a default:**

```sql
ALTER TABLE users ADD COLUMN email_verified BOOLEAN DEFAULT FALSE;
```

**Drop column — never in one step:**

```
1. Deploy code that stops reading the column
2. Deploy code that stops writing the column
3. Run a migration to drop the column
```

**Rename column — expand-contract:**

```
1. Add new column
2. Backfill data (code writes both; backfill job copies old -> new)
3. Switch reads to new column
4. Stop writes to old column
5. Drop old column
```

**Change type — same pattern: add new, backfill, migrate reads, migrate writes, drop old.**

**Split / merge tables — Strangler Fig applied to data:**

- Code writes to both old and new.
- Reads gradually shift.
- Eventually old is read-only, then retired.

**Never:**

- Drop a column that code still writes.
- Run a long `ALTER TABLE` on a hot table (may lock for hours).
- Deploy code that depends on a schema change in the same release as the migration. Decouple.

**Tooling:**

- **Zero-downtime migrations:** `pt-online-schema-change`, `gh-ost`, Liquibase, Flyway with multi-step plans.
- **Shadow reads/writes:** Compare old vs new in production with metrics.
- **Reversible migrations:** Every migration has a tested rollback.

**Interview insight:** Mention that you always write a rollback plan before approving the migration. Many engineers don't, and it's a common incident cause.

### Q17. What do you do when you're partway through a refactor and realise the approach is wrong?

**Answer:**

The sunk-cost fallacy is the dominant danger. The senior answer is to evaluate from the current state, not from the starting state.

**Honest assessment questions:**

1. What have I learned that I didn't know when I started?
2. If I were starting today, with what I now know, what would I do?
3. What's the cost of finishing the current path vs. the cost of reverting and restarting?
4. Is this a local stuck point (solvable by trying harder) or a fundamental issue (the design is wrong)?

**Options and when to use them:**

- **Abandon and revert.** Best if you're <25% done and the realisation is deep. Lose a few days; save weeks.
- **Finish and revisit.** Sometimes finishing gets you to a checkpoint from which you can evaluate properly. Use when the current increment is small.
- **Pivot.** Change direction at the current state. Keep what's useful; adjust the plan. Use when partial progress has lasting value.
- **Pair up.** If you're stuck, a second brain often unblocks. Don't pivot alone.

**Communicate early.** If you told stakeholders "two weeks to X," and now it's three weeks and you want to change direction, tell them today, not next Friday. Small surprises now beat big surprises later.

**Document the dead end.** A one-paragraph note on the failed approach is valuable — the next person (or future you) will not repeat it.

**Interview signal:** Candidates who describe iteration and course-correction are showing intellectual honesty. Candidates who claim they've never hit this problem are either lying or inexperienced.

### Q18. How do you handle refactoring code written by someone still on the team?

**Answer:**

Refactoring is personal. If done carelessly, "improving" someone's code damages the relationship. The technical work and the social work are both mandatory.

**Before you touch the code:**

1. **Loop them in early.** "I'm thinking of extracting X into a module — I wanted to talk through it with you before I write anything."
2. **Respect what exists.** The original code might have reasons you don't see. Ask. "Why did you choose to couple these two things?" is honest, not loaded.
3. **Credit decisions that aged well.** "The queueing design you put in last year scaled beautifully; I'm leaving that alone."

**During the refactor:**

1. **Include them as a reviewer.** Their context is invaluable. Refactoring without the author's input misses details.
2. **Be scrupulous in the PR description.** "Goal: reduce coupling between X and Y. Behaviour should be unchanged. Ran these tests. No hot-path performance change."
3. **Avoid diff-noise commits.** Rename and reshape in separate commits so diffs are readable.

**If you disagree with their original design:**

1. **Say it as a current problem, not a historical error.** "The current coupling makes testing hard" is better than "this was a bad design."
2. **Take responsibility for the new shape.** You're proposing a design; own its trade-offs.
3. **Listen for the third option.** Often the conversation surfaces a better approach than either party's initial idea.

**Interview insight:** This is a question about collaboration as much as code. Candidates who talk only about the technical mechanics and not about the author reveal a gap in their people skills.

### Q19. What tools or techniques would you use to refactor a 1,000,000-line codebase?

**Answer:**

Individual refactorings don't scale past some size — you need programmatic tools and systematic approaches.

**Tools:**

- **LSP-based IDE refactorings.** VSCode, IntelliJ, Pycharm — safe for small changes, rename, extract.
- **AST manipulation tools:**
  - Python: `libcst`, `bowler`, `rope`
  - JavaScript/TypeScript: `jscodeshift`, `ts-morph`
  - Go: `gofmt -r`, `go fix`, `gorename`
  - Java: OpenRewrite, Error Prone
  - Multi-language: Semgrep for pattern-based transformations
- **Refactoring-at-scale tools:** Google's Rosie / LSC (Large-Scale Change) infrastructure, Meta's Fastmod.
- **Deprecation tooling:** Compiler warnings, linter rules, error budgets enforced in CI.

**Techniques:**

1. **Automate the repetitive.** If you'll rename 500 call sites, don't hand-edit 500 sites. Write a codemod.

2. **Migrate by package, not globally.** A global rename leaves no safe intermediate state. Package-by-package leaves each package green.

3. **Type-first when possible.** A gradual type migration (Python → mypy, JS → TS) catches whole classes of bugs as you go.

4. **Lint for the old pattern.** Once the new pattern exists, add a lint rule forbidding the old pattern in new code. Debt stops growing even before you pay it down.

5. **Incremental rollout with flags.** The rewrite runs under a flag; rollout is controlled.

6. **Ownership and communication.** At million-line scale, other teams' code depends on yours. Telegraph changes through ADRs and migration guides well in advance.

**Reference:** Hyrum's Law — "with a sufficient number of users, every observable behaviour of your system will be depended on by someone." Large-scale refactoring is primarily a problem of managing implicit contracts, not of code mechanics.

**Example:** Google's atomic-large-scale-change tooling (described in *Software Engineering at Google*) has made million-scale refactors routine. Meta's "monorepo tooling" is analogous.

### Q20. How do you refactor a module that has no owner and everyone is afraid to touch?

**Answer:**

This is a classic "orphaned code" problem. The code sits in critical paths, no one wants to own it, but it accumulates bugs and blocks progress.

**Diagnose before acting:**

- **Why is it orphaned?** Original author left, political dispute over ownership, or honest capability gap?
- **What's actually in it?** Often the code is less complex than its reputation; fear is overstated.
- **What's production risk today?** If it breaks, who pages? Even orphaned code has an incident playbook somewhere.

**Steps:**

1. **Establish observability.** Add metrics, logs, traces if missing. You cannot refactor what you cannot see.

2. **Write characterisation tests.** Even if coverage is thin, pin the visible behaviour. Use shadow mode or replay of production traffic if possible.

3. **Propose an owner.** Someone needs accountability. Propose yourself or identify the team whose domain it most fits. Ambiguous ownership is how it became orphaned.

4. **Document what you learn.** As you explore, write an ADR or onboarding doc. The next engineer shouldn't start from zero.

5. **Small changes first.** Build trust by making small, safe changes that don't break anything. Each successful change reduces team fear.

6. **Socialise learnings.** A tech talk, a brown-bag, a team demo — share that you now know this code. The social value is large.

7. **Formalise ownership.** Ensure CODEOWNERS or equivalent reflects reality. Any future PR pings the right team.

**Interview insight:** Answers that lead with "I'd rewrite it" reveal naivety. Orphaned code is usually not technically hopeless — it's socially orphaned, and the first fix is social.

### Q21. Describe the "broken windows" theory as applied to code.

**Answer:**

From *The Pragmatic Programmer* (Hunt & Thomas): quality deteriorates when visible signs of neglect accumulate. One broken window signals "no one cares," which licenses more broken windows.

In code, "broken windows" include:

- TODO comments that have been there two years.
- Commented-out code never deleted.
- Failing tests marked as "skipped."
- Lint warnings ignored.
- Dead code paths no one dares delete.
- Out-of-date documentation.
- CI failures that the team has learned to ignore.

**The effect:** New engineers arriving see the standard. "If they leave commented-out code, I can too." Quality compounds downward.

**Interventions:**

1. **Fix broken windows early.** Delete dead code on sight. Close or act on TODOs. Unskip tests or delete them.
2. **Don't tolerate ignored CI failures.** If a test is flaky, fix it or delete it — never normalise "retry until green."
3. **Tooling enforces minimums.** Linters with errors, not warnings. CI that fails on regressions.
4. **Celebrate cleanup.** If a teammate deletes 2,000 lines of dead code, make it visible. If quality work is invisible and feature work is visible, no one does quality work.

**Counter-example:** Over-zealous window-fixing becomes bike-shedding. Don't spend a week deleting TODOs while a production bug languishes. Proportion matters.

**Interview insight:** This question tests whether candidates see code quality as a cultural artifact, not just a technical one. The social dimension — that standards are set by what the team tolerates, not what it documents — is the mature answer.

### Q22. How do you leave a codebase better than you found it when you're leaving the team?

**Answer:**

A senior engineer's legacy is what remains after they leave. Preparing for departure is a leadership act.

**In the last month:**

1. **Write down the things only you know.** System quirks, workarounds, on-call intuition, historical decisions. A one-page "brain dump" doc per area.

2. **Update ADRs.** Decisions that happened informally should be written down. Future engineers inherit the reasoning, not just the code.

3. **Document the "whys."** Any clever code should have a comment explaining why it's clever. `git blame` is not enough — it shows who wrote it, not why.

4. **Pair heavily on your remaining work.** Every PR should have a co-driver who will understand the change after you leave.

5. **Introduce your successor to stakeholders.** Warm handoffs to partner teams, vendors, and on-call rotations.

6. **Close out TODOs honestly.** Either do them, delete them, or reassign them with dates.

**In the longer arc of your time on the team:**

1. **Spread context continuously.** Don't be the SPOF for critical knowledge. If you're the only one who understands the billing module, that's a failure even before you leave.

2. **Document as you go.** ADRs are written when decisions are made, not six months later.

3. **Mentor successors.** Someone should be able to step into your role. If no one can, you've been hoarding impact, not building it.

4. **Automate what you can.** Runbooks become scripts. Scripts become CI. CI becomes invisible infrastructure.

**Interview insight:** Staff+ candidates describe this proactively — "I measure my impact partly by what runs without me." Senior candidates who say "they'll be fine" when asked about their legacy often reveal a narrower view of senior work.

**Reference:** *The Staff Engineer's Path* (Tanya Reilly) discusses "you and your code vs. the code that outlasts you" as a framing for senior impact.
