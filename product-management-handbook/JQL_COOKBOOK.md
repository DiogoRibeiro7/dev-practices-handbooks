# JQL Cookbook (Copy/Paste Ready)

All queries assume a project key of `PMH`. Replace with your project key. For custom fields, use the exact field name from Jira (e.g., "Customer Impact").

---

## Triage (6 queries)

### 1) New intake items
**JQL:** `project = PMH AND status = Intake ORDER BY created DESC`
- **Use:** Triage new requests quickly.
- **Variations:** Add `AND issuetype = Opportunity` for discovery-only intake.
- **Pitfalls:** If Intake is not a status in your workflow, use the correct initial status.

### 2) Intake older than 7 days
**JQL:** `project = PMH AND status = Intake AND created <= -7d`
- **Use:** Prevent backlog rot.
- **Variations:** Use `-3d` for higher SLAs.
- **Pitfalls:** Created date does not reflect last activity; consider `updated` if needed.

### 3) Intake missing owner
**JQL:** `project = PMH AND status = Intake AND assignee is EMPTY`
- **Use:** Ensure every intake item has accountability.
- **Variations:** Filter by component/label for ownership routing.
- **Pitfalls:** If you don’t use assignees for triage, change to `reporter` or custom owner field.

### 4) Intake from sales
**JQL:** `project = PMH AND status = Intake AND labels = sales`
- **Use:** Separate commercial asks from other sources.
- **Variations:** Replace label with `"Source" = Sales` if you use a field.
- **Pitfalls:** Labels drift; enforce consistent label usage.

### 5) Triage decisions pending
**JQL:** `project = PMH AND status = Triage AND updated <= -3d`
- **Use:** Items stuck in triage.
- **Variations:** Add `AND priority in (High, Highest)` to escalate.
- **Pitfalls:** Updates may be automated; verify true staleness.

### 6) Intake duplicates
**JQL:** `project = PMH AND status = Intake AND issueLinkType = duplicates`
- **Use:** Collapse duplicate requests.
- **Variations:** Use `linkedIssues` JQL function if installed.
- **Pitfalls:** Requires consistent duplicate linking practices.

---

## Sprint Execution (6 queries)

### 7) Current sprint scope
**JQL:** `project = PMH AND sprint in openSprints() ORDER BY priority DESC`
- **Use:** See what the team committed to.
- **Variations:** Add `AND status != Done` for remaining scope.
- **Pitfalls:** Items added mid-sprint will appear; track scope changes separately.

### 8) Added after sprint start
**JQL:** `project = PMH AND sprint in openSprints() AND createdDate >= startOfSprint()`
- **Use:** Identify scope creep.
- **Variations:** Add `AND status != Done` to focus on inflight creep.
- **Pitfalls:** startOfSprint() requires Scrum board context.

### 9) Ready but unstarted
**JQL:** `project = PMH AND sprint in openSprints() AND status = Ready`
- **Use:** Spot work pulled but not started.
- **Variations:** Add `AND assignee is EMPTY` to flag ownership gaps.
- **Pitfalls:** If you skip a Ready state, use your equivalent.

### 10) In review bottleneck
**JQL:** `project = PMH AND sprint in openSprints() AND status = "In Review"`
- **Use:** See if review is a bottleneck.
- **Variations:** Add `AND updated <= -2d` for aging review items.
- **Pitfalls:** Review status names vary (QA, Code Review, Test).

### 11) Spillover from last sprint
**JQL:** `project = PMH AND sprint in closedSprints() AND status != Done AND sprint = latestSprint()`
- **Use:** Identify incomplete work.
- **Variations:** Replace `latestSprint()` with a specific sprint name.
- **Pitfalls:** latestSprint() requires the Jira function; use sprint names if unavailable.

### 12) Unestimated in sprint
**JQL:** `project = PMH AND sprint in openSprints() AND Effort is EMPTY`
- **Use:** Prevent planning surprises.
- **Variations:** Replace Effort with Story Points.
- **Pitfalls:** Ensure field name matches your instance.

---

## Blocked Work (5 queries)

### 13) Blocked items
**JQL:** `project = PMH AND issueLinkType = blocks AND status != Done`
- **Use:** Surface active blockers.
- **Variations:** Use `issueLinkType = is blocked by` depending on link direction.
- **Pitfalls:** Requires consistent use of link types.

### 14) Blocked in sprint
**JQL:** `project = PMH AND sprint in openSprints() AND issueLinkType = blocks`
- **Use:** Blockers affecting the sprint.
- **Variations:** Add `AND status != Done` to hide completed items.
- **Pitfalls:** Link direction matters.

### 15) Dependencies by team
**JQL:** `project = PMH AND component = "Data" AND issueLinkType is not EMPTY`
- **Use:** Review dependency load by component.
- **Variations:** Use `labels` for team if components are not used.
- **Pitfalls:** Components must be consistently assigned.

### 16) High-risk blockers
**JQL:** `project = PMH AND issueLinkType = blocks AND Risk = High`
- **Use:** Escalate severe blockers.
- **Variations:** Add `AND status != Done`.
- **Pitfalls:** Risk field must be required.

### 17) Dependency aging
**JQL:** `project = PMH AND issueLinkType is not EMPTY AND updated <= -7d`
- **Use:** Identify stale dependencies.
- **Variations:** Combine with component/team filters.
- **Pitfalls:** updated might change due to unrelated edits.

---

## Aging Work (4 queries)

### 18) In progress too long
**JQL:** `project = PMH AND status = "In Progress" AND updated <= -7d`
- **Use:** Find stalled items.
- **Variations:** Use `-3d` for faster-moving teams.
- **Pitfalls:** updated can be noisy; consider `status changed to` if available.

### 19) Review aging
**JQL:** `project = PMH AND status = "In Review" AND updated <= -3d`
- **Use:** Detect review bottlenecks.
- **Variations:** Include QA/Test statuses if separate.
- **Pitfalls:** If review is a subtask, this may miss it.

### 20) Ready aging
**JQL:** `project = PMH AND status = Ready AND updated <= -14d`
- **Use:** Items stuck in Ready.
- **Variations:** Add `AND priority in (High, Highest)`.
- **Pitfalls:** Ready may be skipped in Kanban flows.

### 21) Backlog aging
**JQL:** `project = PMH AND status = Backlog AND updated <= -30d`
- **Use:** Prune stale backlog.
- **Variations:** Add `AND issuetype = Epic` for large items.
- **Pitfalls:** Stale items might still be valid; review before closing.

---

## Incidents (4 queries)

### 22) Active S1/S2 incidents
**JQL:** `project = PMH AND issuetype = Bug AND Severity in (S1, S2) AND status != Done`
- **Use:** Track critical incidents.
- **Variations:** Replace Severity with Priority if you use built-in priority.
- **Pitfalls:** Severity must be enforced at triage.

### 23) Bugs past SLA
**JQL:** `project = PMH AND issuetype = Bug AND status != Done AND updated <= -2d`
- **Use:** SLA breach detection.
- **Variations:** Use `-1d` for S1.
- **Pitfalls:** updated may not represent SLA; use SLA fields if available.

### 24) Missing postmortem
**JQL:** `project = PMH AND issuetype = Bug AND Severity = S1 AND issueLinkType is EMPTY`
- **Use:** Ensure postmortems exist.
- **Variations:** Filter for label `postmortem` if you use a dedicated issue type.
- **Pitfalls:** Requires consistent linking of postmortems.

### 25) Recurring incident area
**JQL:** `project = PMH AND issuetype = Bug AND component = "Payments" AND created >= -90d`
- **Use:** Find hot spots.
- **Variations:** Use label for service if components are not used.
- **Pitfalls:** Components must be assigned consistently.

---

## Roadmap Readiness (3 queries)

### 26) Epics missing target release
**JQL:** `project = PMH AND issuetype = Epic AND "Target Release" is EMPTY`
- **Use:** Ensure commitments are explicit.
- **Variations:** Add `AND status != Done`.
- **Pitfalls:** Use the correct custom field name.

### 27) Epics missing risk
**JQL:** `project = PMH AND issuetype = Epic AND Risk is EMPTY`
- **Use:** Require risk visibility.
- **Variations:** Add `AND priority in (High, Highest)`.
- **Pitfalls:** Risk field needs to be on the Epic screen.

### 28) Initiatives missing OKR link
**JQL:** `project = PMH AND issuetype = Initiative AND "OKR Link" is EMPTY`
- **Use:** Ensure outcome traceability.
- **Variations:** If no Initiatives, use Epics with label `outcome`.
- **Pitfalls:** Initiative type requires Advanced Roadmaps.

---

## OKR Progress (3 queries)

### 29) OKR-linked work in progress
**JQL:** `project = PMH AND "OKR Link" is not EMPTY AND status != Done`
- **Use:** See all work tied to OKRs.
- **Variations:** Filter to Initiatives or Epics.
- **Pitfalls:** OKR Link may be filled on non-OKR issues if not enforced.

### 30) Outcomes missing metrics
**JQL:** `project = PMH AND issuetype = Initiative AND "Metric Name" is EMPTY`
- **Use:** Ensure outcome metrics exist.
- **Variations:** Replace Initiative with Epic if needed.
- **Pitfalls:** Metric fields must be on the issue screen.

### 31) OKR status at risk
**JQL:** `project = PMH AND issuetype = Initiative AND "OKR Status" = "At Risk"`
- **Use:** Escalate underperforming outcomes.
- **Variations:** Add `AND updated <= -14d` for stale updates.
- **Pitfalls:** OKR Status must be updated on cadence.

---

## Roadmap/Discovery Hygiene (4 queries)

### 32) Discovery items missing hypothesis
**JQL:** `project = PMH AND issuetype = Opportunity AND "Hypothesis" is EMPTY`
- **Use:** Ensure discovery quality.
- **Variations:** Use Epics with label `opportunity` if no Opportunity type.
- **Pitfalls:** Hypothesis field must be defined.

### 33) Experiments without outcome
**JQL:** `project = PMH AND issuetype = Experiment AND status = Done AND "Outcome" is EMPTY`
- **Use:** Enforce learning capture.
- **Variations:** Use a custom label or Task type if no Experiment type.
- **Pitfalls:** Custom field names must match.

### 34) Opportunities stuck in Discovery
**JQL:** `project = PMH AND issuetype = Opportunity AND status = Discovery AND updated <= -14d`
- **Use:** Detect stalled discovery.
- **Variations:** Use `-30d` for longer discovery cycles.
- **Pitfalls:** Updated may not reflect actual progress.

### 35) Ready items missing dependencies
**JQL:** `project = PMH AND status = Ready AND issueLinkType is EMPTY AND Dependencies is EMPTY`
- **Use:** Ensure dependency awareness before delivery.
- **Variations:** Remove `Dependencies` if you don’t use the field.
- **Pitfalls:** Some items truly have no dependencies; confirm manually.
