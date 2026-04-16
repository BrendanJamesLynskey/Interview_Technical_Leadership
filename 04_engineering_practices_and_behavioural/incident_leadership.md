# Incident Leadership — Interview Questions

**Subject:** Technical Leadership
**Topic:** Leading Incidents, Blameless Culture, Communication During Outages, Executive Updates, Post-Incident Reviews
**Difficulty tiers:** Fundamentals / Intermediate / Advanced

---

## Fundamentals

### Q1. What is an incident commander and what is their role?

**Answer:**

An **incident commander (IC)** is the single person accountable for coordinating the response to an incident. Borrowed from the US fire service's Incident Command System (ICS), the role separates *coordination* from *technical execution*.

**Core responsibilities:**

1. **Run the incident.** Set the cadence, decide the structure of the response, keep the room organised.
2. **Make decisions.** When the technical responders disagree on next steps, the IC decides — quickly. A slightly-wrong decision now beats a perfectly-correct decision later.
3. **Manage communication.** Internal updates to stakeholders, external comms via the comms lead, executive updates as required.
4. **Protect responders' focus.** Shield the engineers who are debugging from interruptions. The IC takes the meeting; the engineer keeps typing.
5. **Decide when to escalate.** More responders, more senior leaders, vendor support — the IC owns these calls.
6. **Declare resolution.** When the impact is contained and verified, the IC formally ends the incident.

**What the IC is NOT:**

- The IC is not (usually) the deepest technical expert. They're the coordinator.
- The IC is not the customer-comms author (that's the comms lead).
- The IC is not the post-incident analyst (that role is separate, deliberately).

**Why the role exists:**

Without an IC, every incident has the same failure mode: too many people in the channel, no one driving, decisions stalled, comms going out inconsistently or not at all. The IC role compresses chaos into coordination.

**Reference:** Google's *Site Reliability Engineering* book (chapter on incident management) describes their IC role explicitly. PagerDuty publishes their incident response process publicly. The original FEMA/NIMS Incident Command System documentation is freely available.

### Q2. What is "blameless culture" and why does it matter for incidents?

**Answer:**

**Blameless culture** is the explicit organisational stance that incidents are learning opportunities, not occasions for individual punishment. The post-incident review focuses on the systems, processes, and conditions that allowed the incident to occur — not on who pushed the bad button.

**The core claim:**

People do their best with the information, time, and tools they have. When something goes wrong, the more useful question is "what made this seem reasonable at the time?" not "who do we blame?"

**Why it matters operationally:**

- **Information flow.** Engineers who fear blame hide information. Hidden information makes the next incident worse.
- **Reporting.** In blameful cultures, near-misses go unreported. You lose the cheap learning opportunities.
- **Speed of resolution.** Engineers freeze when they think a wrong move will get them fired. Blameless culture lets them act.
- **Retention.** Talented engineers don't stay in cultures where being on-call during an outage is career-limiting.

**What blameless does NOT mean:**

- Not "no accountability." Teams are still accountable for outcomes; the *system* changes are what's expected, not personal punishment.
- Not "no consequences." If an engineer repeatedly violates documented procedure with intent, that's a different conversation than blame for an honest mistake.
- Not "no names." Accurate post-mortems mention people; they describe their actions in context, without attributing motive.

**The phrasing that works:**

- Instead of "Bob deployed without testing" → "The deployment process did not require pre-deploy verification, so Bob proceeded under the standard practice for that service."
- Instead of "the on-call missed the alert" → "The alert was paged at 3:47am along with 14 unrelated alerts; the on-call triaged the highest-severity-looking ones first; this one was not visually flagged as P0."

**Reference:** John Allspaw's "Blameless Post-Mortems and a Just Culture" (Etsy blog, 2012) is the foundational essay. Sidney Dekker's *The Field Guide to Understanding Human Error* is the academic anchor. Google SRE book, chapter on post-mortems.

### Q3. What information should be in the first internal communication during an incident?

**Answer:**

The first comms during an incident sets the tone for everything that follows. It's read by stakeholders who need to make decisions — customer support, exec leadership, partner teams. Get the structure right and you save hours of follow-up questions.

**The minimum structure:**

```markdown
**INCIDENT — [Severity] — [Date Time TZ]**

**What's happening:** [One sentence — what's broken, who is affected]

**Impact:** [Customer-visible / internal / scope]

**IC:** [Name]
**Tech lead:** [Name]
**Comms:** [Name]
**Channel:** [#incident-NNN]

**Status:** Investigating / Identified / Mitigating / Resolved
**Next update:** [Time — be specific, not "soon"]
```

**Why each part matters:**

- **Severity** lets readers decide if they need to engage now or read later.
- **What's happening** in one sentence prevents misreading. If readers have to scroll to understand, you've lost them.
- **Impact** is what stakeholders care about, not the technical cause.
- **Roles** (IC, tech lead, comms) tell people who to ask.
- **Channel** tells people where to go for live updates without interrupting the response.
- **Status** uses a small standardised vocabulary so updates can be parsed at a glance.
- **Next update** sets expectations. "We'll update in 15 minutes" is reassuring. Silence is not.

**Common mistakes:**

- **Too much technical detail in the first comm.** Save it for the in-channel discussion.
- **Vague impact.** "Some users affected" tells stakeholders nothing. "Approximately 5% of EU customers cannot complete checkout" lets them act.
- **No update cadence.** Stakeholders ping the channel anxiously. Set the cadence and stick to it.
- **No named roles.** "We're working on it" with no IC named means everyone's working on it and no one is leading.

**Reference:** PagerDuty's incident comms templates are publicly published. Atlassian's incident handbook is similarly open.

### Q4. What is the difference between mitigation and resolution?

**Answer:**

This distinction matters more than most engineers realise.

**Mitigation** stops the customer impact. The bug is still there; you've worked around it. The fire is contained.

**Resolution** removes the cause. The bug is fixed; the system is back to its normal operating model.

**Why the distinction matters during incidents:**

- The incident commander's first priority is mitigation, not resolution. Stopping customer impact is more urgent than understanding why.
- Engineers often (wrongly) hold off on mitigation because they don't yet understand the cause. The IC's job is to push: "What can we do *right now* to stop the bleeding?"
- Common mitigations: rollback, feature flag off, traffic divert, scale up, restart, failover. None of these requires understanding the root cause.
- Once mitigated, the incident's urgency drops. The cause investigation can proceed at a sustainable pace.

**The framing in incident channels:**

- "Are we mitigating or are we still investigating?" — clarifies what stage we're in.
- "Is this mitigated, or just calmed down?" — distinguishes real mitigation from things that happen to look better.
- "What's the ETR (estimated time to resolution)?" — distinct from ETM (estimated time to mitigation).

**Anti-pattern — "we'll fix it properly":**

Engineers (especially senior ones) sometimes refuse to mitigate because they want to fix the underlying issue. This trades hours of customer impact for engineering pride. The IC's job is to override this and demand mitigation now, fix later.

**Reference:** Google SRE book, chapter on managing incidents. Allspaw's writing on incident anatomy.

### Q5. What is a runbook and when is it useful during an incident?

**Answer:**

A **runbook** is a written procedure for handling a known operational scenario — a deployment, a routine maintenance task, or a recurring failure mode. The pre-cooked steps allow on-call engineers to act decisively without having to think it through under pressure.

**Why runbooks matter for incidents:**

- **3am cognition.** Engineers paged at 3am don't think as clearly as they do in daylight. A runbook compensates.
- **Spreading knowledge.** A runbook lets engineers other than the original author handle a scenario.
- **Speed.** Following a runbook is faster than reasoning from scratch.
- **Consistency.** Different on-calls handle the same scenario the same way.

**What makes a good runbook:**

- **Triggered by a specific symptom.** "If alert X fires, follow this." Not generic.
- **Step-by-step, with verification.** Each step has a "what should happen" check.
- **Includes escalation triggers.** "If step 3 fails, page the database team."
- **Acknowledges uncertainty.** "If you see Y, the runbook does not cover this — escalate."
- **Last-updated date.** Runbooks rot. Stale runbooks are dangerous.

**Anti-patterns:**

- **Runbooks for things that should be automated.** If the runbook is "ssh in and run script X," put script X in a button. Manual runbooks are a transitional state.
- **Encyclopaedic runbooks.** A 40-page runbook won't be read at 3am. Keep them short and focused.
- **Runbooks as theatre.** Some teams write runbooks because they "should." If no one consults them, they don't exist.
- **No runbook tests.** A runbook that's never been executed in earnest doesn't work. Game days verify them.

**Senior engineer behaviour:** After every incident, ask "should there be a runbook for this?" If yes, write one (or update the existing one). Runbook coverage is a leading indicator of operational maturity.

**Reference:** Google SRE Workbook, chapter on operational practices. The "you build it, you run it" doctrine (Werner Vogels at Amazon) implies runbooks as part of every team's deliverables.

### Q6. What is a post-incident review (post-mortem) and what is its purpose?

**Answer:**

A **post-incident review (PIR)** — also called a post-mortem, retrospective, or after-action review — is a structured analysis of an incident, conducted after resolution, to extract learning that prevents or mitigates future similar incidents.

**Purposes (in priority order):**

1. **Organisational learning.** What can we change so this category of incident is less likely or less impactful next time?
2. **Knowledge spread.** Engineers who weren't involved in the incident benefit from understanding it.
3. **Action items.** Concrete changes — code, process, monitoring, training — that follow from the analysis.
4. **Customer / stakeholder communication.** A redacted PIR may inform external comms.

**What a post-mortem is NOT:**

- A blame allocation. (See blameless culture.)
- A status update. (Status was during the incident.)
- A celebration. (Resolution is its own moment.)
- A management performance review. (Different process.)

**Standard structure:**

```markdown
## Post-Incident Review — [Incident ID]

**Date / Duration / Severity / IC**

### Summary
[1-paragraph what happened, what was the impact, how it was resolved]

### Timeline
[Detected / declared / mitigated / resolved — with key events between]

### Customer Impact
[Quantified — users, requests, revenue, SLO burn]

### Contributing Factors
[Multiple — never just one root cause]

### What Went Well
[Genuine — not platitudes]

### What Went Poorly
[Honest — without blame]

### Action Items
[Owner, deadline, priority — tracked to completion]

### Lessons / Themes
[What does this incident teach us about our systems?]
```

**Why "contributing factors" not "root cause":**

Modern incident analysis (Allspaw, Dekker, Cook, Woods) rejects "the" root cause. Real incidents have many contributing factors — code bug, missing monitoring, ambiguous runbook, on-call fatigue, organisational pressure. Treating one as "the" cause hides the others.

**Reference:** The "How Complex Systems Fail" essay by Richard Cook (1998) is short, free, and influential. *Learning from Incidents in Software* (Lorin Hochstein et al.) collects modern essays. Etsy's, Google's, and Stripe's published post-mortems are good public examples.

---

## Intermediate

### Q7. How do you decide the severity of an incident?

**Answer:**

Severity drives almost everything in the response — who gets paged, how often updates go out, whether executives are informed, how aggressively comms are escalated. Getting it wrong (in either direction) is costly.

**A typical severity ladder:**

| Severity | Definition | Response |
|----------|------------|----------|
| **SEV1 (P1)** | Critical impact — major customer outage, data loss, security breach | All-hands; exec comms within 30 min; war room |
| **SEV2 (P2)** | Significant impact — partial outage, degradation affecting many users | Dedicated IC and team; hourly updates; senior leader informed |
| **SEV3 (P3)** | Limited impact — small subset affected, workaround available | On-call team handles; daily update; tracked in dashboard |
| **SEV4 (P4)** | Minimal — internal only, no customer impact | Standard ticket; no incident process |

**Decision principles:**

1. **Err high, downgrade later.** It's much easier to declare SEV2 and downgrade than to declare SEV3 and discover it's actually SEV1. The cost of over-paging is small; the cost of under-paging is reputational.

2. **Customer impact first.** Severity is from the customer's perspective. "Internal-only" is rarely SEV1, even if it's hairy. "Checkout broken for 1% of EU users" might be SEV1 even if the system looks healthy.

3. **Use written criteria.** Each SEV level should have written, customer-impact-anchored criteria. Without them, severity becomes vibes.

4. **Severity can change.** A SEV3 that gets worse becomes SEV2. The IC re-declares. Don't keep an incident at the original severity out of inertia.

**Common mistakes:**

- **Sandbagging.** Declaring SEV3 to avoid the executive comms cycle. Bites the team when leadership finds out the impact was bigger than represented.
- **Severity inflation.** Every incident as SEV1 numbs the response. People stop taking SEV1 seriously.
- **Severity by author.** "Bob calls everything a SEV1." Severity should be by criteria, not by personality.

**STAR framing:**

> **S:** A latency degradation in our payments service was initially declared SEV3 by the on-call engineer.
> **T:** As staff engineer reviewing the response in real-time I had to decide whether to upgrade.
> **A:** I checked the customer impact — 8% of checkout attempts were timing out, not just slow. I asked the IC to upgrade to SEV2 and brought in the comms lead. I drafted the customer-facing message to support. We went to executive comms within the hour.
> **R:** We avoided a customer-trust incident — leadership was informed before customers started complaining publicly. The retro action item was to add timeout-rate to our SEV criteria explicitly so future on-calls wouldn't have to make the judgement call under pressure.

### Q8. How do you communicate an incident to executives without being either alarmist or evasive?

**Answer:**

Executive communication during incidents is a high-leverage senior-engineer skill. Good comms protect the team's credibility; bad comms damage it for years.

**What executives need to know (in priority):**

1. **Customer impact.** What is breaking, for whom, since when, at what scale.
2. **Business impact.** Revenue, regulatory, reputational, contractual.
3. **Where we are.** Investigating / mitigating / resolved.
4. **What we need.** Decisions, resources, exec engagement with customers/regulators.
5. **When we'll know more.** The next-update commitment.

**What executives don't need (yet):**

- Technical details of the failure mode.
- Blame attribution.
- Detailed remediation plans.
- Speculation on cause.

**The structure that works (for SEV1/SEV2):**

```markdown
**Subject: SEV1 — [System] — [Date Time]**

[1 sentence summary — what's broken, customer impact]

**Impact:** [Quantified — users, revenue, geographic scope]
**Status:** [Investigating | Mitigating | Mitigated | Resolved]
**Started:** [Time, with detection-vs-onset distinction if known]
**Current ETM/ETR:** [Or "estimating now" if genuinely unknown]
**IC:** [Name and contact]

**What we know:** [2-3 bullets — facts, not speculation]
**What we're doing:** [2-3 bullets — current actions]
**What we need from you:** [Decisions / resources / external comms / nothing right now]

**Next update:** [Specific time]
```

**Tone calibration:**

- **Honest about uncertainty.** "We don't yet know the cause" is better than guessing wrongly.
- **No hedging on impact.** If 10% of customers are affected, say 10%. Don't soften.
- **No engineering jargon.** "Database failover stuck" → "the system that stores customer data is in a degraded state we're working to recover."
- **No emotion.** Calm, factual. Executives panic when engineers seem to panic.

**Cadence:**

- For SEV1: every 30 minutes minimum, even if the update is "no new information."
- Always send the promised update, even with nothing new. Missing the cadence is worse than a thin update.

**Anti-patterns:**

- **Burying the lead.** "We had an interesting morning" is not the opening line.
- **Detail-dumping.** A 5-page exec update is read by no one.
- **Optimism inflation.** "We expect resolution shortly" said hourly for six hours destroys credibility.
- **Silence between updates.** Executives in the dark assume the worst.

**Reference:** PagerDuty's exec communication templates (public). Google SRE book on managing incidents at scale. The military "SITREP" (situation report) format is the original.

### Q9. What is "tunnel vision" during an incident and how do you counter it?

**Answer:**

**Tunnel vision** is the cognitive narrowing that happens to responders under pressure. They focus on one hypothesis, one component, one tool — and miss obvious facts that contradict their model. It's well-documented in aviation, medicine, and now in incident response literature.

**Symptoms:**

- Repeatedly checking the same dashboard despite no change.
- Holding a hypothesis for hours despite evidence against it.
- Ignoring suggestions from people not in the channel.
- Missing customer reports that suggest a different scope.
- Not noticing time has passed (3 hours feels like 30 minutes).

**Causes:**

- **Adrenaline.** The body narrows attention under stress.
- **Sunk cost.** "I've been debugging this hypothesis for 90 minutes; surely it's almost solved."
- **Anchoring.** The first hypothesis voiced becomes the dominant one.
- **Authority.** A senior engineer's hypothesis can crowd out alternatives.
- **Fatigue.** Late-night incidents amplify all of the above.

**Counter-measures:**

1. **Rotate the IC.** A fresh IC sees what the tunnel-vision IC missed. Standard practice for long incidents.
2. **Bring in fresh eyes.** Page someone who isn't in the channel. Make them ask the dumb questions.
3. **Re-state the facts every 30 minutes.** What do we actually know, factually, right now? This breaks the hypothesis-loop.
4. **Check the dashboard you've been ignoring.** Tunnel vision is symptomatic of "we keep checking the same one." Force diversity of inputs.
5. **Read the customer reports.** Often the customer description doesn't match the hypothesis. That's data.
6. **Eat. Drink water. Sit down.** Basic physical interventions. ICs should enforce break rotations for responders in incidents lasting more than 2 hours.
7. **Write down the current model.** Forcing it into words exposes its weaknesses.

**The IC's specific role:**

The IC is the most likely person to spot tunnel vision in others — they're not in the technical work. They should explicitly ask:

- "What if the cause is somewhere else entirely?"
- "What evidence would change our current hypothesis?"
- "Has anyone checked X?" (where X is a system not currently being looked at)

**STAR framing:**

> **S:** A 4-hour SEV1 had two senior engineers convinced the cause was a recent deploy in the payments service.
> **T:** As IC I noticed the deploy had been rolled back two hours prior with no recovery — but the team was still investigating it.
> **A:** I called a 5-minute pause. Re-stated the facts: "The deploy was rolled back at hour 2. Symptoms have not improved. What does that tell us?" The team realised they'd been chasing a hypothesis the rollback had already disproven. We brought in a fresh database engineer. Within 20 minutes she identified a runaway query in an unrelated batch job.
> **R:** Mitigated within 30 minutes of refocus. The post-mortem action included an explicit "fact restate every 30 minutes" item for the IC role. The lesson: the IC's job to break tunnel vision is not optional.

### Q10. How do you handle a responder who is becoming unreliable due to fatigue or stress during a long incident?

**Answer:**

Long incidents (4+ hours) wear responders down. A senior engineer making mistakes due to fatigue is a risk to the response and to themselves. Managing this is the IC's responsibility.

**The signs:**

- Repeating actions they just performed.
- Long pauses before responding to questions.
- Snapping at colleagues.
- Making basic typos in commands they've run a hundred times.
- Flat affect, withdrawal from the channel.
- Insisting on staying when their judgement is visibly degraded.

**The interventions, in order:**

1. **Suggest a break.** "Take 15. Get water. Come back fresh." Often this is enough.
2. **Force a break.** "I'm taking you off active execution for 30 minutes. X is taking over your stream."
3. **Rotate them out.** "Hand off to Y. Get sleep. We'll page you only if we genuinely need you."
4. **Send them off-shift.** "You're done for tonight. Come back at 9am with fresh eyes."

**The framing matters:**

- Frame as care, not criticism. "I want you sharp for the post-mortem tomorrow" lands better than "you're making mistakes."
- Validate the contribution. "You've been the one driving this for 4 hours; that's why we need to rotate you."
- Take responsibility yourself. "I as IC am calling this; not your call."

**Plan for it in advance:**

- For SEV1s expected to last more than 4 hours, plan rotations from the start. Two ICs, two tech leads, with handoff at agreed intervals.
- Have a "shadow IC" for long incidents — they pick up if the IC needs to step out.
- Keep a roster of off-call responders who can be paged for relief.

**The cultural piece:**

In some teams, leaving an incident before resolution is seen as weakness. This is wrong and dangerous. Senior engineers who model good handoff behaviour ("I'm tagging out at the 4-hour mark; here's the brief for the next person") establish the norm.

**Reference:** Aviation Crew Resource Management (CRM) literature on fatigue management is the academic anchor. The FAA's published material on cockpit fatigue is freely available. Google SRE on long-incident protocols.

### Q11. What is a "two-pizza" incident bridge and why does it work?

**Answer:**

A **"two-pizza"** incident bridge is the principle (borrowed from Bezos's two-pizza team rule) that an active incident response should have only as many people in the room as can be coordinated effectively — typically 5 to 8.

**Why bigger isn't better:**

- **Coordination overhead grows quadratically.** A 20-person Slack channel during an incident becomes a flood of cross-talk that drowns the actual work.
- **Diffusion of responsibility.** "Someone else is on it" → no one is on it.
- **Confused command.** Multiple voices issuing instructions, conflicting hypotheses, no clear lead.
- **Decision paralysis.** More opinions, slower decisions.

**The roles, kept tight:**

- **Incident Commander.** One.
- **Tech lead / SME.** One per affected system.
- **Comms lead.** One.
- **Scribe.** One (often combined with comms).
- **Observers** — *separate channel, not the active bridge.*

**Total active responders: 5–8 typically. Larger only when the incident genuinely spans many systems.**

**Managing the overflow:**

- **Spectator channel.** Stakeholders, curious engineers, leadership — they get a separate channel with periodic updates from the comms lead. They don't post in the bridge.
- **"Step out unless asked"** rule. If you're not actively contributing, leave. Be on standby.
- **Standing rule for joiners.** "When you join, mute. Wait for the IC to assign you something. Don't ask 'what's happening?' — read scrollback."

**The IC's gatekeeping role:**

- Pull people in deliberately. "I need a database engineer. Page X." Not "everyone come help."
- Push people out gently. "Y, thanks for the input — we've got the auth angle covered. You can drop."
- Be ruthless about scope. The incident channel is for *resolving the incident*. Not for the post-mortem, the curious questions, the "is this related to..." speculation.

**Anti-pattern — the open bridge:**

In some companies, every SEV1 turns into a 50-person Zoom call where 45 people are listening and 5 are working. This is theatrical, not effective. The IC should have authority to send observers to a separate channel.

**Reference:** Bezos's two-pizza team principle (Amazon). PagerDuty's incident response process publicly documents this pattern. Google SRE book on managing incidents.

### Q12. What is a "near-miss" and why should they be reviewed?

**Answer:**

A **near-miss** (sometimes "near-hit" — terminology varies) is an event that *could have caused* an incident but didn't, due to luck, redundancy, or quick action. The deploy that nearly broke production but was caught in canary. The data corruption that affected internal tools but not customer-facing ones. The pager that almost went unanswered.

**Why near-misses matter:**

- **Free learning.** The cost of investigation is low; the cost of the incident-that-didn't-happen would be high.
- **Same causes, no impact.** A near-miss has the same structural causes as the equivalent real incident. Fixing the cause prevents both.
- **Heinrich's Law.** From workplace safety: for every major incident there are many minor incidents and many more near-misses. Acting on near-misses prevents major incidents.

**Why they're often ignored:**

- **No urgency.** Nothing actually went wrong, so the post-mortem feels theatrical.
- **No customer impact data.** Nothing to anchor the severity.
- **No reporting.** Engineers don't report near-misses because there's no incentive.

**How to make near-miss review work:**

1. **Make reporting safe and easy.** A simple form. Anonymous if needed. No process penalty for reporting.
2. **Treat them as small incidents.** Same lightweight post-mortem template. Same action-tracking.
3. **Celebrate the catch.** "X spotted Y in canary — that prevented a SEV1." Make the spotter visible.
4. **Track the rate.** Near-miss reporting going up is a *good* sign — not a problem. It means engineers feel safe reporting.
5. **Tie to chaos-engineering practice.** Game days deliberately create near-misses; review them as you'd review a real one.

**Anti-pattern:**

"It worked out fine" thinking. Engineers (and managers) who treat narrow escapes as proof the system works are setting themselves up for the eventual incident where luck doesn't hold.

**Reference:** *Just Culture* (Sidney Dekker) on near-miss reporting in safety-critical industries. Allspaw on near-misses in software. The aviation industry's NASA Aviation Safety Reporting System (ASRS) is the model — free, anonymous, learning-focused.

---

## Advanced

### Q13. How do you build incident-response capability in a team that doesn't have it yet?

**Answer:**

Many teams discover they need incident response only after their first painful incident. Building the capability deliberately — before you need it — is a senior-engineer responsibility.

**The progression:**

**Stage 1 — Define the basics (week 1–2).**

- A definition of "what's an incident" with severity criteria.
- A single, agreed-upon channel for incident comms.
- A rotation of who's the IC. (Even if it's only two people.)
- A simple incident template (Slack canvas / wiki page).
- A "first 15 minutes" runbook — what does the on-call do when paged?

**Stage 2 — Practice (month 1–3).**

- Walk through past incidents using the new format. Use real incidents from history; rewrite them in the new template.
- Run a tabletop exercise. Pick a hypothetical scenario; the IC and team talk through how they'd respond.
- Run a low-stakes game day. Inject a controlled failure in staging; let the team respond.
- Train backup ICs. Single-person dependencies are a bus factor.

**Stage 3 — Mature (month 3–12).**

- Establish the post-incident review process. Schedule them within 5 business days. Invite broadly.
- Track action items to completion. Action items that don't get done are worse than no action items.
- Build runbooks from PIR action items.
- Establish exec comms cadences.
- Add severity-criteria refinement based on real incidents.

**Stage 4 — Scale (year 1+).**

- Cross-team IC rotation; ICs who aren't from the affected team.
- Chaos engineering programme.
- SLO-driven alerting (alert on what customers care about, not on every metric).
- Formal IC training; formal blameless culture training.
- Quarterly incident review meta-analysis. What patterns are recurring?

**Cultural moves:**

- **Sponsor the on-call role.** It's high-stress, high-value work. Compensate it (literally — on-call pay or comp time).
- **Public learning.** Read post-mortems in team meetings. Make incident-handling a visible craft.
- **Senior IC rotation.** When senior engineers and leaders rotate as ICs, the message "this work is important" becomes credible.

**Reference:** *Site Reliability Engineering* (Beyer et al.) — the chapters on incident management and emergency response. PagerDuty's open-source Incident Response documentation. *Seeking SRE* (Blank-Edelman) for organisational pattern variations.

### Q14. How do you write a post-mortem that's read and acted on, not filed and forgotten?

**Answer:**

The dirty secret of post-mortems: most are written, filed in a folder, and read by no one. The senior-engineer skill is writing PIRs that produce real organisational change.

**The structural moves:**

1. **Write for the reader, not the writer.** The reader is an engineer in 6 months who wasn't there. They need context, not just facts.

2. **Lead with the customer impact.** Engineers care about the technical cause. Leaders, customers, and most readers care about the impact. Put it first.

3. **Use a timeline.** Time-of-day stamps for key events. The reader reconstructs the experience; this builds empathy and surfaces patterns ("we took 35 minutes to identify the cause; how do we cut that?").

4. **Multiple contributing factors, not "the" cause.** Force the analysis to surface 3+ contributing factors. Single-cause PIRs are usually wrong.

5. **Action items with owners and deadlines.** Vague action items ("improve monitoring") get ignored. Specific items ("Y will add an alert on metric Z by date W") get done.

6. **Track action items in a single place.** A spreadsheet, a Jira board, a dashboard. Visible. Reviewed monthly. Engineers learn that PIR commitments are real.

**The cultural moves:**

1. **Read PIRs in team meetings.** Weekly or bi-weekly, walk through one. Discuss. Spread the lessons.

2. **Cite PIRs in code review and design review.** "We had this pattern fail in incident X; let's not repeat it."

3. **Senior engineers attend other teams' PIRs.** Cross-team learning is the highest-leverage form.

4. **Run quarterly meta-analyses.** Patterns across multiple incidents reveal organisational themes (e.g., "5 of last quarter's SEV1s were deploy-related"). These drive larger investments.

5. **Celebrate the action items getting done.** Visible reinforcement.

**Anti-patterns:**

- **PIR as ritual.** Written because the process requires it; filed because no one reads it.
- **Sanitised PIRs.** When PIRs are read by leadership and language is softened to protect feelings, the learning is lost. (See blameless culture.)
- **Action item theatre.** Items added because the process requires them, no intent to complete.
- **PIR for the IC's CV.** Written to make the responder look good, not to learn.

**STAR framing:**

> **S:** Our team had been writing PIRs for 18 months. Action items had a 30% completion rate. Engineers had stopped reading them.
> **T:** As tech lead I was asked to revive the practice without adding overhead.
> **A:** I made three changes: action items moved to a single dashboard with monthly review; we replaced "root cause" with "contributing factors" (typically 3-4 per PIR); we added a 15-minute slot in the bi-weekly team meeting to walk through one PIR. I also wrote a meta-analysis of the past 12 months — patterns across incidents — and presented it to the org.
> **R:** Action item completion went to 82% within two quarters. The meta-analysis surfaced a deploy-process gap that, when fixed, prevented 3 likely future incidents (matched against the historical cause distribution). PIRs became part of the team's identity.

### Q15. How do you handle an incident caused by a vendor or external dependency?

**Answer:**

When the cause is outside your code, the response shape changes — but your accountability to your customers does not.

**During the incident:**

1. **Confirm the dependency is the cause.** Don't blame a vendor before you have evidence. "The vendor's status page is yellow; our metrics show calls to them timing out at 80%; we are degraded because of this."

2. **Mitigate what you can.** Failover, fallback, degraded mode, retries, queue-and-replay. Even if the vendor is down, you may be able to reduce customer impact.

3. **Escalate to the vendor properly.** Use their formal channels (support ticket, named-account-manager phone, status page subscription). Slack-DM'ing your contact is not formal escalation.

4. **Communicate with customers using your name.** Don't deflect to the vendor publicly. "AWS is down" is true but to your customers, *you* are down. Take ownership of the customer experience while privately escalating to the vendor.

5. **Capture timing and evidence.** When was the vendor degraded? When did your monitoring detect? When did they acknowledge? You'll need this for post-incident review and possibly contractual claims.

**During the post-mortem:**

1. **Write the PIR as if the vendor's failure was foreseeable** — because in some sense it was. Vendor failures are expected; your system's response to them is your responsibility.

2. **Examine your dependency design.** Did you have appropriate timeouts? Circuit breakers? Fallbacks? Degraded modes? Could you have routed around the failure?

3. **Examine the contract.** Did the vendor's behaviour violate SLAs? Are you due credit? Should you renegotiate?

4. **Examine the dependency choice.** Is this vendor still the right one? Is single-vendor dependency a risk? Should you multi-source?

5. **Hold the vendor accountable.** Request their post-mortem; verify they're addressing the cause. If they're not, this informs your future relationship with them.

**The communication trap:**

When customers complain, the temptation is to say "it's the vendor's fault." This is correct but unhelpful — and it externalises ownership. The better stance: "We're degraded because of an issue in [system]. We've taken these mitigation steps. We're working with the upstream team to resolve. Updates every X minutes."

**STAR framing:**

> **S:** Our payment processor went down for 3 hours. 100% of credit-card transactions failed. Our status page showed red.
> **T:** As IC I had to manage both a customer-facing incident and a vendor escalation in parallel.
> **A:** I assigned the comms lead to handle vendor escalation while the tech lead and I focused on mitigation. We turned on bank-transfer fallback (which we'd built but never used in earnest — that's another story). We sent customer comms taking ownership of the impact without naming the vendor publicly. I personally called the vendor's named-account-manager. The PIR identified that we'd had no fallback test in 9 months and that our circuit breakers had timeout values too high to fail fast.
> **R:** Mitigated to 70% of normal capacity within 35 minutes via fallback. Full recovery when vendor restored. PIR action items: monthly fallback drill, review all third-party timeout values, multi-vendor strategy for payment processing within 12 months. The multi-vendor work prevented two later incidents from being SEV1.

### Q16. How do you balance the cost of incident process with team velocity?

**Answer:**

Incident process — runbooks, ICs, post-mortems, action items, training — has real cost. Heavy process can crush a small team's velocity. No process at all means every incident is amateur hour. The senior-engineer judgement is calibrating the weight.

**The cost-benefit framing:**

- **Process pays off when incidents are frequent or severe.** A team with weekly SEV2s benefits from heavy process. A team with one minor incident a year does not.
- **Process pays off when teams scale.** Five-person teams can run incidents informally. Fifty-person teams cannot.
- **Process pays off when consequences are high.** Regulated industries, customer-facing payment systems, healthcare — process is non-negotiable. Internal tools may need much less.

**Calibration heuristics:**

| Team / system shape | Suggested process weight |
|---------------------|--------------------------|
| Small team, internal-only | Lightweight: severity criteria, post-mortem template, no IC role |
| Small team, customer-facing | Add: named IC, 15-min post-mortem, action tracking |
| Mid-size team, business-critical | Add: rotation of IC, comms lead, formal exec comms, runbooks |
| Large org, regulated | Add: full ICS-style structure, mandated PIRs, retention policies, drills |

**Anti-patterns from over-investment:**

- **Process for theatre.** PIRs that no one reads. Action items that get filed and forgotten. The process exists to look mature; it doesn't produce learning.
- **IC as career path.** When IC becomes a status role rather than a needed one, you have too much process.
- **Heavy templates that go unfilled.** If the post-mortem template has 40 fields, engineers will fill it badly or not at all. Fewer required fields = better completion.
- **Monthly incident review meetings with no purpose.** A meeting with no clear input or output that runs out of inertia.

**Anti-patterns from under-investment:**

- **No IC role.** Every incident is a free-for-all.
- **No PIR.** You incur the same incidents repeatedly because nothing's learned.
- **No severity criteria.** Every incident is "we need to look at this" — no clear escalation.
- **No runbooks.** Every on-call reinvents from scratch.

**Senior-engineer move:**

Run the process as light as possible while still producing the outcomes — learning, prevention, customer protection. Inspect quarterly: are we generating learning? Are action items getting done? Are incidents recurring with the same causes? Adjust accordingly.

**Reference:** *The Phoenix Project* and *The DevOps Handbook* (Kim et al.) on the relationship between operational maturity and velocity. *Accelerate* (Forsgren, Humble, Kim) on incident MTTR as a DORA metric — its correlation with overall organisational performance.

### Q17. How would you respond if your post-mortem revealed that a senior leader's decision was a major contributing factor?

**Answer:**

This is one of the hardest tests of blameless culture in practice. The post-mortem reveals that, say, a leadership-imposed deadline forced a deploy that should have been delayed; or a leadership decision to underinvest in monitoring set up the missed alert. The temptation is to soften the language. Doing so destroys the post-mortem.

**The principles:**

1. **Blameless does not mean nameless.** Specific decisions can and should be described. "The Q3 deadline commitment to the customer required a deploy that bypassed normal canary procedure" is factual; it's not blame.

2. **Address systems, not motives.** "The deadline pressure created an incentive structure where canary procedures appeared unaffordable" — this surfaces the real issue without indicting the leader's character.

3. **Invite the leader to the PIR.** Ideally before publication. They can correct factual errors, add context you didn't have, and (often) become a sponsor for the action items.

4. **Tie to a system change.** "We need a process where deadline pressure of this kind is escalated rather than absorbed into engineering risk-taking." This is a fixable thing.

5. **Don't soften under pressure.** If the leader's reaction is "remove that finding," explain why you can't. The PIR's value depends on its honesty. A leader who insists on dishonesty is the underlying organisational problem; the PIR has surfaced it.

**The trade-off you may face:**

If you're junior and the leader is senior, surfacing this can have career consequences. The senior-engineer move is to surface it anyway, in the safest available framing, with the support of your manager and other leaders. If the org genuinely punishes honest PIRs, you have a bigger problem than the incident — and you're learning something important about whether to stay.

**Practical phrasing in PIRs:**

- "The decision to commit to date X without engineering scoping reduced safety margin in the deploy process."
- "Investment in observability had been deferred 3 quarters due to roadmap pressure; this contributed to the late detection."
- "The on-call's decision to proceed without runbook backing was consistent with team norms; the absence of a runbook is the contributing factor."

**STAR framing:**

> **S:** A SEV1 outage was rooted in a deploy that had bypassed canary because of a hard date commitment a VP had made to a customer.
> **T:** As IC running the post-mortem I had to surface this without making the VP defensive.
> **A:** I drafted the PIR with the contributing factor framed as "deadline pressure incentivised canary bypass." I shared the draft with the VP one-on-one before the broader review. I framed it as "I want your input on whether we have the system change right." She added context, agreed with the framing, and personally took the action item to revisit how customer commitments are scoped going forward.
> **R:** The PIR was published unchanged. The VP became an internal advocate for engineering capacity protection. Two later quarters had visibly more conservative date-setting. The lesson: invite the leader into the PIR as a partner, not as a defendant. Most leaders rise to the framing; the few who don't have revealed something important about themselves.

**Reference:** Sidney Dekker, *Just Culture: Restoring Trust and Accountability in Your Organisation*. Allspaw's writing on blameless post-mortems addresses leadership findings explicitly.
