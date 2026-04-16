# Mentorship — Interview Questions

**Subject:** Technical Leadership
**Topic:** 1:1s, Growth Frameworks, Engineering Ladders, Pair Programming, Onboarding, Sponsorship vs Mentorship
**Difficulty tiers:** Fundamentals / Intermediate / Advanced

---

## Fundamentals

### Q1. What is the difference between mentorship and sponsorship?

**Answer:**

Both are forms of investment in another engineer's career, but they operate in different rooms.

**Mentorship** is *advice given to the mentee*. You sit down with them, talk through their problems, and help them think more clearly. The mentee is in the room.

**Sponsorship** is *advocacy spoken on behalf of someone when they are not in the room*. You put their name forward for the high-profile project. You push for their promotion in the calibration meeting. You quote their work in front of senior leaders. The sponsored engineer is *not* in the room.

**Why the distinction matters:**

- Mentorship costs your time. Sponsorship costs your political capital.
- Mentorship benefits anyone willing to listen. Sponsorship requires you to have capital to spend.
- Mentorship is mostly safe. Sponsorship has downside — if your sponsoree underperforms, your judgement is questioned.

**The data point:** Lara Hogan (*Resilient Management*) and others have written extensively on this distinction. Studies consistently show under-represented engineers receive abundant mentorship and a deficit of sponsorship — they get advice, not advocacy.

**Interview insight:** Strong senior candidates name this distinction unprompted and describe specific sponsorship behaviours: "In the last calibration cycle I argued for X's promotion based on the migration she led; she wasn't there, but the work was visible because I made sure of it."

### Q2. What is a 1:1 and what is its purpose?

**Answer:**

A **1:1** is a recurring, private conversation between two people — typically manager-and-report, but also mentor-and-mentee, peer-and-peer, or staff-engineer-and-junior. The format is structured but the agenda belongs to the more junior person.

**The 1:1 is not:**

- A status update. (Status belongs in async written form.)
- A code review. (Reviews belong on the PR.)
- A performance review. (Performance reviews are scheduled separately and have a different shape.)

**What 1:1s are for:**

1. **Career conversation.** Where do you want to be in two years? What's blocking you?
2. **Feedback in both directions.** What's working? What's not? Manager and report both speak.
3. **Surfacing blockers.** Things too small to escalate, too important to ignore.
4. **Trust-building.** Routine, low-stakes conversation makes the high-stakes ones possible.
5. **Calibration.** Are we aligned on what good looks like for this role?

**Cadence:** Weekly is standard. Bi-weekly works for senior reports who don't need it. Monthly is too rare — by the time you talk, the issue has festered.

**Reference:** *The Manager's Path* (Camille Fournier) and *High Output Management* (Andy Grove) are the canonical references. Grove's framing — "the 1:1 is for the subordinate" — is useful in interviews.

### Q3. How would you structure a 1:1 with a direct report or mentee?

**Answer:**

A 1:1 without structure becomes a status meeting. With too much structure, it loses its purpose. The light scaffold most experienced managers use looks like this:

```markdown
## 1:1 Agenda — [Date]

### Their items (priority)
- ...
- ...

### My items
- ...

### Career / growth (every 2-4 weeks)
- ...

### Feedback in both directions (every meeting)
- ...
```

**Defaults:**

- **Mentee leads.** They bring topics first. If they bring nothing, that's a signal worth probing.
- **Async notes.** Both parties add to a shared doc beforehand. Avoids surprises.
- **End on action items.** Each side leaves with at most 1–2 concrete things to do.
- **Skip when needed.** A 1:1 with no agenda is worse than a cancelled 1:1.

**Common questions to drop into the rotation:**

- "What's energising you right now? What's draining?"
- "Where are you stuck that I could help unstick?"
- "Is there feedback you've been holding back?"
- "What would make next week better than this week?"

**Anti-pattern:** The 1:1 that becomes a stand-up. If you're working through Jira tickets, you've lost the meeting.

### Q4. What is an engineering ladder and what does a senior engineer use it for?

**Answer:**

An **engineering ladder** is a written rubric describing the expectations at each level — typically Junior, Mid, Senior, Staff, Principal, Distinguished. It captures the scope of impact, the technical depth, the leadership behaviours, and the autonomy expected at each rung.

**Why it exists:**

- **Calibration.** Without a written ladder, "senior" means whatever a manager says it means today.
- **Promotion.** Engineers need a target to aim at. Managers need evidence to argue for promotion.
- **Hiring.** Levelling new hires consistently is impossible without a shared rubric.
- **Compensation.** Pay bands attach to levels. The ladder anchors the bands.

**What a senior engineer does with it:**

1. **Self-assess.** Honestly compare yourself to the next level's expectations. The gap is your growth plan.
2. **Mentor against it.** Use the ladder as the framework when discussing growth with juniors.
3. **Sponsor against it.** Make the case for promotion using the language of the ladder.
4. **Improve it.** Ladders that don't get updated drift away from reality. Senior engineers contribute to keeping them honest.

**Public examples:** progression.fyi aggregates dozens. Rent the Runway's, CircleCI's, Patreon's, and Riot Games' are often cited as good models. Gergely Orosz's writing covers the topic in depth.

**Interview phrasing:** "When I mentor someone toward staff, I'm not coaching them on coding — I'm coaching them on the scope-of-influence and ambiguity-handling expectations the ladder describes."

### Q5. What does effective onboarding look like in the first 30/60/90 days?

**Answer:**

A senior engineer is often expected to design or run onboarding for new hires. The standard 30/60/90 framing maps to expanding scope.

**Days 1–30: Setup and context.**

- Environment works on day 1 (laptop, accounts, repos, deploy access).
- Read the team's primary documents (architecture overview, ADRs, runbooks).
- Pair with two or three teammates on real work.
- Ship one trivial PR by end of week 1 to validate the deploy pipeline (and dignity).
- Meet stakeholders the team works with.

**Days 31–60: Contribution.**

- Own a real, scoped feature or bug.
- Begin reviewing teammates' PRs.
- Attend incident reviews to absorb production reality.
- Identify one onboarding gap and fix it (a doc, a script, a runbook).

**Days 61–90: Independence.**

- Lead a small project end-to-end.
- Be the on-call (with a buddy if production is risky).
- Give feedback in retros confidently.
- Begin mentoring the next new hire.

**Senior leadership behaviours during onboarding:**

- **Assign a buddy.** Not the manager. Someone who pairs technically and answers stupid questions.
- **Write down the implicit.** "We always X" rules that aren't documented are onboarding tax.
- **Reduce hero dependence.** If only one person can answer a question, you have a bus factor problem the new hire just exposed.
- **Measure.** "Time to first PR," "time to first on-call," "30-day retention" — these are leading indicators of onboarding health.

**Reference:** *The First 90 Days* (Michael Watkins) is the leadership-track reference. Will Larson's *An Elegant Puzzle* covers engineering-specific onboarding.

### Q6. When should you pair-program with a mentee instead of letting them work independently?

**Answer:**

Pairing has high cost (two engineers, full-time) and high value (live transfer of skill and context). It's not the default — it's a tool with specific use cases.

**Pair when:**

- **The mentee is stuck and unblocking is taking too long.** A 90-minute pair beats three days of frustration.
- **The work crosses an unfamiliar boundary.** New service, new language, new domain. Pair through the first ramp.
- **The risk is high.** Production data migration, security-sensitive code, complex concurrency. Two heads catch what one misses.
- **You're transferring tacit knowledge.** "How do you debug this?" is hard to write down — pair and let them watch.
- **The mentee has lost confidence.** Sometimes pairing is psychological, not technical.

**Don't pair when:**

- The work is well-scoped and well-understood by the mentee — they'll learn more by struggling.
- You're doing the work and dictating; that's not pairing, it's an audience.
- The mentee needs deep-thinking time. Pairing is intense and exhausting.

**The senior anti-pattern:** Pairing where you take the keyboard and they watch. They learn nothing. The mentee should drive at least half the time, and you should resist correcting in real time — let them try, fail, and reflect.

**Reference:** *Extreme Programming Explained* (Kent Beck) is canonical on pairing. *Pair Programming Illuminated* (Williams & Kessler) covers the research base.

---

## Intermediate

### Q7. How do you set growth goals with a mentee?

**Answer:**

Generic goals ("be more senior") are useless. Effective growth goals are specific, observable, and tied to evidence the mentee can produce.

**The structure I use:**

1. **Identify the gap.** Use the engineering ladder, recent feedback, and the mentee's own reflection.
2. **Pick 1–2 areas, not 5.** Concentration beats breadth for growth.
3. **Define "what good looks like."** What does the next level do that they don't do today?
4. **Choose evidence.** What artefact will demonstrate progress? An ADR, a talk, leading an incident, mentoring a hire.
5. **Set a horizon.** A quarter is the right unit. Annual is too vague; weekly is too narrow.
6. **Plan check-ins.** Monthly review of progress, not just the goal at the end.

**Example:**

> **Gap:** "Influence beyond the team" — currently strong within team, no cross-team artefacts.
>
> **Goal (Q3):** Lead the design review for the cross-team API consolidation project.
>
> **Evidence:** RFC document authored by mentee; review meeting facilitated by mentee; ADR captured at conclusion.
>
> **Support:** I will co-author the first draft, attend the review as observer, and debrief afterwards.
>
> **Check-ins:** Monthly 1:1 with progress review.

**Anti-patterns to avoid:**

- **Vague goals:** "Communicate better." Better at what? Measured how?
- **Stretch goals with no support.** Setting someone up to fail is not mentorship.
- **Goals tied to outcomes outside their control.** "Get the project shipped" depends on twenty people. "Lead the design review" is something they can do.

**STAR framing for interviews:**

> **S:** A senior engineer on my team had been at-level for two years and was frustrated by stalled growth.
> **T:** As her mentor I needed to help her break the plateau without setting unrealistic expectations.
> **A:** We mapped her work against the staff-level rubric and found her gap was scope-of-influence — she was excellent within the team, invisible outside it. We set a quarter-long goal: lead one cross-team initiative with a written artefact. I sponsored her for the data-pipeline consolidation project and committed to a monthly review.
> **R:** She delivered the RFC, ran the review, and the artefact was cited in her promotion case six months later. She was promoted to staff.

### Q8. How do you mentor someone whose technical skills exceed yours?

**Answer:**

This becomes common at staff and principal level — you'll mentor specialists in domains you don't know deeply (ML, embedded, GPU programming, kernel work). Trying to fake technical depth is the wrong move.

**What you can offer regardless of technical asymmetry:**

- **Career navigation.** How does promotion work here? How do you make work visible? What do leaders look for?
- **Stakeholder management.** How do you negotiate scope with the PM? How do you handle a frustrated customer call?
- **Writing and communication.** How do you turn this technical achievement into a one-pager an exec will read?
- **Organisational context.** Who decides what? Where do similar projects get stuck? What's the political landscape?
- **Sponsorship.** You may not understand their CUDA kernel, but you can argue for their promotion based on the impact you saw.

**What to do explicitly:**

1. **Acknowledge it openly.** "I won't be useful on the kernel work itself — but I can be useful on these other things."
2. **Learn enough to ask good questions.** You don't need depth, but you need enough vocabulary to be a useful conversation partner.
3. **Find them a technical mentor too.** Mentorship doesn't have to be one-to-one. A specialist needs a domain mentor and a career mentor; you can be the second.

**Anti-pattern:** The senior who insists on adding technical opinions to areas they don't understand. The mentee loses respect, and the relationship deteriorates.

**Interview phrasing:** "I lead a team that includes ML researchers whose technical depth exceeds mine. My mentorship value is on the parts of their job they don't get from their PhD — making impact visible, navigating the org, and pulling political weight in calibration."

### Q9. How do you give feedback that an engineer doesn't want to hear?

**Answer:**

Hard feedback is the test of mentorship. Avoiding it is the most common failure.

**The framing I use, attributed to Kim Scott (*Radical Candor*):**

- **Care personally** — they need to know you're on their side.
- **Challenge directly** — say the hard thing, plainly.

Skipping the first creates "obnoxious aggression." Skipping the second creates "ruinous empathy" — which is more common and more damaging.

**A structure that works:**

1. **Lead with intent.** "I want to give you some feedback because I think it'll help you get to staff."
2. **Be specific.** Names, dates, observed behaviours. "In the design review last Thursday..." not "people feel..."
3. **Describe impact.** What happened as a result. Not what you assume they meant.
4. **Ask, then listen.** "Does that match how you saw it?" Genuinely consider their perspective.
5. **Land on action.** "What would you try differently next time?" — let them own the change.
6. **Follow up.** Reference it in the next 1:1. Feedback delivered once and forgotten doesn't count.

**Don't:**

- Use the "feedback sandwich" (compliment-criticism-compliment). It dilutes the message and feels manipulative.
- Deliver in front of others.
- Wait for the end-of-quarter review. Feedback decays in storage.
- Phrase as "people are saying..." Own the feedback yourself.

**STAR framing:**

> **S:** A senior IC on my team was technically strong but his code reviews were demoralising junior engineers — sarcastic, dismissive, no praise.
> **T:** Two juniors mentioned it in skip-levels with my manager. I was the closest peer and was asked to raise it.
> **A:** I booked a 1:1, opened with "I want to talk about something I think is hurting your reputation more than you realise." I shared two specific review comments verbatim, described what I'd heard about their impact, and asked for his perspective. He'd not realised the tone landed that way. We agreed he'd review three of my comments per week for tone calibration for a month.
> **R:** Within six weeks the team's anonymous feedback shifted noticeably. He told me a year later it was the most useful piece of feedback he'd had in his career.

### Q10. What is "scope of influence" and why does it matter for senior engineers?

**Answer:**

**Scope of influence** is the size of the engineering organisation a person's work affects. It is the dominant axis on which staff and principal engineers are evaluated. Coding ability flattens out at senior; influence does not.

**The conventional ladder of scope:**

| Level | Typical scope |
|-------|---------------|
| Junior | Tasks within a project |
| Mid | Features within a team |
| Senior | A team's roadmap and quality |
| Staff | Multiple teams; technical strategy of a domain |
| Principal | An organisation; cross-domain technical direction |
| Distinguished | The company; industry-level impact |

**How influence is grown (the things mentees should be doing):**

- **Writing.** ADRs, RFCs, post-mortems, internal blog posts — these scale your thinking beyond any meeting you can attend.
- **Public artefacts.** Library released, talk given, runbook authored that other teams use.
- **Cross-team work.** Initiatives where you have no authority, only influence.
- **Mentoring across team boundaries.** Your influence is multiplied through the people you grow.
- **Sponsorship.** Other people whose careers you have visibly accelerated.

**What blocks growth in influence:**

- **Hoarding work.** If you're the only one who can do X, you can't move on from X.
- **Not writing things down.** Influence by Slack message dies in scrollback.
- **Avoiding ambiguity.** Influence lives in the unowned space between teams.

**Reference:** *The Staff Engineer's Path* (Tanya Reilly) is the definitive reference. *Staff Engineer* (Will Larson) is the companion volume on the role itself.

### Q11. How do you handle mentoring someone who isn't growing?

**Answer:**

Plateau is normal. Sustained non-growth despite support is a different problem and requires a different response.

**Diagnose first.**

- **Is it skill?** They don't know how to do the next-level work yet.
- **Is it confidence?** They have the skill but don't apply it.
- **Is it motivation?** They don't want the next level (legitimate — not everyone does).
- **Is it environment?** The work doesn't give them the opportunities they'd need to grow.
- **Is it personal?** Burnout, life situation, mental health. None of which are mentorable through.

**Each diagnosis has a different response:**

- **Skill gap:** Concrete training plan. Pair on the missing skill. Stretch assignments.
- **Confidence gap:** Sponsor them into visible work. Catch and counter their self-deprecation.
- **Motivation:** Have an honest conversation about what they actually want. Not everyone wants staff. Some want to be excellent senior engineers for twenty years — that's a valid career.
- **Environment:** Work with their manager to change the work, or help them find a different team.
- **Personal:** Step back from growth conversations. Refer to manager, EAP, or whatever support is appropriate.

**The senior failure mode:** Pushing growth on someone who hasn't asked for it. Mentorship is invited, not imposed. Some engineers want a career coach; others want a peer who reviews their PRs well and otherwise leaves them alone. Respect the difference.

**Honest question to ask:** "Do you want me to keep coaching you on growth, or would you prefer I just be a sounding board for current work?"

### Q12. What does "leveraging your time" look like for a senior engineer who is also expected to mentor?

**Answer:**

Mentorship is an investment, not a cost — but unmanaged it consumes the entire week. Senior engineers need an explicit model for where their time goes.

**The leverage hierarchy (roughly, highest leverage first):**

1. **Writing things down.** A doc you write once is read by hundreds; a Slack DM is read by one.
2. **Sponsoring others into the work.** Their growth multiplies the team.
3. **Reviewing high-leverage work.** A review on an ADR shapes a year of work; a review on a typo doesn't.
4. **Pairing on critical-path work.** Bottlenecks broken; skills transferred.
5. **1:1 mentorship.** Direct skill transfer to one person.
6. **Doing the work yourself.** Lowest leverage — you don't scale.

**Allocation heuristics:**

- **No more than 4–6 mentees concurrently.** More than that and the quality drops to social meetings.
- **Group what you can.** A weekly "office hours" channel beats 6 individual repeats of the same conversation.
- **Write the answer once.** If you've explained something three times, write it down. The fourth time, link the doc.
- **Cap synchronous time.** 25–35% of the week on meetings is sustainable; above that, your output collapses.

**Reference:** *High Output Management* (Andy Grove) on leverage. *The Staff Engineer's Path* (Tanya Reilly) on the staff-engineer time-allocation problem specifically.

---

## Advanced

### Q13. How do you build a mentorship programme for an engineering organisation?

**Answer:**

Ad-hoc mentorship privileges those who are already well-connected. A formal programme distributes opportunity. Doing it well is harder than it looks.

**Structural decisions:**

1. **Voluntary, not assigned.** Mentees opt in; mentors opt in. Forced pairings produce bad relationships.

2. **Matching.** Some programmes match by interest; others by ladder gap; others let mentees pick from a list. The biggest risk is matching only by demographic — well-meaning but often produces shallow relationships.

3. **Cadence.** Suggest fortnightly 1:1s for 1–2 quarters. Open-ended programmes drift; capped programmes have a clean off-ramp.

4. **Curriculum or open-ended.** A loose framework helps new mentors. A structured programme (e.g., "next-level expectations week 1, writing week 2") risks becoming a course rather than a relationship.

5. **Train mentors.** Most engineers have never been taught how to mentor. A 90-minute workshop on listening, feedback, and goal-setting raises the floor enormously.

6. **Measure carefully.** NPS-style surveys at programme end. Promotion rate of participants vs. control. Retention rate. Don't measure attendance — that's a vanity metric.

**Pitfalls to avoid:**

- **Mentor-as-status.** If becoming a mentor is "selected," it becomes political. Open it broadly.
- **Same mentors every cycle.** Burnout. Rotate.
- **No exit ramp.** Some pairings don't work. The programme should make it easy to dissolve and re-match without drama.

**Reference:** Lara Hogan's writing on mentorship at scale. *Resilient Management* contains usable templates.

### Q14. How do you mentor someone whose career goals diverge from your team's needs?

**Answer:**

This is a values test for senior engineers. The selfish move is to keep them where they are. The right move is to help them get to where they want to go, even if it costs your team.

**Why it matters:**

- Engineers who feel held back leave. You don't keep them by blocking — you accelerate their departure.
- Engineers who feel sponsored toward their goals stay engaged longer, and often return.
- Your reputation as a mentor depends on this. If you only help people in ways that benefit you, no high-performer will work for you again.

**What it looks like in practice:**

1. **Ask explicitly.** "Where do you want to be in two years?" If the answer surprises you, you haven't been listening.
2. **Map the gap.** What experience do they need? Who do they need to know? What artefacts would prove it?
3. **Find the work that fits.** Sometimes inside your team. Sometimes a stretch project elsewhere. Sometimes a different team entirely.
4. **Sponsor across boundaries.** Introduce them to leaders on the team they want to join. Argue their case to your peers.
5. **Plan the transition.** If they need to move, plan it. The team that loses a strong engineer with notice handles it better than the team blindsided by a resignation.

**STAR framing:**

> **S:** A senior IC on my team was an excellent backend engineer but kept telling me he wanted to move into ML infrastructure.
> **T:** I had no ML work on my roadmap and a real need for his current contributions. The selfish play was to keep him.
> **A:** I introduced him to the staff engineer leading the ML platform team. I sponsored a 20% time arrangement so he could contribute to their work for two quarters. When that team's headcount opened, I supported the transfer rather than counter-offering.
> **R:** He moved, thrived, and was promoted within the year. Two years later he came back to my org as a tech lead — a stronger engineer than the one who left, and bringing ML expertise we now needed. The cost of letting him go was less than the cost of holding him.

### Q15. What is reverse mentorship and when is it valuable?

**Answer:**

**Reverse mentorship** is a structured relationship where a more junior engineer mentors a more senior one — typically on a domain where the junior has more current expertise (new technology, recent academic work, organisational subculture, generational perspective).

**When it's valuable:**

- **Technology refresh.** A principal engineer who hasn't touched the modern frontend stack in five years can be reverse-mentored by a mid-level engineer who lives in it.
- **Cultural perspective.** Senior leaders often lose touch with how the org feels three rungs below. A junior reverse-mentor offers a window.
- **DEI awareness.** Engineers from under-represented backgrounds reverse-mentoring leaders has been shown (e.g., PwC's programme) to shift hiring and promotion behaviour.
- **Tooling and practices.** New approaches to testing, observability, AI-assisted dev. Juniors often adopt these faster than seniors.

**What makes it work:**

- **Explicit framing.** "You are mentoring me on X. I won't pull rank in this conversation." If the senior treats it as a casual chat with a junior, it isn't reverse mentorship.
- **Defined topic.** Reverse mentorship works best with a narrow scope.
- **Reciprocity in private.** The senior may still mentor the junior in other contexts — but separately, not in the same session.

**Anti-pattern:** The senior who uses reverse mentorship as cover for not learning. Reading a book, attending a conference, or doing the work themselves are still required.

### Q16. How do you decide whether to give an engineer a stretch project they might fail at?

**Answer:**

Stretch projects are how engineers grow. They are also how engineers burn out and leave. The senior's job is to calibrate.

**Conditions for a stretch project that grows the engineer:**

1. **The gap is bridgeable.** Not "I'm a junior and I'm leading the data centre migration." More like "I'm a senior and I'm leading my first cross-team initiative with three teams."
2. **Visible support exists.** A mentor, a sponsor, a manager who will catch them if they fall. Stretch without support is hazing.
3. **The cost of failure is recoverable.** Production failure with rollback is fine. Public-facing reputational failure for a junior may not be.
4. **The engineer wants it.** Forced stretch is resented. Asked-for stretch is invested in.
5. **Time pressure is realistic.** A stretch project on a hard deadline often fails on the deadline, not the stretch.

**What the senior provides:**

- **Permission to ask for help.** "Tell me when you're stuck. I will not be disappointed; I will help."
- **Visibility management.** "I'll attend the steering meeting with you at first; we'll taper as you settle in."
- **Pre-mortem.** "Where might this fall over? What do we do if it does?"
- **Cover.** If it goes wrong, the senior takes the heat publicly. The engineer takes the lessons privately.

**The Wardley/Reilly framing:** Stretch is a form of options-buying. You're paying a premium (risk of failure) for the option of much-faster growth than the conservative path would give you.

**Anti-pattern:** Sink-or-swim assignments dressed up as stretch. If you've thought "well, they'll figure it out or they won't," you're not mentoring — you're outsourcing your judgement.

### Q17. How does mentorship change when you're no longer in the same office or time zone as your mentee?

**Answer:**

Co-located mentorship has free serendipity — overheard conversations, water-cooler context, body language in meetings. Remote mentorship loses all of that and must compensate explicitly.

**What changes:**

1. **Casual context disappears.** You won't overhear that they're stressed. They have to tell you, which they often won't.
2. **Tone flattens in writing.** Slack DMs lack the smile and the pause. Misreadings multiply.
3. **Sponsorship is harder.** You can't pull a leader aside in the corridor. You have to engineer the moment.
4. **Time-zone constraints squeeze 1:1s.** A weekly 30-minute slot at the only overlapping hour is fragile.

**What to do differently:**

- **Video on, by default.** Audio-only loses too much. Both of you should default to camera.
- **Longer 1:1s, less often.** A 60-minute fortnightly beats a 25-minute weekly with travel-time overhead.
- **Async between syncs.** Shared doc with running notes; both add to it during the week.
- **Watch for silence.** A mentee who stops bringing items isn't fine — they're disengaging. Probe gently.
- **Visit if possible.** One in-person trip per year is worth twenty Zoom calls. Budget for it.
- **Sponsor publicly.** Mention their work in async-visible places (Slack channels, all-hands docs, internal blogs) so leaders see it without you brokering the introduction in person.

**STAR framing:**

> **S:** I took on a mentee in another time zone, eight hours apart, who became increasingly quiet over three months.
> **T:** As her remote mentor I had to notice without the in-office signals I'd usually rely on.
> **A:** I noticed the 1:1 doc agendas had become empty for three weeks and the items she'd brought before were small. I added a single direct question to the next session: "Are you OK?" She told me she'd been struggling — burnout symptoms, no one in-office to vent to, manager unaware. We restructured the work, brought her manager into the conversation with her permission, and she took two weeks off.
> **R:** She came back, stayed in the role for another two years, and was promoted. Lesson: in remote mentorship the absence of signal is itself the signal. Look for what's missing, not just what's present.

### Q18. How do you mentor toward the staff-engineer transition specifically?

**Answer:**

The senior-to-staff transition is the hardest career step in software engineering. The work, the rewards, and the failure modes all change. Mentoring through it requires a specific framework.

**What changes at staff:**

- **Scope expands beyond a team.** You influence multiple teams, often with no authority over them.
- **Coding time decreases.** Often dramatically. Many staff engineers code less than 20% of the week.
- **Writing dominates.** ADRs, RFCs, technical strategy docs, executive updates.
- **Ambiguity is the work.** Senior-and-below get well-defined problems. Staff get "figure out what we should be doing."
- **Relationships are the medium.** Influence requires trust with peers, leaders, and stakeholders — built over years.

**Common stuck points:**

| Stuck point | Coaching response |
|-------------|-------------------|
| "I deliver more than anyone but don't get promoted." | Delivery isn't enough at staff. Show me the cross-team artefact. |
| "I miss coding." | This is the biggest tax. Plan how to keep one or two coding outlets without dropping the leverage work. |
| "No one listens to my proposals." | Are they written down? Are they short? Have you built relationships with the people whose buy-in you need? |
| "My manager doesn't get it." | Find a staff-level peer or sponsor outside your reporting line. The staff-promotion case is rarely made by your direct manager alone. |
| "I'm becoming a meeting machine." | Audit your calendar. Cut 20% in the next two weeks. |

**Tactical mentorship moves:**

- **Co-author one RFC.** They drive; you edit. Builds the muscle.
- **Bring them to one steering meeting per month.** Visibility and pattern absorption.
- **Sponsor them into one cross-team initiative per quarter.** Influence is built one project at a time.
- **Talk explicitly about the time-allocation shift.** Most senior engineers are surprised by how much it changes.

**Reference:** *The Staff Engineer's Path* (Tanya Reilly) is structured exactly around this transition. Will Larson's *Staff Engineer* gives the complementary perspective. Patterns like "tech lead manager," "architect," "right hand" are usefully named in both.
