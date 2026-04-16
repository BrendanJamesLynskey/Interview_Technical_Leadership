# Writing and Communication — Interview Questions

**Subject:** Technical Leadership
**Topic:** Design Docs, RFCs, Technical Writing, Public Speaking, Async Communication, BLUF
**Difficulty tiers:** Fundamentals / Intermediate / Advanced

---

## Fundamentals

### Q1. Why is writing considered a core senior-engineer skill?

**Answer:**

Writing is the primary leverage tool for engineers beyond the level where they can be in every meeting. It scales your thinking past your available hours.

**The leverage argument:**

- **Meetings are expensive and don't scale.** Your best insight, delivered in a meeting, reaches at most a dozen people once. The same insight in a well-written doc is read by hundreds, over years.
- **Writing forces clarity.** You can be vague in speech and still be understood; in writing, the reader's confusion is your fault. The act of writing exposes weak thinking.
- **Writing is asynchronous.** Readers consume on their schedule, in their time zone. You don't need to be present.
- **Writing creates durable artefacts.** A decision recorded in a doc survives the team reorg. A decision delivered in a Zoom call does not.

**The career argument:**

- Staff+ promotion cases are built on written artefacts — RFCs, ADRs, strategy docs, post-mortems, internal blog posts.
- Writing a widely-read doc is one of the most visible forms of influence without authority.
- Leaders skim-read; engineers who write well for skim-readers get their ideas across.
- Writing is portable. Your code mostly stays with your employer; your ability to write about systems goes with you.

**The caveat:**

Not all writing is equal. Long-winded, badly-organised, jargon-heavy writing is worse than no writing. The senior skill is writing *effectively* — short, structured, reader-optimised.

**Reference:** *On Writing Well* (Zinsser) is the canonical general-purpose text. *The Elements of Style* (Strunk & White) for basics. For engineering-specific: Amazon's 6-pager culture and the "narrative over slides" tradition is well-documented. *Accelerate* (Forsgren, Humble, Kim) positions written culture as a DORA-correlated marker of high performance.

### Q2. What is BLUF and why does it matter?

**Answer:**

**BLUF — Bottom Line Up Front.** State the conclusion, decision, or ask in the first sentence. Put the supporting detail after.

**Origin and purpose:**

The term comes from US military staff communications. The reader — typically a senior officer with limited time — can decide in one sentence whether they need to act, delegate, or move on. The supporting detail is there for when they or their staff need it, but not required for the decision.

**In software engineering:**

- Executives read your update in seconds, not minutes. BLUF gets your message through.
- Slack messages and emails are scanned, not read. BLUF survives scanning.
- In a 50-page design doc, the reader reads page 1. BLUF makes page 1 enough for casual readers.

**What BLUF looks like in practice:**

```markdown
# Good — BLUF

**Recommendation: Adopt Library X over Library Y for our auth stack.**

X better supports our multi-region requirements and has faster CVE response times.
Y is cheaper operationally but its single-region design would require
a significant re-architecture to match our reliability targets.

[further detail follows...]
```

```markdown
# Bad — conclusion buried

Over the last six weeks we've been evaluating various auth libraries.
Library X has these features, Library Y has those features, and we
considered Library Z but ruled it out for the following reasons...

[eight paragraphs later]

...and so we recommend Library X.
```

**Where BLUF is less natural:**

- Narrative writing (a blog post, an onboarding guide) sometimes benefits from a setup before the conclusion.
- Sensitive communication (disagreement, difficult feedback) may need context first to avoid reading as abrupt.
- Stories (a retrospective of a project) sometimes flow better chronologically.

But for most engineering writing — decisions, recommendations, status updates, exec communication — BLUF wins.

**Reference:** The US Army's ADP 6-22 (Army Leadership) describes BLUF explicitly. Amazon's internal narrative standards promote it. Josh Bernoff's *Writing Without Bullshit* is a useful popular reference.

### Q3. What is a design doc and what should it contain?

**Answer:**

A **design doc** is a written document describing a proposed technical solution to a specific problem, reviewed before significant implementation work begins. Its purpose is to surface design issues, align stakeholders, and create a durable record of the decision.

**Standard structure:**

```markdown
# [Project / Feature Name] Design Doc

**Author:** [You]
**Reviewers:** [Named]
**Status:** Draft / Review / Approved / Implemented

## Background
What's the current state? What problem are we solving? Why now?

## Goals
What must this design achieve? Quantified where possible.

## Non-Goals
What is deliberately out of scope?

## Proposed Design
The solution, at the level of detail required to review.
Diagrams for anything non-trivial.

## Alternatives Considered
Other approaches and why they were rejected.

## Risks / Unknowns
What could go wrong? What don't we know?

## Rollout / Migration Plan
How do we get from current state to the new state safely?

## Open Questions
Things for reviewers to weigh in on.
```

**What makes a good design doc:**

- **Right-sized.** Usually 3–10 pages. Longer than that, split it.
- **Writes the important sections first.** Many doc writers spend pages on background before getting to the design. Reverse it.
- **Explicit about alternatives.** "We chose X" is suspicious without "here's what we considered and why we didn't choose it."
- **Honest about risk.** A design doc with no listed risks is either incomplete or dishonest.
- **Written for the reviewer, not the author.** The goal is to get useful feedback. If the doc makes feedback hard, the author is optimising for the wrong thing.

**Common failure modes:**

- **No alternatives.** The proposed solution is presented as the only option. Reviewers can't evaluate trade-offs.
- **Missing non-goals.** Scope grows during review. Writing non-goals explicitly caps it.
- **Vague rollout plan.** "We'll deploy it" is not a plan. Migration is usually the hard part.
- **Doc never goes to "approved."** Docs that linger in draft are confusion magnets; future engineers don't know if this is truth or proposal.

**Reference:** Google's published design doc culture (Jeff Ellis on Google's engineering blog). Will Larson's writing on technical strategy docs. Gergely Orosz's writing on design doc practices in large tech companies.

### Q4. What is the difference between a design doc and an RFC?

**Answer:**

Both are pre-decision technical writing. The distinctions vary by company, but the useful ones:

| Aspect | Design Doc | RFC |
|--------|-----------|-----|
| Scope | A specific solution to a specific problem | Often broader — a proposal for a change in practice, standard, or strategy |
| Audience | The team and immediate stakeholders | The engineering org or a community of interest |
| Invitation | "Review my design" | "Comment on this proposal" |
| Status | Often implicit (on a wiki page) | Explicit lifecycle (draft, review, accepted, rejected) |
| Durability | Tied to the project; may be superseded | Long-lived; numbered; referenced |
| Examples | A new auth service's design | "We should move to trunk-based development" |

**Some orgs unify them.** Everything becomes an "RFC" or everything becomes a "design doc." The distinction matters less than consistency within the org.

**Public RFC traditions to learn from:**

- **IETF RFCs** — the original, for internet standards.
- **Python PEPs** — Python Enhancement Proposals.
- **Rust RFCs** — a public process with a well-documented structure.
- **Kubernetes KEPs** — Kubernetes Enhancement Proposals.

**In interview answers:** Showing awareness that you've read real-world RFCs (e.g., "I appreciate the Rust RFC structure's explicit 'unresolved questions' section") signals maturity.

### Q5. How do you structure a technical one-pager for a senior audience?

**Answer:**

The one-pager is the exec communication workhorse. Fit on a single page (or screen). Structured for skim-reading. Drives a decision or informs an update.

**Standard structure:**

```markdown
# [Title — active, decision-oriented]

**TL;DR:** [Single sentence: the bottom line. Often includes the recommendation or key finding.]

## What we want [or "Situation"]
[2-3 bullet points or sentences — what's the current state and what decision/action is needed]

## Why now
[Urgency, cost of delay, opportunity]

## Options [optional, only if decision needed]
- Option A: [1-line summary + pro/con]
- Option B: [1-line summary + pro/con]
- Option C: [1-line summary + pro/con]

## Recommendation
[What you're asking for, why]

## Risks / Asks
[What could go wrong, what you need from the reader]
```

**Design principles:**

1. **One page means one page.** Not "two pages if needed." The discipline of fitting on one page forces ruthless editing.

2. **Skimming must work.** Headlines, bullets, bolded key phrases. A reader spending 30 seconds should get the gist.

3. **Numbers matter.** Quantify cost, benefit, risk where possible. "Faster" is weak; "30% latency reduction" is strong.

4. **No jargon for the audience's level.** If writing for a VP, translate technical terms. If writing for engineers, don't over-simplify.

5. **End with an ask.** What do you want the reader to do? Approve? Discuss? Fund? Choose? Be explicit.

**Anti-patterns:**

- **Build-up before the point.** Executives skim; they won't reach the point at the bottom.
- **Multiple asks in one page.** Focus. One decision per one-pager.
- **Data without interpretation.** A chart without a "what it means" sentence is work the reader has to do.
- **Optional padding.** Every sentence should be load-bearing.

**Reference:** Amazon's 1-pager (and 6-pager) culture is well-documented in Colin Bryar and Bill Carr's *Working Backwards*. The McKinsey / BCG one-pager conventions (from consulting) are a related tradition.

### Q6. What is the difference between writing for an engineer-reader and a leader-reader?

**Answer:**

The same content written for different audiences requires very different prose. A senior engineer writes to both, often in the same week.

**For the engineer-reader:**

- They want detail. "How does it work?" is a valid question.
- Jargon is a shortcut, not a barrier. "Consistent hashing" saves a paragraph.
- Code and pseudo-code are welcome.
- They can follow technical arguments, trade-offs, and failure modes.
- They want the reasoning, not just the conclusion.
- Tone is peer-to-peer. Hedging looks weak; strong claims invite debate.

**For the leader-reader (exec, director, senior leader):**

- They want the bottom line. "What should I do?" is the driving question.
- Jargon is a barrier. "Consistent hashing" needs translation or avoidance.
- Code is rarely helpful.
- They trust your technical conclusions; what they need is the business impact.
- They care about risk, cost, timeline, customer impact.
- Tone is briefer. Strong claims with support.

**What to translate:**

| Engineer framing | Leader framing |
|-----------------|----------------|
| "Move to trunk-based development" | "Change how we ship — from branch-based to continuous — to cut time-to-market for fixes" |
| "Migrate from MySQL to Postgres" | "Replace our primary database; $X migration cost, Y weeks, reduces vendor risk" |
| "Adopt a service mesh" | "Add a layer to our infrastructure that improves reliability and observability; Y quarters of investment" |
| "The bug was in our cache invalidation logic" | "A design flaw in how data is refreshed caused the outage; the fix is in flight" |

**The senior engineer's superpower:** Writing one document that works for both. The engineer-reader reads the detail; the leader-reader reads the first paragraph and the summary. BLUF plus structured detail achieves this.

**Anti-pattern:** Writing "up" so much that engineer-readers find it useless, or writing "down" so much that leader-readers don't get it. Both audiences deserve respect.

---

## Intermediate

### Q7. How do you run an effective design review meeting?

**Answer:**

The design review is where a design doc becomes a decision. Run well, it saves months. Run badly, it becomes another meeting that achieves nothing.

**Before the meeting:**

1. **Send the doc at least 24 hours in advance.** Reviewers read on their schedule.
2. **Make reading mandatory.** If the meeting opens with "let me walk through the doc," you've lost. Reviewers should arrive with comments pre-written.
3. **Call out the specific questions.** "I'd especially like input on the data model in §3 and the rollout risk in §6." Focuses attention.
4. **Name the decision needed.** "At the end of this meeting I want an approval to proceed, or a list of blocking concerns."
5. **Invite the right people.** Not everyone who might have an opinion — the people whose buy-in you need. Larger meetings produce worse decisions.

**Amazon's "reading-in-silence" model:**

A variant: the meeting starts with 15–30 minutes of silent reading. Everyone reads the doc and adds comments. Discussion begins after. This enforces that reviewers actually read the doc and levels the playing field between fast-talkers and careful-thinkers.

**During the meeting:**

1. **Facilitator is not the author.** The author needs to engage the content; someone else runs the meeting clock.
2. **Work the comments, not the narrative.** Go to the comments left on the doc in priority order.
3. **Separate blocking from non-blocking.** Be explicit: "This is a blocker / this is a suggestion / this is a question."
4. **Drive to decisions, not to consensus.** Consensus is a bonus; a clear decision is the requirement.
5. **Capture open questions.** If something can't be resolved in the meeting, name it, assign an owner, set a deadline.

**After the meeting:**

1. **Summarise the outcome in the doc.** Decision, dissent, open questions. Written down.
2. **Update the doc.** Reflect agreed changes. Change status ("Approved, 2026-04-15").
3. **Close open questions asynchronously.** Via doc comments, Slack, follow-up meeting — whatever works.

**Common failures:**

- **Rubber-stamp review.** Reviewers arrive unprepared, agree to everything, incidents follow.
- **Bike-shedding.** Discussion focuses on the colour of the bike shed (naming, minor conventions) instead of the nuclear reactor (core design). Facilitator's job to redirect.
- **No decision at the end.** "Let's think about it" is failure. Even "we can't decide yet, we need X" is a decision.

**Reference:** Amazon's narrative-meetings culture (*Working Backwards*, Bryar & Carr). Will Larson's writing on tech-spec reviews. The "decider" role in the Sprint / Design Sprint literature (Jake Knapp).

### Q8. How do you give and receive feedback on a design doc?

**Answer:**

Design doc feedback is where the craft of technical writing meets the craft of code review. The same disciplines apply; the stakes are often higher (a single doc shapes a quarter of engineering work).

**Giving feedback on a design doc:**

1. **Read the whole thing before commenting.** Anchoring on the first section distorts judgement.
2. **Comment on the design, not the writing (first).** A typo can be fixed later; a design flaw can't.
3. **Separate blocking from non-blocking.** Same conventions as code review — `nit:`, `suggestion:`, `blocking:`. Make your intent explicit.
4. **Ask before asserting.** "Have you considered X?" invites dialogue. "You should have done X" invites defensiveness.
5. **Propose alternatives when you push back.** "This approach worries me because Y; have you looked at Z?" is more useful than pure criticism.
6. **Be specific.** "Section 3.2 concerns me" is weak. "The claim in §3.2 that consistency is eventual conflicts with the customer-expectation in §1" is useful.
7. **Acknowledge strengths.** Not platitudes — "the rollout plan is unusually thorough" is a real signal that the doc has done good work.

**Receiving feedback on your design doc:**

1. **Don't defend — understand.** The reviewer's comment may be wrong, but first make sure you understand what they're saying.
2. **Separate signal from noise.** Track which comments are blocking, which are suggestions, which are preferences, which are out of scope.
3. **Respond in writing.** Answer each comment in the doc thread, even if the answer is "disagree, here's why."
4. **Update the doc.** Resolved comments should result in doc changes. Don't let the doc drift from the agreed state.
5. **Don't disappear.** A doc that sits with unresolved comments for weeks blocks the whole project.
6. **Thank the reviewers who spent real effort.** Publicly if warranted. Good review is work; recognise it.

**Common failures:**

- **Author takes feedback personally.** Design is not identity. Feedback on the design is not feedback on you.
- **Reviewer writes "I wouldn't do it this way" feedback without engaging the why.** Unhelpful.
- **Feedback that stops at the problem.** "This won't scale" is half a comment; "this won't scale at N; consider Y" is a whole one.
- **The author changes the doc silently without responding.** Reviewers can't tell which comments were accepted, rejected, or misunderstood.

### Q9. How do you write a status update that doesn't waste the reader's time?

**Answer:**

Most status updates are wasted. Readers skim or skip them; authors write from inertia. The senior-engineer move is to make updates that are actually read and acted on.

**Core principles:**

1. **Clarity of audience.** Who's reading this? What do they need from it? If you can't answer, don't write.

2. **Status colour, explained.** Green / Yellow / Red isn't useful without "because X." The colour draws attention; the explanation acts on it.

3. **Focus on outcomes, not activity.** "We had three design reviews" tells the reader nothing. "The design is ratified; implementation starts Monday" tells them everything.

4. **Surface risks proactively.** Readers want to know what might go wrong — it's what they can act on. Hiding risk breeds distrust.

5. **Explicit asks.** "I need approval for X by Friday" is more actionable than burying the request.

6. **Cadence consistency.** Weekly at the same time, every time. Missing updates erode the signal.

**A template that works:**

```markdown
## [Project] — [Date] — [Color]

**Why the color:** [1 sentence]

### Shipped since last update
- ...

### In flight
- ...

### At risk
- ... (and what we're doing about it)

### Asks
- ... (or "none this week")

### Next update: [Date]
```

**Anti-patterns:**

- **Activity logs.** Listing everything the team did this week. Readers don't care.
- **Status drift.** Green four weeks running and then suddenly Red. Either the colour was wrong before or communication was.
- **Vague milestones.** "Making progress" is not a status. Either "X done" or "X blocked by Y."
- **No ETA changes.** Tracking when an ETA moves and why is more valuable than the ETA itself.

**STAR framing:**

> **S:** Leadership complained that our multi-team platform initiative was opaque — they couldn't tell what was going on.
> **T:** As tech lead I was asked to fix the status comms.
> **A:** I replaced the weekly 20-slide deck with a 1-page written update: colour, what changed since last week, at-risk items, asks. I cut the status meeting from 60 to 20 minutes with the freed time going to reading the written update beforehand.
> **R:** Leaders stopped asking "what's the status of X?" in hallways — they read the update. The team saved about 2 engineer-hours/week and made decisions faster because the status was now legible.

### Q10. How do you communicate bad news in writing?

**Answer:**

Bad news — a slipped date, a failed experiment, a scope cut — is harder to write than good news, which is why so many engineers write it badly. The senior move is to communicate bad news clearly, on time, and framed for action.

**Principles:**

1. **Tell people early.** Bad news doesn't improve with age. Waiting to tell makes the eventual telling worse.

2. **Lead with the bad news.** BLUF applies here more than anywhere. Don't bury it.

3. **Own it.** "We missed the date" not "the date was missed." Passive voice signals avoidance.

4. **No surprises in the escalation chain.** Your boss shouldn't hear bad news about your team from their boss. Tell your boss first, give them time to prepare.

5. **Separate the bad news from the plan.** First, the fact. Then, what you're doing about it. Don't bury the fact in a plan.

6. **Quantify if you can.** "Delayed" is soft; "delayed by 6 weeks, currently at 40% complete of a 100% scope" is hard.

7. **Don't over-apologise.** One acknowledgement is professional; repeated apology is uncomfortable for the reader.

**Template:**

```markdown
Subject: [Project X] — date slipping to [new date]

**We are moving the [Project X] target date from [old] to [new].**

**Why:** [2-3 sentences — honest diagnosis]

**What's still on track:** [Scope-preserving or reducing impact]

**What we're doing:**
- [Action 1]
- [Action 2]

**What we need:** [Help, decisions, or "nothing — we have it"]

Happy to discuss.
```

**Anti-patterns:**

- **Softening language.** "Slight adjustment to the timeline" for a two-quarter slip is infuriating to read.
- **Spin.** "We learned a lot" is fine as a side note; it's not an explanation of why you missed the date.
- **Over-explaining.** A five-paragraph contextualisation buries the news.
- **Blaming.** "This was because the platform team didn't ship X" reads as deflection even if it's true.

**Reference:** Kim Scott, *Radical Candor*, on direct communication. Patrick Lencioni's work on teams that can deliver bad news up.

### Q11. How do you give a technical talk that lands with a mixed audience?

**Answer:**

Technical talks — at team meetings, internal conferences, external meetups — are a major growth vector for senior engineers. Public speaking isn't the point; the point is spreading ideas at scale.

**Structure that works for most technical talks:**

1. **Hook (0–1 min).** A concrete story, a surprising statistic, or a question. Make them want to hear the rest.
2. **Context (1–3 min).** Why does this matter? What's the stakes?
3. **The core (5–20 min).** The actual content — be disciplined about scope.
4. **Takeaways (1–2 min).** Three things they should remember. State them explicitly.
5. **Q&A.** Plan what you want the first question to be; have an answer ready for the hostile one.

**Calibration for a mixed audience:**

- **Start accessible.** The first five minutes should work for the least-expert person in the room.
- **Deepen progressively.** The second half can be technical for the deepest quartile.
- **Signpost.** "If you're familiar with X, bear with me for a minute; the rest won't make sense otherwise."
- **Have a deeper-dive asset.** A doc, a GitHub repo, a follow-up talk — so the experts don't feel short-changed.

**Slide design (when used):**

- **One idea per slide.** A slide with three ideas is a confused slide.
- **Minimal text.** The audience reads your slide or listens to you, not both.
- **Large fonts.** At the back of a room, small text is invisible.
- **No read-the-slide.** If you can read your slide to them, they don't need you.

**Voice and delivery:**

- **Slow down.** Nerves speed you up. Aim for 2/3 your rehearsal pace.
- **Pauses are powerful.** Silence after a key point lands it. Don't fill every second.
- **Make eye contact.** In person — pick 3–4 spots in the room and rotate. Virtually — look at the camera.
- **Practice the opening and closing especially.** You never flub the middle; you always flub the ends under stress.

**Recovering from mistakes:**

- If you forget something, move on. Most of the audience didn't know what you planned.
- If you say something wrong, correct it matter-of-factly. "Actually, X is Y — I misstated earlier."
- If a slide breaks, talk to it anyway. Contingency is part of the craft.

**Reference:** *Talk Like TED* (Carmine Gallo) for structure. *Presentation Zen* (Garr Reynolds) for slide design. For engineering-specific: Rich Hickey's talks are widely studied as models of clear technical communication.

### Q12. What makes async communication work well across distributed teams?

**Answer:**

Async-first teams live or die on the quality of written communication. The practices that work in a co-located team (brief Slack ping, quick meeting) fail across time zones.

**The core disciplines:**

1. **Default to writing.** For anything non-trivial, written beats spoken. Writing is async, searchable, and forces clarity.

2. **BLUF everything.** Thread titles, doc titles, Slack messages, emails. The reader in a different time zone should know in the first line whether they need to engage.

3. **Invest in writing time.** A 30-minute doc that saves 5 meetings is a bargain. Teams that under-invest in writing pay in meetings.

4. **One source of truth.** A Slack message, an email, a Jira comment, and a wiki page saying different things is a failure mode. Pick the canonical place per topic.

5. **Document decisions, not just discussions.** Threads show discussion; future readers need the decision summarised somewhere findable.

6. **Reduce synchronous dependency.** Every "let's jump on a call" has a cost across time zones. Ask: could a doc or written back-and-forth solve this?

7. **Respect time zones explicitly.** Name the "I'll get to this in my tomorrow morning" expectation. Don't expect instant response.

**Async-friendly tooling patterns:**

- **Short, written 1:1 agendas.** Both parties add items asynchronously before the call.
- **Design doc with inline comments.** Replaces the design meeting, or precedes it.
- **Recorded video updates.** A 3-minute Loom is async-watchable and richer than a doc for some updates.
- **Written status updates.** Weekly, in writing, at a consistent cadence and location.
- **Public-by-default channels.** #team-x-general beats #team-x-private-leads. Information flow compounds.

**Failure modes in async teams:**

- **Meeting creep.** As the team grows, people default to meetings for comfort. Async discipline has to be actively maintained.
- **Hero-dependencies.** One person always answers. Their PTO stalls the team. Cross-training and documentation are the fix.
- **Thread graveyards.** Discussions start, peter out, and the decision is never captured. Sweep threads weekly; write decisions down.
- **Presence theatre.** Judging performance by how often someone's green online. Destroys trust across time zones.

**Reference:** GitLab's publicly available Handbook on async work is the reference. Basecamp's *Shape Up* and *It Doesn't Have to Be Crazy at Work* describe a highly async-first culture. *Remote: Office Not Required* (Fried & Hansson) on async fundamentals.

---

## Advanced

### Q13. How do you write for people who will read your doc without you there to explain it?

**Answer:**

Most writing at scale is consumed by readers you'll never meet — engineers in future quarters, colleagues in other teams, new hires in a year. Writing for them is a different craft than writing for the people in the meeting.

**What the un-present reader needs:**

1. **Context you take for granted.** "We use Service X" — what is Service X? Link to it or state it briefly.
2. **Decisions, not just conclusions.** "We chose Y" — why did we choose Y? What did we reject? What's the trade-off?
3. **Unambiguous language.** "The system" — which system? Name it.
4. **Date stamps.** "Recently" means different things in April and in October. "As of 2026-04-15" is durable.
5. **Linked sources.** Claims need citations. "Latency was 150ms" — linked to the dashboard, the report, the ticket.
6. **Terminology defined.** If the doc uses jargon specific to the team, define it on first use or link to a glossary.

**Techniques:**

- **Write, then read it a week later as a stranger.** What confuses you? That's what needs fixing.
- **Have someone un-involved read it.** "Does this make sense?" Their confusion points to gaps.
- **Record the verbal context you'd give.** If you'd explain the doc live with three sentences of intro, add those to the doc.
- **Include "this is not about" sections.** Negatives orient the reader. "This doc doesn't cover X; see [other doc]."

**The senior-engineer muscle:**

Writing for strangers is uncomfortable at first — it feels over-explained, too long, pedantic. With practice, you build a model of the un-present reader, and you write for them naturally. The artefacts survive you.

**Anti-patterns:**

- **Insider writing.** Everything is in-group code. New readers have no entry point.
- **Over-reliance on memory.** "As discussed in the meeting last Tuesday" is unreferencible.
- **Slack-as-documentation.** Threads aren't discoverable; Slack search is terrible; messages disappear.
- **Docs in disposable tools.** Write in durable media. Google Docs is better than Figma comments; a wiki is better than a Google Doc; a repo is better than a wiki, for code-adjacent content.

### Q14. How do you handle writing when you're not a native speaker of the primary language of the team?

**Answer:**

Tech teams are multilingual. The primary language (usually English) is the lingua franca, and many strong engineers operate in it as a second or third language. Senior engineers writing in a non-native language need specific strategies, and team leads need to support them.

**For the non-native writer:**

1. **Optimise for clarity, not elegance.** Short sentences, active voice, concrete nouns. Readers prefer clear-and-slightly-plain to elegant-but-ambiguous.
2. **Use tools.** Grammarly, ChatGPT, DeepL. They'll catch prepositions, articles, and tense issues that otherwise make prose feel off.
3. **Ask a trusted native-speaker peer to read drafts.** Especially for high-stakes writing. Return the favour in technical review.
4. **Read widely in the target language.** Your prose converges on what you read. Read good engineering writing; your writing will get better.
5. **Record yourself speaking the doc.** If you wouldn't say it, don't write it. Helps surface awkward phrasing.
6. **Over-structure.** Clear headings, bullets, numbered lists compensate for prose that may feel rough. Structure is language-neutral.

**For the team leader supporting non-native colleagues:**

1. **Don't make writing a proxy for technical ability.** A brilliant engineer writing in their second language looks less brilliant on a doc than a mediocre native-speaker peer. Separate the technical signal from the writing signal.
2. **Offer review, not correction.** Editing someone's doc without asking feels like criticism of their English. Offering to review their doc is support.
3. **Translate in both directions.** If critical meetings happen in English, make sure follow-up docs capture what was decided in plain language. Also model writing clear English for the benefit of non-native readers.
4. **Accept that the lingua franca has evolved.** International English drops idioms and simplifies grammar. That's a feature, not a failure.
5. **Invest in tools.** Grammarly for the team. Live transcription / translation in meetings where useful.

**Anti-patterns:**

- **Correcting grammar in front of others.** Humiliating and unnecessary.
- **Treating language as a hiring filter.** Brilliant engineers exist across languages. Requiring flawless English narrows the pool artificially.
- **Assuming silence means agreement.** In cross-cultural, cross-language teams, silent in-meeting participation doesn't mean consent. Ask, then give async channels to confirm.

### Q15. How do you tell a compelling technical story in writing?

**Answer:**

Storytelling is underused in engineering writing. The best technical docs, post-mortems, and internal blog posts have a story shape — not a feature dump.

**What "story" means in a technical context:**

- A protagonist (the team, the system, the engineer).
- A problem (the incident, the requirement, the impasse).
- Stakes (what happens if it goes wrong).
- A journey (what was tried, what failed, what was learned).
- A resolution (the decision, the fix, the outcome).

**Why it works:**

- Stories are memorable. A feature list is forgotten in a week; a story is remembered in a year.
- Stories create empathy. Readers understand the "why" through the narrative, not just the "what."
- Stories carry nuance. Trade-offs feel natural in a story; they look like defeats in a feature list.

**Examples of technical storytelling:**

- **Post-mortems as stories.** "At 14:03 our on-call received the page. They checked the dashboard — nothing obvious. Then the second page arrived..." The timeline creates tension; the reader learns through the narrative.
- **Decision docs as stories.** "We started assuming X. The more we investigated, the more we found Y. Eventually the data was overwhelming..." The reader follows the reasoning, not just the conclusion.
- **Internal blog posts about a project.** "Six months ago we had three engineers and no pipeline. Today we have..." The arc carries the reader.

**Structural moves:**

1. **Open with specificity.** A date, a name, a number. Not "recently we noticed..."
2. **Chronology often beats logic.** A logical argument feels cold; a chronological telling feels human.
3. **Include the wrong turns.** Stories where everything went right are suspicious. The wrong hypothesis, the dead end, the surprised engineer — these are what makes the story credible.
4. **Name the people.** "The on-call engineer" is anonymous; "Priya, on-call that morning" is specific. (Get permission before naming in public writing.)
5. **End with the lesson, not the resolution.** The resolution closes the plot; the lesson is why you told the story.

**Anti-patterns:**

- **Over-dramatisation.** Engineering stories don't need cliffhangers. Measured prose lands better than breathless.
- **Hero worship.** "Thanks to the heroic efforts of..." Tells the reader someone was heroic; shows nothing. Let the facts speak.
- **Lost thread.** A technical story can wander into depth and lose the narrative. Check: does each paragraph advance the story?
- **Storytelling as a substitute for rigor.** A charming story without data can win a meeting and lose the project. Use both.

**Reference:** *Made to Stick* (Chip and Dan Heath) on what makes ideas memorable. *Bird by Bird* (Anne Lamott) on the writing craft. Will Larson's blog posts and Dan Luu's blog are excellent models of technical storytelling in engineering.

### Q16. How do you write for influence rather than just for information?

**Answer:**

Informational writing tells readers what's true. Influential writing changes what they do. Senior-engineer writing is often the second kind — proposing a change, shifting a strategy, moving a decision.

**What influence writing requires that information writing doesn't:**

1. **A clear ask.** "I want you to approve X" or "I want you to move toward Y." Readers who can't articulate what you want from them won't do anything.

2. **Shared framing.** Readers have to see the problem the way you see it before they'll accept your solution. Build shared framing before proposing the solution.

3. **Acknowledged counter-arguments.** The reader is thinking of objections as they read. Pre-empting them shows you've considered them and reduces resistance.

4. **Evidence at the reader's calibration.** Data for the data-driven; principles for the principled; stories for the story-minded. Know your reader.

5. **Cost honestly stated.** Influence writing that hides cost is eventually exposed, and credibility is ruined. Front-load the cost.

6. **A bias toward action.** End with what happens next. Readers should close the doc knowing what to do.

**Rhetorical moves (that work):**

- **Concede early.** "The current approach has these strengths..." puts you on the reader's side before you make your case.
- **Steelman opposition.** Present the counter-view better than its proponents would. If you can still make your case after that, you've won.
- **Use "we" when you can.** "We need to decide" invites joint ownership. "You need to decide" puts the burden on the reader. "I recommend" puts it on you.
- **Name the trade-off openly.** "Choosing X means giving up Y. Here's why we should." Readers trust honesty about cost.

**Moves that don't work:**

- **Appeals to authority alone.** "Google does it this way" doesn't make it right for you.
- **Hyperbole.** "This is the most important decision of the year" if it isn't, credibility is shredded.
- **Hiding dissent.** If senior engineers on the team disagree and the doc doesn't say so, readers will find out later.
- **Manipulation.** Any perception that you're trying to steer the reader rather than inform them kills the piece.

**STAR framing:**

> **S:** I needed to move our org from three competing observability stacks to one. The technical case was clear but each team had invested in their current tool.
> **T:** I had to write a proposal that would land with engineers (who cared about features), leaders (who cared about cost), and incumbent-tool advocates (who would be most resistant).
> **A:** I wrote a 6-page RFC structured around the costs of the status quo (outage context-switching, training tax, duplicated work) with data. I steelmanned each incumbent: "If you're a strong advocate for Tool X, here's what you'd lose if we consolidated to Tool Y — and here's why the loss is worth the gain." I included migration commitments — not just decree, but "we'll do the migration work for you."
> **R:** The RFC was approved with two weeks of async review. Incumbents' objections were absorbed in advance. Migration took a year; the decision held because the case had been made honestly. Post-consolidation survey had 8/10 satisfaction, not 10/10 — because honest trade-offs were real.

**Reference:** *Thinking, Fast and Slow* (Kahneman) for the cognitive basis. *Made to Stick* (Heath & Heath) on what makes ideas travel. Aristotle's *Rhetoric* is 2,300 years old and still directly applicable.

### Q17. What are the habits of senior engineers who write consistently well over years?

**Answer:**

Writing well once is a talent. Writing well consistently over years is a practice. The engineers who become known for their writing (Rich Hickey, Dan Luu, Will Larson, Tanya Reilly, Julia Evans, Gergely Orosz) have patterns in common.

**Observable habits:**

1. **They write on a cadence.** Weekly, fortnightly, monthly — something regular. The deadline forces output; the output forces thinking.

2. **They write about what they're currently doing.** Their work feeds the writing; the writing clarifies the work. Writing about things you don't do reads as abstract.

3. **They edit ruthlessly.** First drafts are bad — theirs too. They cut, restructure, rewrite. Many good writers throw away the first paragraph of everything.

4. **They read widely.** Inside and outside software. Good writing comes from consuming good writing. Staff engineers who only read RFCs write like RFCs.

5. **They have a publishing mechanism.** Blog, internal wiki, mailing list. Something that creates a record and a small audience. Writing without publishing atrophies.

6. **They invite feedback.** Not defensively — genuinely. Good writers revise based on reader confusion. Ego-less writers get better faster.

7. **They specialise.** Not always in topic, but in angle. Will Larson writes on leadership and engineering management. Dan Luu writes on the gap between systems-theory and systems-practice. The specialisation builds recognition.

8. **They write shorter over time.** Early essays are often too long. With practice comes compression. A senior engineer's blog post is usually shorter than a junior engineer's essay on the same topic.

9. **They keep a "drafts" pile.** Not everything they write gets published. Some essays ferment for months. Some die quietly. The filter improves the output.

10. **They don't abandon their voice.** Good writers sound like themselves. They don't imitate; they develop their own register and stick with it. Trying to sound like someone else produces forgettable writing.

**What to do about it (as a growing senior engineer):**

- Pick a cadence. Monthly internal-blog posts is a reasonable starting point.
- Pick an angle. "Observations from debugging" or "patterns I see across teams" — something narrow enough that you can notice material.
- Start a drafts folder. Write whenever you have 30 minutes and a thought. Don't publish immediately.
- Read at least one piece of good writing per week. Outside your domain sometimes. Notice what makes it work.
- Ask a colleague to critique one piece a quarter. Return the favour. Writing partners accelerate both parties.

**The compounding effect:**

Writing improves at a rate that feels imperceptible in weeks, obvious in years. Staff engineers with five years of weekly writing are unrecognisable as writers compared to their starting point. The craft compounds.

**Reference:** *On Writing* (Stephen King) for the craft in general. *Bird by Bird* (Anne Lamott) on the discipline. *Show Your Work* (Austin Kleon) on the publishing habit. The essays of Paul Graham, though controversial in places, are a worked example of the "write consistently, publish, edit mercilessly" pattern over 20 years.
