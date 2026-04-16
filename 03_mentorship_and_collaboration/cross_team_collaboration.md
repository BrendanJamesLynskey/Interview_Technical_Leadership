# Cross-Team Collaboration — Interview Questions

**Subject:** Technical Leadership
**Topic:** Working Across Teams, Stakeholder Management, Influence Without Authority, Disagreement, RACI
**Difficulty tiers:** Fundamentals / Intermediate / Advanced

---

## Fundamentals

### Q1. What does "influence without authority" mean and why is it a senior-engineer skill?

**Answer:**

**Influence without authority** is the ability to drive outcomes through people who do not report to you and whose work you cannot directly assign. It is the dominant mode of work for staff and principal engineers, and a major part of the senior-engineer job.

**Why it's the staff-track skill:**

- A staff engineer's scope is usually multiple teams. None of them report to them.
- The decisions they care about (architecture, quality bar, technical strategy) cut across team boundaries.
- They have to *convince* people, *enable* people, and *make it easier to do the right thing* — they cannot order it.

**The toolkit:**

1. **Trust capital.** Built slowly through reliability, expertise, and reciprocity. Spent in the moments that matter.
2. **Written artefacts.** A well-written RFC carries influence further than the author can travel.
3. **Relationships.** The people who'll help you in a crunch are the ones you helped without an immediate ask.
4. **Forums.** Being in the room (architecture council, tech leads' channel, design reviews) where decisions get made.
5. **Sponsorship from above.** A senior leader's "I trust this person" carries you across team lines.
6. **Removing friction.** Make the right thing easier than the wrong thing. People follow the path of least resistance.

**Anti-patterns:**

- **Pulling rank that you don't have.** "I'm a staff engineer" doesn't move people who report to a different staff engineer.
- **Mandates without buy-in.** Architecture diktats from on high get ignored at execution time.
- **Burning capital on small things.** Save your influence for the decisions that matter.

**Reference:** *Influence Without Authority* (Cohen & Bradford) is the classic. *The Staff Engineer's Path* (Tanya Reilly) covers the engineering application. *An Elegant Puzzle* (Will Larson) on org dynamics.

### Q2. What is RACI and how does it help in cross-team work?

**Answer:**

**RACI** is a responsibility-assignment matrix that disambiguates who does what on a piece of work. The acronym:

- **R — Responsible.** The people who do the actual work.
- **A — Accountable.** The single person who owns the outcome. Exactly one person, by convention.
- **C — Consulted.** People whose input is needed before decisions are made. Two-way conversation.
- **I — Informed.** People who need to know the outcome. One-way communication.

**Why it matters in cross-team work:**

When two teams work together without clarity, the predictable failures are:

- Both think the other team is doing X. X doesn't get done.
- Both think they should decide Y. Y is decided twice, incompatibly.
- A stakeholder finds out about Z too late and forces rework.

A RACI matrix surfaces these before they become incidents.

**Example:**

| Activity | Platform Team | App Team | Security | Product |
|----------|---------------|----------|----------|---------|
| Design auth library | A, R | C | C | I |
| Migrate apps to library | C | A, R | I | I |
| Approve API surface | C | C | A, R | I |
| Comms to customers | I | I | I | A, R |

**Common pitfalls:**

- **Multiple A's.** "We're both accountable" means no one is accountable.
- **R = A everywhere.** Responsible engineers don't always have decision authority. Splitting them surfaces the gap.
- **Treating it as bureaucracy.** RACI is a 30-minute conversation, not a quarterly artefact. Done badly, it's worse than nothing.
- **Building it once, never updating it.** As the project changes, the matrix needs to change.

**When NOT to use RACI:** Small, single-team work; well-trusted teams who already have working norms; throwaway projects. RACI is overhead — use it when the cost of confusion exceeds the cost of formality.

### Q3. What is a stakeholder and how do you map yours?

**Answer:**

A **stakeholder** is anyone whose work or outcomes are affected by yours — directly or indirectly. For a senior engineer running cross-team work, stakeholders typically include other engineering teams, product managers, designers, security, SRE/ops, legal/compliance, customer-facing teams, and senior leadership.

**Why mapping them matters:**

Unmapped stakeholders show up late, surprised, and often blocking. The most common cross-team failure mode is "we shipped the thing and X team blocked launch because they weren't consulted."

**A simple stakeholder map:**

| Stakeholder | Interest | Power | Engagement strategy |
|-------------|----------|-------|---------------------|
| Security team | High (auth changes) | Veto power on ship | Consulted from week 1 |
| Customer success | Medium | None directly | Informed at milestones |
| VP Engineering | Low (until something fails) | Override authority | Informed monthly |
| Sister product team | High (shared dependency) | Could fork the lib | Consulted on API |

**Power × Interest** is the standard 2×2:

- **High power, high interest:** Manage closely. They are partners.
- **High power, low interest:** Keep satisfied. Don't surprise them.
- **Low power, high interest:** Keep informed. Their goodwill matters at execution.
- **Low power, low interest:** Monitor. Don't waste your time or theirs.

**The senior failure mode:** Mapping only the people you like working with. The stakeholder you've been avoiding is the one who'll block you.

**Reference:** *Influence Without Authority* (Cohen & Bradford). *Crucial Conversations* (Patterson et al.) on managing the high-stakes ones. The Mendelow stakeholder matrix is the academic anchor.

### Q4. What's the difference between a "consulted" and an "informed" stakeholder, and why does mixing them up cost you?

**Answer:**

A **consulted** stakeholder gets a say *before* a decision is made. An **informed** stakeholder gets told *after* the decision is made. The difference seems small; the consequences are not.

**The cost of treating "consulted" as "informed":**

- The stakeholder learns about the decision after the fact.
- They feel ignored — because they were.
- They use whatever leverage they have (escalation, blocking review, public complaint) to reverse or delay.
- Your decision is reopened, often after work has begun. Sunk cost increases the friction.

**The cost of treating "informed" as "consulted":**

- Decision-making becomes a committee.
- Trivial decisions take weeks because everyone has to weigh in.
- Senior engineers learn to make important decisions in hidden channels to avoid the overhead.
- Trust erodes — people are consulted but their input is never used.

**Heuristics for which is which:**

- **Consulted:** Has expertise you need; has authority to block; will be materially affected; will have to maintain it.
- **Informed:** Needs to know for awareness, planning, or messaging; has no authority; isn't materially affected.

**Practical phrasing:**

- "Hi X, before we finalise this we'd like your input by Friday." (Consulted.)
- "Hi X, we've decided to do this — wanted you to hear it from us before the announcement." (Informed.)

The phrasing makes it explicit. Stakeholders who think they were consulted but were actually informed will object even if they wouldn't have objected in real consultation. Clarity reduces friction.

### Q5. How do you negotiate scope between two teams with conflicting priorities?

**Answer:**

This is the daily reality of cross-team work. Two teams with shared dependencies will, at some point, want incompatible things at the same time.

**Step 1: Get the facts on the table.**

- What does each team actually need? Often the stated need ("we need feature X") and the underlying need ("we need to make our SLO this quarter") are different.
- What's the cost of doing each? Engineering time, complexity, opportunity cost.
- What's the cost of *not* doing each? Risk, customer impact, blocked downstream work.

**Step 2: Find the shared goal.**

Both teams (almost always) report up to a shared leader and serve a shared customer. Reframe the disagreement around the upstream goal: "We both care about delivering the migration; how do we sequence to do that?"

**Step 3: Look for non-zero-sum trades.**

- Phasing: A team gets what they need first; the other team gets what they need second.
- Scope reduction: What's the minimum each team needs to be unblocked?
- Resourcing: Can one team contribute engineers to the other's work?
- Time-shifting: Does one need it now, the other in a quarter?

**Step 4: Escalate as a partnership, not as adversaries.**

If you can't agree, escalate together with a recommendation. "Both teams have looked at this and we can't reach agreement; here are the two options and the trade-offs we see." Joint escalation is much faster to resolve than dueling escalations from each side.

**Step 5: Document the agreement.**

Write down what was decided and why. Memory is short, especially across teams; "we agreed last quarter" is unrecognised by the team that wasn't in the meeting.

**STAR framing:**

> **S:** Two teams I worked with both depended on the platform team's auth library. The platform team had committed to a major rewrite that would block both of them for a quarter.
> **T:** As staff engineer for the platform org, I had to broker an outcome that didn't stall both consuming teams.
> **A:** I ran a session with the leads of all three teams. I framed it around the shared goal — moving the org off the legacy auth system. We agreed to phase: keep the existing library running with maintenance freezes; deliver the new library behind a feature flag; migrate one consuming team at a time on a quarterly cadence.
> **R:** Both consuming teams hit their goals; the platform team finished the rewrite without rushing; we avoided the all-or-nothing failure mode. The phasing pattern became a template we used for two later platform migrations.

### Q6. How do you handle a disagreement with another senior engineer?

**Answer:**

Senior-on-senior disagreement is uncomfortable and consequential. Done well, it's where the best decisions get made. Done badly, it ossifies into political conflict that drags on for years.

**Before the conversation:**

1. **Steelman their position.** Could you argue their side convincingly? If not, you don't understand it yet.
2. **Identify what would change your mind.** "If they could show me X, I'd update."
3. **Identify what's at stake.** Is this a values disagreement, a context disagreement, or a preference?
4. **Pick the right venue.** Slack threads escalate; whiteboards de-escalate.

**During the conversation:**

1. **Lead with shared goal.** "We both want the system to be reliable. Where we differ is..."
2. **Be specific.** Concrete examples beat abstract assertions. "In the case of the order service..." not "in general..."
3. **Separate observation from interpretation.** "The latency is up" is observation; "your design caused it" is interpretation. Confusing them invites defence.
4. **Accept new information.** If they show you something that changes your view, say so. Visibly updating your view *increases* your influence, not decreases it.
5. **Look for the "third option."** Many senior disagreements are framed as A vs B when C is better than either.

**If you can't agree:**

- **Disagree and commit (Amazon's term).** If a decision is made that you don't agree with but isn't dangerous, support it visibly. Re-litigating after the fact poisons trust.
- **Document your dissent.** If it's important, write down your concern and the agreed decision. If it later goes wrong, this is a calibration record, not an "I told you so."
- **Escalate as last resort.** Two senior engineers escalating because they can't agree should feel uncomfortable. Sometimes necessary, never first-line.

**Anti-patterns:**

- **Going around them to their manager.** Burns the relationship permanently.
- **Litigating in public Slack channels.** Audience-driven escalation rarely produces good outcomes.
- **Holding the position because you committed to it.** Sunk-cost fallacy applied to your own opinions.

**Reference:** *Crucial Conversations* (Patterson et al.). Amazon Leadership Principle "Have Backbone; Disagree and Commit" — useful framing whether or not you're at Amazon.

---

## Intermediate

### Q7. How do you handle a stakeholder who keeps changing their mind?

**Answer:**

Volatile stakeholders are exhausting and consume disproportionate engineering time. The first move is to diagnose, not to defend.

**Common causes:**

1. **They didn't know what they wanted in the first place.** Common in early product discovery. The "changes" are actually the discovery process working.
2. **The world keeps changing.** Markets, competitors, regulations, customer feedback. The stakeholder is rationally responding.
3. **They have no clear principal.** They're getting conflicting input from above and translating it inconsistently.
4. **They're avoiding hard trade-offs.** "Yes to everything" leads to "change priorities monthly."
5. **They don't trust the cost of change.** They don't believe you when you say it's a quarter of work, so they keep reopening it.

**Each diagnosis has a different response:**

- **Discovery process:** Build a lightweight prototyping arm. Don't over-invest in builds until the design stabilises. Reframe the work explicitly as "we're discovering, not building."
- **External change:** Build optionality into the work. Modular design that can absorb change cheaply. Document the assumption changes.
- **No clear principal:** Ask to talk with their boss. "Help me understand what we're optimising for at the level above this."
- **Avoiding trade-offs:** Force the trade-off explicitly. "We can do A or B this quarter, not both. Which?"
- **Don't trust the cost:** Show your work. Be visible about engineering trade-offs. Build credibility by being right about cost.

**Tactical moves:**

- **Lock decisions in writing.** "I want to make sure I have this right — the decision is X, agreed by you and Y on date Z." Re-opening a written decision feels different to re-opening a verbal one.
- **Charge for change.** "If we re-open this, we lose two weeks." Make the cost visible in the moment.
- **Set decision deadlines.** "We need a decision by Friday or we'll proceed with the default."

**STAR framing:**

> **S:** A product partner had changed direction four times in eight weeks on a customer-facing dashboard project.
> **T:** As tech lead I needed to stabilise the work without damaging the relationship.
> **A:** I brought it up in the next sync, framed as my problem not theirs: "I'm finding it hard to ship work when we're changing direction this often. Help me understand what's driving the changes." The honest answer was that the VP of Sales was giving conflicting feedback. We agreed to lock scope for two weeks at a time, written down, with explicit re-decision points. I offered to attend the VP syncs to remove the translation layer.
> **R:** Direction stabilised; we shipped within a quarter; the product partner became a strong supporter of mine because I'd helped solve a problem they'd been struggling with privately.

### Q8. How do you bring people along on an unpopular technical decision?

**Answer:**

"Unpopular but correct" decisions are common — deprecating a beloved tool, adopting a stricter quality bar, requiring code review that wasn't required before. Senior engineers who can land these well are rare and valuable.

**The framework:**

1. **Be sure you're right.** Unpopular and wrong is a career-limiting move. Pre-mortem the decision: who will be most opposed, and are their objections valid?

2. **Listen first.** Talk to the people who'll be most affected. Not to convince them — to understand. You'll find legitimate concerns you hadn't considered, and you'll defuse the "no one consulted me" complaint that otherwise dominates.

3. **Build a coalition early.** Get one or two influential allies on board before you go public. Lone advocacy looks like a personal crusade; a coalition looks like a shared view.

4. **Frame around shared values, not your preference.** "We need to deprecate X because the maintenance burden is preventing us from delivering on the reliability commitments we've made" lands better than "I think X is bad."

5. **Communicate with empathy.** People who'll lose familiar tools are losing something real. Acknowledge it: "I know this was the right choice five years ago; the situation has changed."

6. **Provide a migration path.** "Stop using X" without "use Y instead, here's how" is doomed. The work to make the new path easy is the senior engineer's responsibility.

7. **Set a realistic timeline.** Forcing a deprecation in a sprint creates resentment. Quarterly migration with buddy support tends to land.

8. **Take the heat.** When the decision lands and someone is angry, absorb it personally. Don't deflect. The anger has to land somewhere; let it land on you, not on the engineers executing.

**STAR framing:**

> **S:** Our org had three competing logging libraries with overlapping functionality. As staff engineer I'd decided we should consolidate to one — but the choice would force migration on every team.
> **T:** I had to land a decision that none of the affected teams had asked for and that all of them would resist initially.
> **A:** I spent two weeks talking to leads on each team. I built a one-pager with the cost of the status quo (incident with confused logging context, 4 hours of debugging that was attributable to library mismatch). I co-authored the proposal with the most respected senior on the most-affected team. We chose the library that was least disruptive to the largest team. I built a migration tool that handled 80% of the cases automatically. We set a 6-month timeline with monthly check-ins.
> **R:** All teams migrated within 7 months (one slipped). Post-migration retro feedback was 7/10 — not loved, but accepted as the right call.

### Q9. How do you collaborate across teams when there's a history of conflict between them?

**Answer:**

Teams with a history of conflict bring it to every interaction. The new project inherits the baggage. A senior engineer brought in to bridge them needs to deal with the relationship before they deal with the technical work.

**Step 1: Understand the history.**

- Talk to senior people on both sides, separately. Listen for the same incident described differently.
- Look at the team boundaries. Often inter-team conflict is structural — overlapping ownership, unclear interfaces, mismatched incentives. Personality is the symptom, structure is the cause.
- Read the post-mortems and ADRs. The artefacts often reveal the fault lines.

**Step 2: Don't pretend the history doesn't exist.**

- Naming the elephant ("I know we've had friction on past projects — I want this one to be different") is more powerful than tip-toeing.
- Don't take sides. Even if one team was clearly more at fault historically, you're now bridging both.

**Step 3: Build small wins first.**

- The first joint output should be something both teams agree they wanted. A shared post-mortem on a recent incident. A small joint design doc. An interface specification.
- Celebrate the win publicly. "Team A and Team B together delivered X" reinforces the new pattern.

**Step 4: Address structure as well as relationships.**

- If interfaces are unclear, write them down. Most cross-team conflict at the technical level is a missing API contract.
- If ownership overlaps, propose a clearer split. Even imperfect clarity beats fuzzy joint ownership.
- If incentives are misaligned, raise it to leadership. "Team A is on-call for code Team B writes; we should fix that."

**Step 5: Be patient.**

- Trust is rebuilt over months, not meetings. Don't expect the next project to be conflict-free because the first one went well.

**Anti-patterns:**

- **Mediating without authority.** If you have no real leverage and no senior backing, your bridging will fail and you'll be blamed.
- **"Just be professional."** This phrase from the senior engineer to the junior engineer in conflict is unhelpful. Address what's broken; don't perform-fix it.

### Q10. What is "managing up" and what does it look like for an engineer (not a manager)?

**Answer:**

**Managing up** is the practice of actively informing, guiding, and supporting your manager (and senior leaders) so they can make better decisions about your work, your team, and your career. Many engineers think it's not their job; senior engineers know it absolutely is.

**Why it matters:**

- Your manager has limited time and many reports. They will form opinions of your work from the slices they happen to see. Managing up controls which slices.
- Decisions about you (promotion, scope, opportunities) happen in rooms you're not in. Your manager is your representative there. They can only argue what they know.
- Senior leaders make decisions that affect your work. If they don't know what you're doing, they decide as if you weren't doing it.

**Concrete behaviours:**

1. **Brief regularly without being asked.** A weekly written update — what shipped, what's stuck, what's risky — gives your manager material to manage with and to defend you with.

2. **Pre-empt surprises.** Bad news travels up faster than you think. The senior engineer pattern is "you'll hear about X from leadership tomorrow; here's what happened and what we're doing."

3. **Translate technical to business.** Your manager's manager doesn't care about the migration; they care about the customer impact, cost, or risk. Do that translation once, well, in writing.

4. **Make your manager look good in their meetings.** Give them the slide, the metric, the data they need. They reciprocate by giving you better opportunities.

5. **Ask for what you need explicitly.** Promotions, scope, time off, support on a hard project. Managers can't read minds. "I'd like to lead the next platform initiative" is more likely to land than waiting to be picked.

6. **Be reliable.** The single biggest thing. Managers protect, sponsor, and trust the engineers they don't have to chase.

**Anti-patterns:**

- **Manipulating up.** Managing up means making them better-informed, not deceiving them. The latter is detected and ends careers.
- **Treating your manager as your only audience.** Skip-level visibility matters. Your work should be legible without your manager translating it every time.
- **"My manager should already know this."** They might. They might not. The cost of telling them is small; the cost of them not knowing is large.

**Reference:** *The Manager's Path* (Camille Fournier) covers the manager's side. *Staff Engineer* (Will Larson) and *The Staff Engineer's Path* (Tanya Reilly) cover the IC side, including managing up to multiple leaders simultaneously.

### Q11. How do you collaborate effectively with non-engineering stakeholders (PM, design, sales, legal)?

**Answer:**

The senior-engineer mistake is to treat non-engineers as obstacles to "real" engineering work. The senior-engineer behaviour is to recognise them as the partners who make the work matter.

**General principles:**

1. **Learn their language.** PM thinks in roadmaps and customer outcomes. Designers think in user journeys. Sales thinks in deals and quotas. Legal thinks in risk. Speaking their vocabulary lowers the friction of every conversation.

2. **Respect their expertise.** A designer probably knows more about user behaviour than you. A lawyer probably knows more about regulatory risk. Treating their input as something to be argued against rather than learned from limits the work.

3. **Translate clearly.** Don't make them learn your jargon. "We're moving to event-driven architecture" means nothing to them. "Customers will see updates within seconds instead of minutes" means everything.

4. **Be honest about cost.** Padding estimates breeds distrust; under-promising breeds surprise. Calibrated estimates with honest uncertainty bands work best.

5. **Be early on risk.** "This will probably be fine" said three weeks early is more useful than "this just blew up" said the day before launch.

**Stakeholder-specific patterns:**

- **PM:** Co-author the spec. Disagreement on scope is much cheaper before code than after. Bring data, not opinions.
- **Design:** Pair early on technical constraints. Designers can design within constraints they know about; they can't design around constraints you reveal in week 4.
- **Sales:** They will sell things you haven't built. Your job is not to police that but to provide them honest information about what you can credibly commit to. Lying to make sales happy ends in customer-facing failures.
- **Legal/compliance:** Bring them in at design time. The cost of a compliance change after launch is 100x the cost of designing it in. Treat them as designers, not gatekeepers.
- **Support / customer success:** They know your product's failure modes better than you do. The senior engineer who reads support tickets has free design feedback.

**Anti-patterns:**

- **"Engineering decides."** Some decisions are technical-only. Most aren't. Knowing the difference is part of the senior-engineer job.
- **Jargon weaponising.** Using technical language to shut down debate is a junior behaviour wearing a senior costume.
- **Treating Slack as collaboration.** Important cross-functional decisions deserve a meeting and a doc, not a thread.

### Q12. How do you write a status update that senior leaders will actually read?

**Answer:**

Senior leaders read updates in seconds, not minutes. An update written like a paper will be skimmed; an update written for skimming will be read.

**The structure that works (often called BLUF — Bottom Line Up Front):**

```markdown
## Project X — Weekly Update [Date]

**Status:** Green / Yellow / Red — [one-line explanation]

**This week:** [The 1-3 things that matter]

**Risks:** [What could go wrong, what we're doing about it]

**Asks:** [What you need from leadership]

---
[Detail follows for those who want it.]
```

**Why this works:**

- **Status colour scannable in 1 second.** A red flag draws attention; a green check confirms no action needed.
- **One-line explanation prevents misread.** "Yellow — auth migration sliding two weeks" tells the leader what they need to know without reading the rest.
- **Asks are explicit.** Leaders skim for "do I need to do anything?" If asks are buried, they get missed. If there are no asks, say so explicitly.
- **Detail at the bottom for those who want it.** You're not hiding work; you're prioritising it.

**Common mistakes:**

- **Burying the lead.** "We had a productive week with significant cross-team alignment, and following the architecture review, we've decided to delay the migration by two weeks." The "two weeks" is what the leader needs; everything before is filler.
- **Status colour-shifting without explanation.** "Yellow this week" without saying why is worse than not changing colour.
- **Activity over outcome.** Leaders care about outcomes ("the migration is on track for end of quarter") not activity ("the team had three design reviews this week").
- **Hiding bad news.** Leaders find out anyway. Surprise is the worst gift you can give them. Tell them first, framed clearly, with a plan.
- **Inconsistent cadence.** Updates that arrive sometimes get ignored. Set a cadence and keep it.

**Reference:** *On Writing Well* (William Zinsser). The military origins of BLUF are well-documented; the practice has become standard in tech executive communication.

---

## Advanced

### Q13. How do you drive a multi-team initiative when you have no formal authority over any of them?

**Answer:**

This is the staff-engineer job description. The pattern that works is well-documented across the staff-engineering literature.

**Phase 1 — Build the case.**

- Write the problem down. Not the solution — the problem. A one-page "Why this matters" that anyone can read.
- Get data. Cost, risk, customer impact, opportunity. Quantify where you can.
- Identify the leadership owner. There's almost always a senior leader whose org spans the affected teams. Get their backing in private before going public.

**Phase 2 — Build the coalition.**

- Identify the senior engineers on each affected team. Talk to them one-by-one. Listen more than pitch.
- Ask for their input on the proposal — not as a formality but genuinely. Their objections will surface real risks.
- Aim for them to feel like co-authors of the eventual plan. Influence multiplies through co-ownership.

**Phase 3 — Land the plan.**

- Write the RFC. Crisp, short, options-considered. Get it reviewed by the coalition before broadcast.
- Run the broadcast review. The first time most stakeholders see the plan should be in a meeting where the people they trust are nodding along.
- Document the decision. Even if some teams are uncertain, get a decision written down with explicit follow-ups for the open questions.

**Phase 4 — Execute the plan.**

- Make execution easier than the alternative. If the plan requires every team to do hard work, build the migration tooling, the docs, the office hours that make their hard work easier.
- Cadence the work. Monthly checkpoints across teams keep momentum. Without cadence, work slides.
- Celebrate progress publicly. Make it visible that teams are getting it done. Social proof matters.
- When teams are stuck, help. Roll your sleeves up. Your credibility is built on doing, not steering.

**Phase 5 — Hand off and move on.**

- Find the long-term owner. Multi-team initiatives need someone who'll maintain them. If it's not you, it must be someone, and you're responsible for finding them.
- Write up the lessons learned. The next staff engineer running a similar initiative should be able to read your retro and avoid your mistakes.

**Reference:** *The Staff Engineer's Path* (Tanya Reilly), particularly the chapters on "Project Leadership" and "You're a Role Model Now." *Staff Engineer* (Will Larson) for archetypes and case studies.

### Q14. How do you handle a peer who undermines you in cross-team meetings?

**Answer:**

Visible undermining — interruptions, dismissive comments, talking over you, taking credit for your work — is corrosive to a senior engineer's effectiveness. Ignoring it doesn't make it stop; it usually escalates. Confronting it badly makes you look like the problem. The senior move is calibrated and direct.

**Step 1: Verify the pattern.**

- Is this consistent across many situations, or context-specific? Once is forgivable; a pattern requires response.
- Are others noticing? Quietly check with a trusted peer. If they've noticed, the cost of inaction is already accumulating reputationally.
- Are you doing something that invites it? Honest self-check. (E.g., are you over-claiming, or stepping on their domain?)

**Step 2: Address it privately first.**

- Book a 1:1 in private. Never call this out in the meeting where it happened.
- Lead with curiosity, not accusation: "I noticed in the architecture review yesterday you cut me off twice. Help me understand what was going on."
- Listen to the answer. Sometimes there's a legitimate reason — they had context you didn't, they thought you were wrong, they were under pressure. Sometimes it's revealing — they don't think much of your work and this is the leak.
- Be explicit about impact. "When that happens it makes it harder for me to do my job, and it's noticeable to others."
- Land on a behaviour change. "Can we agree to flag disagreements after the meeting rather than in it?"

**Step 3: If it continues, escalate the pattern, not the incidents.**

- Talk to your manager (or theirs, or a shared leader). Don't tattle on individual incidents. Frame as a pattern: "I've raised this with X directly twice; the pattern continues. I want your help."
- Bring evidence. Specific incidents with dates. Vague accusations don't hold up.
- Ask for a specific intervention. "I'd like you to observe the next architecture review" is concrete; "make them stop" isn't actionable.

**Step 4: If it still continues, change the structure.**

- Avoid working with them where you can.
- If you can't avoid them, make sure your work is documented in writing so claims of authorship can be verified.
- Consider whether one of you is in the wrong role. Sometimes peer conflict is a symptom of unclear ownership.

**Anti-patterns:**

- **Public confrontation.** Even if you're right, it makes you look unprofessional.
- **Going to leadership without trying direct conversation.** Burns the relationship and your reputation.
- **Withdrawal.** Refusing to engage means they win the meetings by default.

**STAR framing:**

> **S:** A peer staff engineer in a sister team had a pattern of taking credit for joint work in cross-org reviews — referring to "my team's design" when half had been mine.
> **T:** I had to address it without escalating into a political battle that would harm both our reputations.
> **A:** I booked a 1:1, opened with "I want to talk about something I think we both want to handle well." I described the specific incidents and the impact ("when this happens, my team's contribution becomes invisible to the leaders we need to see it"). His response was honest — he hadn't realised. We agreed to write joint memos with explicit "by X and Y" attribution and to verbally acknowledge each other's teams in meetings.
> **R:** The behaviour stopped. The relationship became one of my strongest cross-team partnerships. The lesson: assume good faith first, but be specific and direct about impact.

### Q15. How do you build relationships with stakeholders before you need them?

**Answer:**

The senior-engineer pattern: invest in relationships when there's no immediate need, so the relationship exists when you need it. Engineers who only call you when they want something get treated accordingly; engineers you've worked with for a year on small things will pick up your call when it matters.

**The investment patterns that compound:**

1. **Show up to their stuff.** Their team's demo, their RFC review, their on-call retro. You'll learn what they care about and they'll notice you cared enough to attend.

2. **Help with small things, no expectation of return.** Review their PR. Answer their question. Connect them to someone who can help. Reciprocity is real, but only if you're not transparently transactional.

3. **Bring them context they wouldn't have.** "Heads up — the platform team is changing X next quarter, here's how it might affect you." This is high-value, low-effort, and signals you're paying attention to their interests.

4. **Public credit for their work.** Mention them in your demos. Cite their RFC. Sponsor them when their name comes up. Goodwill stored for the asking.

5. **Lunch / coffee / virtual coffee.** Sounds basic; it works. The conversations that don't happen on the meeting agenda happen here.

6. **Be useful in incidents.** Cross-team incidents are forge-fire for relationships. Showing up, being calm, contributing real value during their crisis — they remember.

**Why this matters more at senior+:**

- The work senior engineers do is increasingly cross-team, increasingly ambiguous, increasingly dependent on goodwill.
- A staff engineer who hasn't invested in relationships is a staff engineer with a brittle network. The first political shock breaks them.
- Promotion calibration depends on what other senior people say about you in rooms you're not in. Those people only speak well of you if they know you and have positive interactions to draw on.

**Reference:** *Give and Take* (Adam Grant) on the structure of reciprocity. Staff engineering literature consistently emphasises this; Larson's *Staff Engineer* explicitly calls out "your network is your career" at staff+.

### Q16. How do you escalate effectively without burning relationships?

**Answer:**

**Escalation** — bringing a problem to a senior leader for resolution because the people closest to it can't resolve it — is necessary and useful. Done badly, it brands you as someone who can't work with peers and burns the relationships of everyone involved.

**When to escalate:**

- **You and a peer have tried to resolve it directly and failed.** Escalation is the second move, not the first.
- **The decision is above your level of authority.** Some decisions genuinely require leadership input — budget, headcount, cross-org priority calls.
- **The cost of delay exceeds the cost of escalation.** Some disagreements can be resolved slowly; others can't.
- **There is real risk that needs visibility upstream.** If something is going to fail and leadership needs to know, that's escalation as transparency, not escalation as conflict.

**When NOT to escalate:**

- **As a way to win an argument.** "I can't convince you so I'll get someone above you to tell you." Burns the peer relationship for the rest of your career together.
- **Without trying first.** Skipping the direct conversation looks like (and often is) avoidance.
- **For things in your authority to decide.** Escalating a decision you should have owned signals you can't take responsibility.

**How to escalate well:**

1. **Tell the peer first.** "I'm going to raise this with X because we can't agree and a decision is needed. I'll represent your view fairly." Surprise escalation is the hostile kind.

2. **Frame around the decision needed, not the person to blame.** "We need a call on Y. Both teams have considered it and have different recommendations." Not "Team A is being unreasonable about Y."

3. **Bring the full picture.** Both options, both sides' reasoning, what you recommend and why, what you'd lose if the other choice were made. Leaders make better decisions with the full context.

4. **Joint escalation if possible.** Both peers go to the leader together. Reduces the politics, accelerates the resolution, signals maturity.

5. **Accept the outcome.** Whatever the leader decides, support it visibly and execute. If you escalated and you don't like the answer, you escalated.

**STAR framing:**

> **S:** Two teams I bridged disagreed sharply on whether to fork or share a critical library. We'd discussed it twice and reached impasse.
> **T:** As the staff engineer between them I needed to break the stalemate without making either team feel defeated.
> **A:** I told both leads I was going to bring the question to our shared director. I drafted a 1-page memo with both options, the trade-offs, and a recommendation. I showed it to both leads before sending. The director agreed to a 30-minute meeting with all three of us. The decision came in 25 minutes; both teams felt heard.
> **R:** The decision held. Both teams continued to work together effectively. The director thanked me later for "the cleanest escalation I've seen this year." Lesson: framing escalation as a decision-needed (not a conflict-to-resolve) changes the dynamic completely.

### Q17. How do you navigate cross-team work when teams have conflicting incentives or metrics?

**Answer:**

Most cross-team conflict is structural, not personal. When teams are measured on different things, they will optimise for different things, and they will collide. Senior engineers who can see the structural cause are the ones who can fix it.

**Common structural conflicts:**

- **Team A on uptime; Team B on feature velocity.** A blocks B's deploys; B resents A; A resents B's risky changes.
- **Team A on cost; Team B on customer experience.** A pushes for compression and caching; B pushes for richer responses; they fight over latency budgets.
- **Team A on local team velocity; Team B on platform stability.** A wants to fork; B wants standardisation.
- **Sales on quarter revenue; engineering on long-term tech health.** Sales sells things engineering shouldn't commit to; engineering ships things sales can't sell.

**The senior-engineer approach:**

1. **Name the structure.** Make it explicit in conversation. "We're measured on different things. That's the source of this fight, not anything about either of us."

2. **Find the shared upstream metric.** There is almost always a metric one level up that both teams contribute to. Customer NPS. Revenue. Reliability. Reframe the local conflict around the shared metric.

3. **Propose joint metrics.** If two teams have to work together, give them a joint metric for the work they share. Some companies (Amazon, others) explicitly use joint OKRs for this.

4. **Surface the conflict to leadership.** Not as a complaint, but as a request to align incentives. "If you want these teams to collaborate, the current metrics fight you." Leaders often don't see this until it's named.

5. **Bridge with shared rituals.** Joint planning meetings, shared retros, paired representation in escalations. Process can compensate for misaligned incentives in the short term.

6. **Accept what you can't fix.** Some incentive conflicts are real and persistent. The senior engineer's job is then to manage the conflict, not to pretend it doesn't exist.

**Reference:** *Team of Teams* (Stanley McChrystal) on incentives across organisational boundaries. *Accelerate* (Forsgren, Humble, Kim) on the platform-vs-product team dynamic specifically. *Team Topologies* (Skelton & Pais) on the structural moves to reduce cross-team friction by design.
