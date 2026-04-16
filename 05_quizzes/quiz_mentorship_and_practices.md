# Quiz — Mentorship, Collaboration, and Engineering Practices

**Subject:** Technical Leadership
**Topics covered:** Mentorship, cross-team collaboration, incident leadership, writing and communication, behavioural scenarios
**Format:** Multiple-choice and short-answer questions. Answers at the end.

---

### Q1. What is the difference between mentorship and sponsorship?

---

### Q2. A 1:1 with a direct report should primarily be:

a) A status update from the report to the manager
b) A conversation driven by the report's agenda — career, blockers, feedback
c) A performance review
d) A team planning meeting

---

### Q3. An engineer on your team is consistently late to stand-up. Which approach is most appropriate first?

a) Raise it in their next performance review
b) Discuss privately in a 1:1 to understand the cause before escalating
c) Publicly call them out in stand-up
d) Reassign them to another team

---

### Q4. Which of the following is a characteristic of an effective engineering ladder?

---

### Q5. What is the purpose of a "stretch project" for a growing engineer?

---

### Q6. "Influence without authority" means:

a) Being promoted to management
b) Using formal authority sparingly
c) Getting cross-team commitment and alignment without direct reporting relationships
d) Avoiding conflict

---

### Q7. When two teams disagree on a shared architectural choice, which approach is most effective?

---

### Q8. What is a RACI matrix and when should you build one?

---

### Q9. As a technical leader, which is your primary job during a SEV1 incident (assuming you are NOT the Incident Commander)?

a) Debug the issue yourself
b) Provide situational context, unblock responders, and handle stakeholder/executive communication so the IC can focus
c) Dictate the response plan
d) Write the postmortem during the incident

---

### Q10. What is "psychological safety" and why does it matter for incident response?

---

### Q11. The BLUF principle in written communication stands for:

a) Big List Under Footer
b) Bottom Line Up Front
c) Brief, Linked, Uncoloured, Formal
d) Brevity, Language, Urgency, Frame

---

### Q12. A design document should typically open with:

a) A detailed technical architecture diagram
b) A summary of the problem, proposed solution, and expected outcome, with decisions and trade-offs following
c) A list of contributors
d) The implementation timeline

---

### Q13. What is the primary purpose of an RFC (Request for Comments)?

---

### Q14. Which of these is the weakest form of engineering communication?

a) Recorded video walkthrough with accompanying doc
b) Async doc review with inline comments
c) A long Slack thread in #general
d) A scheduled design review meeting with a pre-read doc

---

### Q15. A behavioural interview question begins: "Tell me about a time you disagreed with a peer." Which framework structures the answer effectively?

---

### Q16. "Bias for action" as a leadership principle means:

a) Ignoring data in favour of intuition
b) Acting quickly when the cost of delay is high, accepting reversible risks
c) Avoiding meetings
d) Writing code before writing design docs

---

### Q17. Which of these is NOT a good onboarding practice?

a) Assigning a buddy
b) Clear 30/60/90-day goals
c) Shipping a small, meaningful change in the first week
d) Expecting full productivity in week one

---

### Q18. An executive asks you during an ongoing outage: "When will it be fixed?" — you don't know. What is the best response?

---

### Q19. What is "sponsorship" as distinct from mentorship?

---

### Q20. Which is the strongest indicator that a Staff+ engineer is being effective?

a) Their personal commit count is the highest on the team
b) They are the only one who understands a critical system
c) Their team and adjacent teams ship faster, safer, and with better judgement because of their influence
d) They attend the most meetings

---

### Q21. Describe the "disagree and commit" principle.

---

### Q22. What is a "technical critique" and how does it differ from a code review?

---

### Q23. Which of the following is the hallmark of a good async communicator?

---

### Q24. A peer is stuck on a problem and frustrated. As their mentor, what is the most effective intervention?

a) Solve it for them
b) Ignore it — they should be self-sufficient
c) Pair with them briefly to model problem-solving, then step back to let them complete it
d) Escalate to their manager

---

### Q25. In the STAR format for behavioural answers, what does each letter stand for?

---

## Answers

**A1.** Mentorship is guidance — sharing knowledge, advice, perspective. Sponsorship is advocacy — actively putting the person's name forward for opportunities (projects, promotions, speaking) in rooms they aren't in. Mentorship helps people grow; sponsorship helps them advance. Both are needed; sponsorship is usually scarcer.

**A2.** (b). 1:1s are for the report. The manager's job is to listen, remove blockers, give feedback, and discuss career. Status belongs in other channels (standups, docs, PRs). Filling 1:1s with status is the most common mistake.

**A3.** (b). Start by understanding the cause in a private 1:1. Reasons range from timezone issues, to childcare, to disengagement. Each has a different response. Public escalation (c), performance review (a), or reassignment (d) without context damages trust and may miss the real issue.

**A4.** An effective engineering ladder has: distinct levels with clear behavioural indicators, orthogonal axes (technical depth vs scope/impact), examples per level, a way to progress without management, and calibration across the organisation. It should describe what engineers do at each level, not just what they've completed.

**A5.** A stretch project gives an engineer scope slightly beyond their current level — enough that they'll need to grow new skills, seek help, and operate with uncertainty, but not so much that they're set up to fail. It's the primary mechanism for growth from one level to the next.

**A6.** (c). Influence without authority is the core skill of senior/staff engineers: getting other teams to do things by credibility, relationships, clear writing, and alignment on shared goals — not by issuing orders.

**A7.** Move from opinion to principle: what does each team value and why? Often a good design respects both concerns and can be found. If a real trade-off exists, document the options with trade-offs, have a decision owner (named escalation path), and commit to the outcome even if you disagreed. Don't let disagreements become stalemates.

**A8.** RACI = Responsible, Accountable, Consulted, Informed. Build one when a cross-team initiative has ambiguous ownership, when multiple teams think they're driving the same work, or when accountability is unclear. It forces explicit decisions about who does what; that's where the value is, not the document itself.

**A9.** (b). A senior technical leader's highest leverage during an incident (when not IC) is to provide context (historical knowledge, dependency maps), unblock responders (give them authority/resources), and handle upward communication so the IC has protected bandwidth. Trying to debug competes with the IC; dictating (c) creates dysfunction.

**A10.** Psychological safety is the shared belief that the team is safe for interpersonal risk-taking — admitting mistakes, asking questions, disagreeing publicly. During incidents, responders must report "I just ran this command and it made things worse" without fear. Without safety, facts get hidden and incidents drag on.

**A11.** (b). Bottom Line Up Front. State the main point, decision, or recommendation in the first sentence or paragraph, then provide detail. Executives and busy readers get the answer immediately; detail is available on demand.

**A12.** (b). A design doc should state the problem, the proposed solution, and the expected outcome up front, followed by context, alternatives considered, trade-offs, and implementation details. Diagrams support the narrative but don't open it.

**A13.** An RFC proposes a change (technical or process), invites critique from stakeholders, documents alternatives considered, and creates a durable record of the decision and rationale. Value is in the deliberation process and the written trail — not just in the final decision.

**A14.** (c). A long Slack thread is the weakest: ephemeral, un-linkable, chronological (can't restructure), hard for new readers to follow, and gets lost. Move substantive discussions to a doc and link the doc from chat.

**A15.** STAR: Situation (context), Task (your responsibility/goal), Action (what you did — specifically you, not "we"), Result (outcome, measured). For disagreement specifically, extend with reflection: what you learned, how you'd approach similarly next time.

**A16.** (b). Bias for action means recognising that for reversible, low-cost decisions, waiting for perfect information is itself a cost. Move quickly, observe outcomes, adjust. Distinct from "shoot from the hip" — decisions are informed, just fast.

**A17.** (d). Expecting full productivity in week one is an anti-pattern that creates anxiety and shallow understanding. Onboarding invests in context that pays off for months. First shipped change should be small (a), with a buddy (a), and clear goals (b).

**A18.** "I don't have a reliable ETA yet. Here's what we know, what we're doing now, and what we'll know next. Next update in [specific time]." Honesty about uncertainty builds credibility; fake estimates erode it. Executives want a next-update time even if you can't promise a fix time.

**A19.** Sponsorship is going into a room — promotion committee, project selection, leadership nomination — and advocating actively for someone who isn't in that room. It requires political capital and willingness to put your name behind theirs. It's harder than mentorship and often more impactful.

**A20.** (c). Staff+ impact is measured by the multiplier effect on others — teams around them ship faster, avoid known mistakes, make better architectural decisions. Personal output (a) is an individual contributor signal. Bus-factor-of-one (b) is a failure mode. Meetings (d) are an input, not output.

**A21.** Disagree and commit: after the decision is made (through a fair process), even dissenting voices commit to making it succeed rather than undermining it. This combines honest debate with organisational execution. Continuing to fight the decision after it's made is the failure mode to avoid.

**A22.** A technical critique reviews an approach (design doc, architecture proposal) before implementation — evaluating choices, surfacing risks, exploring alternatives. A code review reviews an implementation against an agreed approach. Critiques are more expensive to act on late; cheap early. Doing critiques properly is a leadership skill.

**A23.** Good async communicators: write clearly with BLUF, include all necessary context (no "as we discussed"), state what response is needed and by when, link artefacts not re-explain, and don't expect synchronous responses outside agreed hours.

**A24.** (c). Pair briefly to model the approach (what questions to ask, what to debug next), then step back. Doing it for them (a) removes the learning. Ignoring (b) abandons the mentorship. Escalating (d) turns a learning moment into a performance issue.

**A25.** STAR: Situation, Task, Action, Result. Situation sets context (what, where, when). Task states your specific role. Action describes what you personally did (use "I" not "we"). Result measures the outcome with concrete metrics. Many behavioural answers fail by being vague on Action or missing Result.
