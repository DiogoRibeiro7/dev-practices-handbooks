# Dashboards Pack

Eight dashboards designed for different audiences. Each includes suggested gadgets, queries, and how to interpret the signals.

---

## 1) Team Flow Dashboard
**Audience:** Delivery team (engineers + EM)

**Gadgets / charts**
- Sprint Burndown (Scrum) or Control Chart (Kanban)
- Filter Results: In Progress items
- Two-Dimensional Filter Statistics: Status x Assignee
- Created vs Resolved (last 30 days)

**Interpretation guide**
- If cycle time rises, reduce WIP and shrink story size.
- If Created >> Resolved, pause intake and rebalance.
- If a single assignee column dominates, redistribute work.

---

## 2) Scrum Execution Dashboard
**Audience:** Scrum teams and PO

**Gadgets / charts**
- Sprint Burndown
- Sprint Health gadget
- Filter Results: Added after sprint start
- Filter Results: Unestimated in sprint

**Interpretation guide**
- If scope creep appears, renegotiate sprint goal immediately.
- If unestimated items exist, pause new work until estimated.

---

## 3) Kanban Flow Dashboard
**Audience:** Kanban teams

**Gadgets / charts**
- Cumulative Flow Diagram
- Control Chart
- Filter Results: Aging work (In Progress > 7 days)
- Filter Results: Review bottleneck

**Interpretation guide**
- If WIP trends up, tighten limits and stop starting.
- If aging work spikes, swarm on the oldest items.

---

## 4) Incident Command Dashboard
**Audience:** TPM/EM on-call, incident leads

**Gadgets / charts**
- Filter Results: Active S1/S2 incidents
- Filter Results: Bugs past SLA
- Pie Chart: Severity distribution
- Filter Results: Missing postmortem

**Interpretation guide**
- If S1/S2 counts > 0, enter incident mode.
- If postmortems are missing, block new work until closed.

---

## 5) Portfolio Risk Dashboard
**Audience:** PM leadership, TPM

**Gadgets / charts**
- Filter Results: High-risk epics
- Filter Results: Blocked dependencies
- Two-Dimensional Filter Statistics: Risk x Target Release
- Filter Results: Initiatives At Risk

**Interpretation guide**
- If high-risk epics cluster in one release, replan scope.
- If dependencies pile up, escalate ownership immediately.

---

## 6) Roadmap Readiness Dashboard
**Audience:** PM, PO

**Gadgets / charts**
- Filter Results: Epics missing Target Release
- Filter Results: Epics missing Risk
- Filter Results: Initiatives missing OKR link
- Filter Results: Opportunities stuck in Discovery

**Interpretation guide**
- If readiness gaps persist, stop planning and fix data quality.
- If discovery is stalled, reduce batch size and time-box.

---

## 7) OKR Outcomes Dashboard
**Audience:** PM, execs

**Gadgets / charts**
- Filter Results: OKR-linked Initiatives
- Filter Results: OKR status At Risk
- Filter Results: Initiatives missing metrics
- Created vs Resolved (OKR items)

**Interpretation guide**
- If At Risk grows, revisit hypotheses and allocate resources.
- If metrics are missing, pause outcome reporting until fixed.

---

## 8) Exec Portfolio Overview
**Audience:** Execs

**Gadgets / charts**
- Filter Results: Top Initiatives (by Impact)
- Pie Chart: Initiative status distribution
- Created vs Resolved (portfolio)
- Filter Results: Escaped defects (last 30 days)

**Interpretation guide**
- If Escaped defects rise, slow roadmap commitments.
- If Initiatives cluster in Discovery, accelerate decision-making or kill low-confidence bets.
