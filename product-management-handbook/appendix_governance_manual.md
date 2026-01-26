# Governance Operating Manual (Appendix)

## Purpose
This manual defines the minimum governance loop for a product portfolio using Jira. The goal is to make decisions fast without creating theatre.

## Cadence
- Weekly portfolio review (30–45 min)
- Monthly product review (60–90 min)
- Quarterly planning (half-day)

## Weekly portfolio review
**Inputs**
- Top 10 Initiatives by risk or impact
- Blocked dependencies
- Critical path items
- Delivery metrics (cycle time, WIP, throughput)

**Decisions**
- Escalate or resolve blocked dependencies
- Adjust target releases when capacity shifts
- Reprioritize when risk exceeds tolerance

**Outputs (record in Jira)**
- Updated Initiative status
- Updated dependency owners
- New or removed risks

## Monthly product review
**Inputs**
- Outcome metrics by Initiative
- OKR status and evidence
- Release outcomes and learnings

**Decisions**
- Continue, pivot, or stop initiatives
- Allocate or remove capacity
- Approve next experiments

**Outputs (record in Jira)**
- OKR status updates
- New experiments and owners
- Updated confidence and impact fields

## Quarterly planning
**Inputs**
- Portfolio capacity and throughput
- Strategic goals and constraints
- Major dependencies and risks

**Decisions**
- Select top initiatives and funding levels
- Confirm release windows and delivery horizons
- Set guardrails for scope changes

**Outputs (record in Jira)**
- Initiative priority order
- Target releases
- Dependency map

## Escalation rules
- Any S1 incident or risk of data loss escalates immediately.
- Any dependency blocked > 7 days goes to weekly review.
- Any Initiative off track for 2 consecutive reviews triggers a reset plan.

## Anti-theatre rules
- No status-only updates. Every update must include a decision or a metric change.
- No new fields without a usage rule and owner.
- No duplicate roadmaps; Jira is the source.
