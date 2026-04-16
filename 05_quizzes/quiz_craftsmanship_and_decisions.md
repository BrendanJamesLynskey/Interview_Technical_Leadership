# Quiz — Craftsmanship and Technical Decisions

**Subject:** Technical Leadership
**Topics covered:** Code Review, Refactoring, Legacy Code, Architecture Decisions, Technical Debt, Estimation and Planning
**Format:** Multiple-choice and short-answer questions. Answers at the end.

---

### Q1. What is the primary goal of a code review, according to research such as *Software Engineering at Google*?

a) Finding bugs before they reach production
b) Knowledge sharing and design feedback
c) Enforcing code style consistency
d) Preventing developers from shipping broken code

---

### Q2. In a code review, what does the Conventional Comments prefix `nit:` typically signal?

---

### Q3. What is the approximate size beyond which code review quality drops sharply, according to the Cisco SmartBear study and similar research?

a) Over 50 lines
b) Over 100 lines
c) Over 200–400 lines
d) Over 1,000 lines

---

### Q4. Name three biases a reviewer may bring to code review and give one mitigation for each.

---

### Q5. What is Michael Feathers' definition of "legacy code"?

a) Code older than five years
b) Code written in an outdated language
c) Code without tests
d) Code the original author has left the company

---

### Q6. What is a characterisation test and when is it used?

---

### Q7. The Strangler Fig pattern is most useful when:

a) You need to add features to an existing system
b) You need to replace an existing system incrementally while keeping it in production
c) You need to rewrite a system from scratch
d) You need to delete unused code

---

### Q8. What is the key principle of refactoring, as Martin Fowler defines it?

---

### Q9. What is an Architecture Decision Record (ADR)?

a) A ticket in a project-management system tracking architectural work
b) A short document capturing a decision, its context, options considered, and reasoning
c) A detailed technical specification
d) A runbook for architecture changes

---

### Q10. What is the key difference between an RFC and an ADR?

a) RFCs are longer than ADRs
b) RFCs are written before a decision (to solicit input); ADRs are written after (to document what was decided)
c) ADRs are used by architects; RFCs are used by engineers
d) They are identical

---

### Q11. What is a "two-way door" decision (per Amazon's framing)?

a) A decision that requires two approvers
b) A decision that is easily reversible
c) A decision that requires two meetings to finalise
d) A decision involving two teams

---

### Q12. When should you NOT write an ADR?

---

### Q13. Which of the following is NOT one of the DORA metrics?

a) Deployment frequency
b) Lead time for changes
c) Number of developers
d) Change failure rate

---

### Q14. What are the four quadrants of Martin Fowler's technical debt taxonomy?

---

### Q15. What does "interest" on technical debt mean in practice?

a) The rate at which the debt grows
b) The ongoing cost incurred by not having fixed the debt — slower velocity, more bugs, higher operational pain
c) The fee developers charge to work on the debt
d) The decay rate of the code

---

### Q16. You inherit a legacy service with no tests and significant production issues. What is the first step you'd take before refactoring?

---

### Q17. Which of the following is the best way to communicate an estimate with uncertainty?

a) "This will take 3 weeks."
b) "This will take about 3 weeks."
c) "Best case 2 weeks, most likely 3 weeks, worst case 6 weeks."
d) "I'll let you know when it's done."

---

### Q18. What is a "build vs buy" analysis and what are the two most common mistakes made in it?

---

### Q19. What is the Gall's Law perspective on complex systems?

a) Complex systems should be designed from scratch
b) Complex systems should always be built from simple working systems, not designed complex from the start
c) Complex systems require large teams
d) Complex systems cannot be tested

---

### Q20. What is the YAGNI principle and when might you ignore it?

---

### Q21. "Prudent and deliberate" technical debt, in Fowler's quadrant, describes:

a) Debt taken on by mistake and then not fixed
b) Debt taken on knowingly, as a conscious trade-off for speed, with a plan to address it
c) Debt caused by outdated libraries
d) Debt that accumulates over time from inaction

---

### Q22. A junior engineer submits a PR where the overall design is wrong. What is the appropriate first step?

a) Reject the PR with detailed line-by-line feedback
b) Approve it with suggestions to refactor later
c) Stop commenting inline and move to a synchronous conversation to understand their approach
d) Escalate to their manager

---

### Q23. What is the purpose of a "pre-mortem" in planning?

---

### Q24. The phrase "strong opinions, weakly held" means:

a) Have no opinions until you have data
b) Form confident views, but be genuinely willing to update them on new information
c) Hold opinions strongly regardless of feedback
d) Defer to senior engineers

---

### Q25. Give an example of a decision that should be an ADR vs. one that should NOT be.

---

## Answers

**A1.** (b). Knowledge sharing and design feedback are the dominant values of code review. Bug-finding is a side effect, not the primary goal. Google's research via *Software Engineering at Google* establishes this explicitly.

**A2.** `nit:` signals a minor, non-blocking style preference that the author is free to adopt or dismiss. It's part of the Conventional Comments convention and exists specifically to remove ambiguity about whether a comment is a blocker.

**A3.** (c). Review quality drops sharply beyond roughly 200–400 lines of diff. Reviewers start skimming; bugs slip through. Smaller PRs review faster and with higher quality.

**A4.** Examples: **Author-name bias** (stricter on juniors, lenient on seniors) — mitigated by blinded review or pair reviewers. **Hindsight bias** ("I'd have done it differently") — mitigated by asking "would I block on this, or is it just preference?". **Recency bias** (recent incidents over-influence current review) — mitigated by applying policies consistently. **Anchoring** (first issue colours the whole review) — mitigated by doing a full read-through before commenting.

**A5.** (c). Feathers' definition is "code without tests." The key property is not age or style — it's the lack of a safety net that makes changes confident.

**A6.** A characterisation test documents what legacy code *currently does* (not what it was intended to do). You write one before refactoring untested code: observe the current behaviour, lock it in with tests, then refactor with confidence that behaviour is preserved.

**A7.** (b). The Strangler Fig pattern (named by Martin Fowler after the strangler fig tree) incrementally replaces an existing system by building new functionality around it, gradually routing traffic away, and eventually removing the old system. It keeps the system in production throughout.

**A8.** Refactoring is a disciplined technique for restructuring existing code, altering its internal structure without changing its external behaviour. Key properties: behaviour-preserving, small steps, tests as safety net.

**A9.** (b). An ADR is a short document (typically 1–3 pages) capturing a single architectural decision — the context, options considered, the decision, and its consequences. Named by Michael Nygard in 2011.

**A10.** (b). RFCs are written before a decision to solicit input and build consensus. ADRs are written after a decision to document what was decided and why.

**A11.** (b). A two-way door decision is easily reversible — you can walk back through it cheaply if you change your mind. One-way doors are expensive to reverse. Amazon uses this framing to calibrate decision speed: two-way doors should be made quickly and locally; one-way doors deserve more deliberation.

**A12.** Don't write an ADR for: trivial or reversible choices; implementation details that don't affect contracts; decisions captured better in code (naming, formatting); purely stylistic preferences. Scarcity preserves the value of ADRs — one-ADR-per-decision keeps them readable.

**A13.** (c). DORA's four metrics are: deployment frequency, lead time for changes, change failure rate, and mean time to restore service (MTTR). Number of developers is not a DORA metric.

**A14.** Fowler's technical debt quadrant: **Reckless/Deliberate** ("we don't have time for design"), **Reckless/Inadvertent** ("what's layering?"), **Prudent/Deliberate** ("we must ship now and deal with the consequences"), **Prudent/Inadvertent** ("now we know how we should have done it"). The quadrants arise from two axes: deliberate vs inadvertent, reckless vs prudent.

**A15.** (b). Interest is the ongoing cost you pay for not having fixed the debt: slower development, more bugs, higher operational pain, more time explaining legacy quirks. Like financial interest, it compounds.

**A16.** Write characterisation tests. You need a safety net before you can refactor anything with confidence. Observe what the system does today (even the surprising bits), lock it in with tests, and only then begin structural changes.

**A17.** (c). A range with best/likely/worst cases communicates uncertainty honestly. Point estimates hide uncertainty and breed false confidence. The PERT/three-point estimation approach is the formal version of this.

**A18.** Build vs buy analysis is the assessment of whether to build something in-house or buy it from a vendor/use open source. Common mistakes: (1) underestimating the ongoing maintenance cost of building (tooling, docs, support, upgrades); (2) ignoring the integration and customisation cost of buying. Both are TCO (total cost of ownership) misses.

**A19.** (b). Gall's Law: "A complex system that works is invariably found to have evolved from a simple system that worked. A complex system designed from scratch never works and cannot be patched up to make it work." The practical implication: start with something simple that works, grow it incrementally.

**A20.** YAGNI ("You Aren't Gonna Need It") says don't build features, abstractions, or flexibility until there's a concrete need. You might ignore it when: (1) a specific concrete need is imminent; (2) the cost of adding it later is genuinely much higher than adding it now (rare, but real for e.g. internationalisation primitives); (3) you have data (not speculation) about the future need.

**A21.** (b). Prudent and deliberate debt is taken on knowingly, with understanding of the trade-off and a plan to address it. It's the "healthiest" form of debt. The other quadrants represent accidental or reckless accumulation.

**A22.** (c). Stop inline commenting; move to synchronous. Line-by-line feedback on code that shouldn't exist is wasted effort. A conversation saves both engineers' time and avoids making the author feel piled-on. If the design is wrong, start with understanding why they took that approach before criticising.

**A23.** A pre-mortem is a planning exercise where the team imagines the project has failed and writes down why. It surfaces risks and concerns that people would hesitate to voice as predictions. Gary Klein, who formalised the technique, found it produces 30% more ideas about risks than unstructured risk discussion.

**A24.** (b). "Strong opinions, weakly held" (attributed to Paul Saffo) means you have confident views (you're willing to make calls and defend them) but you genuinely update them when new information arrives. Not having opinions is paralysis; not updating opinions is dogma.

**A25.** **Should be an ADR:** Choice of primary database (hard to reverse, wide impact, non-obvious reasons); choice of authentication library (affects security, touched by many teams); service boundary decisions (shapes the org). **Should NOT be an ADR:** Variable naming convention (captured in style guide); whether to use tabs or spaces (captured in formatter config); which specific colour to use for a log-level (implementation detail).
