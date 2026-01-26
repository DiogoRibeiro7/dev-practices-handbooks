# Product + Project Management in Jira: Book Plan

## Target readers
- Product managers (PMs)
- Product owners (POs)
- Technical program/project managers (TPMs/TPjMs)
- Engineering managers (EMs)
- Founders and operators at growing startups
- Product analysts and data analysts supporting delivery

## Prerequisites
- Basic familiarity with agile concepts (backlog, sprint, release, MVP)
- Access to Jira Software (Cloud or Data Center) or a sandbox instance
- Ability to edit Jira projects (admin or delegated admin) for hands-on setups
- A real or sample product area to model (feature, workflow, team)

## Core promise
By the end of this book you will be able to design a practical delivery system in Jira that connects strategy to outcomes, runs predictable execution, and scales governance without suffocating teams. You will know how to configure projects, workflows, fields, dashboards, and reports to support discovery, delivery, and decision-making with clear operating rules.

## Chapter template (consistent structure)
- Motivation
- Concepts
- Jira implementation steps
- Worked example (screens + configs described textually)
- Checklists
- Exercises
- Common failure modes

## Style constraints
- Minimal fluff: every section must teach or decide.
- Use short sentences mixed with occasional longer ones for emphasis.
- Provide strong decision rules and defaults.
- Avoid vendor marketing tone; describe Jira as a tool, not a solution.
- Prefer concrete examples, field names, and query snippets over theory.

---

# Table of Contents

## Part I — Foundations

### 1. Why Jira, and what it should *not* do
Learning objectives
- Distinguish project tracking from product strategy.
- Identify the minimum Jira footprint for a team.
- Avoid over-configuration that slows delivery.
- Define the boundaries between Jira and product docs.
- Choose when *not* to log work in Jira.
- Set expectations with stakeholders on what Jira will show.

What you will build
- A “Jira boundary map” that clarifies what belongs in Jira vs elsewhere.

### 2. Jira data model for product work
Learning objectives
- Explain issues, issue types, workflows, statuses, and resolutions.
- Map product artifacts (epics, outcomes, hypotheses) to issue types.
- Understand custom fields, field contexts, and screens.
- Identify when to use components vs labels.
- Design a minimal issue type scheme.
- Prevent field bloat with governance rules.

What you will build
- A lightweight issue type taxonomy with agreed ownership and usage rules.

### 3. Team and project setup that scales
Learning objectives
- Choose company-managed vs team-managed projects.
- Set permission schemes that enable collaboration without chaos.
- Configure roles for PM, PO, EM, analyst, and QA.
- Establish default workflows for product vs delivery work.
- Decide when to split or merge projects.

What you will build
- A reusable project setup checklist and baseline workflow.

---

## Part II — Delivery operating system

### 4. From roadmap to executable plan
Learning objectives
- Translate roadmap items into epics and deliverable slices.
- Define acceptance criteria that enable predictable delivery.
- Link epics to releases and milestones.
- Use Jira versions/releases for external commitments.
- Set planning horizons and review cadences.
- Establish the “definition of ready” for intake.

What you will build
- A roadmap-to-epic mapping template with release alignment.

### 5. Backlog intake and prioritization
Learning objectives
- Build a single intake path without silos.
- Choose a prioritization framework (RICE/WSJF/ICE) and encode it.
- Create scoring fields and guardrails in Jira.
- Avoid priority inflation with decision rules.
- Design a triage workflow that respects SLAs.
- Align backlog ordering to outcomes, not opinions.

What you will build
- A scored backlog with a transparent triage workflow.

### 6. Sprint and flow execution
Learning objectives
- Choose between Scrum and Kanban and configure accordingly.
- Define WIP limits and enforce them with board rules.
- Set sprint metrics that predict delivery health.
- Handle spillover and scope changes without eroding trust.
- Run ceremonies with Jira data (planning, review, retro).
- Keep work items small enough to flow.

What you will build
- A delivery board with WIP rules, SLAs, and sprint metrics.

### 7. Dependencies, risks, and impediments
Learning objectives
- Track cross-team dependencies without turning Jira into a Gantt tool.
- Model risks and impediments with explicit issue types or labels.
- Create dependency review cadences and escalation paths.
- Use blockers and linked issues for transparency.
- Prevent dependency sprawl with explicit exit criteria.

What you will build
- A dependency tracking view and risk register embedded in Jira.

### 8. Reporting that drives decisions
Learning objectives
- Define delivery KPIs (cycle time, throughput, predictability).
- Build dashboards for execs vs teams with different time horizons.
- Use filters and JQL to create trustworthy reports.
- Avoid vanity metrics and misleading burndowns.
- Create a weekly operating review with a fixed template.

What you will build
- A decision-oriented dashboard pack with role-based views.

---

## Part III — Product discovery + outcomes

### 9. Outcome-driven roadmaps in Jira
Learning objectives
- Encode outcomes as first-class Jira objects (epics or initiatives).
- Link hypotheses to experiments and delivery slices.
- Define outcome success metrics and owners.
- Keep roadmap changes auditable.
- Review outcomes on a cadence independent of sprints.

What you will build
- An outcome-to-initiative structure with metrics in Jira.

### 10. Experiment tracking and learning loops
Learning objectives
- Translate discovery work into Jira issues without bloating the backlog.
- Track experiments, assumptions, and results in a structured way.
- Define what “learning done” means.
- Decide when to kill or scale a hypothesis.
- Use Jira automation to capture experiment status changes.

What you will build
- An experiment log with clear entry/exit criteria.

### 11. Customer insights and feedback routing
Learning objectives
- Create a feedback intake and tagging system.
- Connect support data to product priorities.
- Aggregate feedback by segment and severity.
- Avoid drowning teams in raw tickets.
- Build a lightweight evidence bundle for decisions.

What you will build
- A feedback triage flow and insight dashboard.

### 12. Outcome measurement and post-release learning
Learning objectives
- Define outcome metrics and instrumentation requirements.
- Connect releases to analytics events and dashboards.
- Set post-release review windows and owner responsibilities.
- Decide whether the outcome was achieved and what to do next.
- Capture learnings in Jira for reuse.

What you will build
- A post-release outcome review template with metrics.

---

## Part IV — Governance + scale

### 13. Portfolio governance without dead weight
Learning objectives
- Define portfolio tiers and decision rights.
- Establish intake thresholds for executive review.
- Use Jira plans/advanced roadmaps responsibly.
- Keep approvals fast with explicit criteria.
- Balance governance with team autonomy.

What you will build
- A portfolio intake policy and review cadence.

### 14. Standards, guardrails, and Jira administration
Learning objectives
- Create standard workflows, fields, and screens across teams.
- Decide which customizations require review.
- Build a change-control process for Jira configuration.
- Monitor field usage and clean up obsolete data.
- Document system ownership and escalation paths.

What you will build
- A Jira governance playbook with configuration policies.

### 15. Scaling teams and programs
Learning objectives
- Organize multiple teams across a product area.
- Model programs and cross-team initiatives in Jira.
- Handle resource constraints and staffing changes.
- Coordinate release trains without chaos.
- Design portfolio-level reporting.

What you will build
- A multi-team program board and reporting layer.

### 16. Operating reviews and continuous improvement
Learning objectives
- Build a monthly operating review agenda using Jira data.
- Measure process health and bottlenecks over time.
- Introduce improvement experiments with owners and due dates.
- Close the loop between governance and delivery outcomes.
- Keep the system lightweight as it scales.

What you will build
- A continuous improvement cadence with Jira-backed metrics.
