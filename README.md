# Technical Leadership — Interview Preparation

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Subject: Technical Leadership](https://img.shields.io/badge/Subject-Technical%20Leadership-blue)](https://en.wikipedia.org/wiki/Software_engineering)

## Overview

This repository provides interview preparation material for Senior, Staff, and Principal Software Engineer roles that require demonstrated technical leadership. The content covers the "soft skills and craft" side of the interview loop — behavioural questions, technical-judgement scenarios, code review practice, architecture decision-making, mentorship, and incident leadership.

The material targets engineers interviewing at companies that evaluate leadership behaviours alongside coding ability — senior tracks at large technology firms, staff+ roles where scope extends beyond a single team, and any role where the loop includes a "leadership" or "behavioural" round. Questions are answered using the STAR format (Situation, Task, Action, Result) where appropriate, with references to widely-known books and frameworks such as *The Staff Engineer's Path*, *An Elegant Puzzle*, *Accelerate*, and DORA metrics.

## Table of Contents

- [01 Code Review and Craftsmanship](#01-code-review-and-craftsmanship)
- [02 Technical Decision-Making](#02-technical-decision-making)
- [03 Mentorship and Collaboration](#03-mentorship-and-collaboration)
- [04 Engineering Practices and Behavioural](#04-engineering-practices-and-behavioural)
- [05 Quizzes](#05-quizzes)
- [How to Use](#how-to-use)
- [Related Repositories](#related-repositories)
- [Contributing](#contributing)
- [License](#license)

### 01 Code Review and Craftsmanship

The daily practices that distinguish senior engineers — reviewing code effectively, improving legacy systems, and raising the quality bar without alienating the team.

- `code_review_practices.md` — Effective code reviews, review checklists, giving/receiving feedback, reviewer bias
- `refactoring_and_legacy_code.md` — Safe refactoring, strangler fig pattern, characterisation tests, incremental migration

### 02 Technical Decision-Making

How senior engineers make, document, and communicate technical decisions with long-term impact.

- `architecture_decisions.md` — ADRs, RFC process, evaluating trade-offs, build vs buy, technology selection
- `technical_debt.md` — Identifying debt, prioritisation, debt quadrant, communicating with stakeholders
- `estimation_and_planning.md` — Breaking down work, estimation techniques, handling uncertainty, scope negotiation

### 03 Mentorship and Collaboration

Scaling your impact through others — mentoring, cross-team influence, and working effectively with stakeholders.

- `mentorship.md` — 1:1s, growth frameworks, pairing, onboarding, sponsorship vs mentorship
- `cross_team_collaboration.md` — Working across teams, stakeholder management, influence without authority, disagreement

### 04 Engineering Practices and Behavioural

The classic behavioural interview topics — how you lead incidents, communicate in writing, and tell impactful STAR stories.

- `incident_leadership.md` — Leading incidents, blameless culture, communicating during outages, executive updates
- `writing_and_communication.md` — Design docs, RFCs, technical writing, public speaking, async communication
- `behavioural_scenarios.md` — STAR format scenarios, leadership principles, conflict resolution, impact stories

### 05 Quizzes

Self-assessment quizzes covering each major topic area.

- `quiz_craftsmanship_and_decisions.md` — Code review, refactoring, architecture decisions, technical debt, estimation
- `quiz_mentorship_and_practices.md` — Mentorship, collaboration, incident leadership, writing, behavioural scenarios

## How to Use

This repository is structured as a progressive technical leadership interview course:

1. **Start with craftsmanship.** Code review and refactoring questions are frequent for senior candidates. They reveal how you raise quality and navigate legacy code without breaking things.

2. **Move to decision-making.** ADRs, technical debt, and estimation are staple topics in staff-level loops. Know how to frame trade-offs, document decisions, and communicate uncertainty.

3. **Study mentorship and collaboration.** Senior and staff roles are scored heavily on "scope of influence." These sections cover 1:1s, sponsorship, and working across organisational boundaries.

4. **Learn incident leadership and writing.** The incident round is common at operationally-mature companies. Design doc and RFC questions are near-universal. Blameless culture is a trigger phrase interviewers listen for.

5. **Prepare behavioural stories.** Work through `behavioural_scenarios.md` and prepare 6–10 STAR stories covering conflict, failure, ambiguity, and impact. Reuse the same stories across multiple question types.

6. **Use the quizzes** to identify weak areas and return to the relevant section.

## Related Repositories

- **[Interview_System_Design](https://github.com/BrendanJamesLynskey/Interview_System_Design)** — System design interview preparation
- **[Interview_Software_Testing](https://github.com/BrendanJamesLynskey/Interview_Software_Testing)** — Testing strategy and quality engineering
- **[Interview_Design_Patterns](https://github.com/BrendanJamesLynskey/Interview_Design_Patterns)** — Design patterns and SOLID principles
- **[Interview_Observability_SRE](https://github.com/BrendanJamesLynskey/Interview_Observability_SRE)** — Observability, SRE, and production readiness

## Contributing

Contributions are welcome. Please ensure:

1. Answers are grounded in real engineering scenarios, not platitudes
2. STAR answers are specific — concrete situations, concrete actions, measurable results
3. Trade-offs are presented honestly — leadership advice is context-dependent
4. Quiz questions reflect realistic interview scenarios

For significant additions, please open an issue first to discuss scope and approach.

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
