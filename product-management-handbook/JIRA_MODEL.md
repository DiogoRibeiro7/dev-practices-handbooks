# Canonical Jira Data Model

This model is the default configuration for the book. It favors clarity, minimal custom fields, and reusable workflows. It supports both product outcomes and delivery tracking.

## 1) Project approach (single project vs multi-project)
**Chosen approach: single product area project, multi-team boards.**

**Why:**
- Keeps a single backlog and consistent reporting for outcomes + delivery.
- Reduces cross-project linking, permission complexity, and field duplication.
- Enables shared workflows and fields while allowing multiple boards by JQL.

**When to split into multiple projects:**
- Regulatory/data isolation requirements.
- Independent release trains with separate governance.
- Distinct product lines with different workflows and ownership.

## 2) Issue types and when to use them

**Initiative (if Advanced Roadmaps is available)**
- Purpose: top-level outcome or strategic objective spanning multiple epics.
- Use when: an objective requires multiple epics or teams over a quarter.
- If Advanced Roadmaps is not available: use an **Epic** plus an "Outcome" label and a dedicated "Initiative" custom field (single-select) to represent the outcome layer.

**Epic**
- Purpose: a coherent product deliverable or outcome slice (4–12 weeks).
- Use when: you need multiple stories/tasks grouped to a release or outcome.

**Story**
- Purpose: user-facing capability that delivers value.
- Use when: a feature can be framed from the user’s perspective.

**Task**
- Purpose: non-user-facing delivery work (refactor, infra, docs).
- Use when: work is necessary but not a user story.

**Bug**
- Purpose: defect from expected behavior.
- Use when: production or test failures require remediation.

**Spike**
- Purpose: time-boxed research/learning.
- Use when: uncertainty is too high to estimate or commit to a story.

## 3) Minimal custom fields (avoid sprawl)
These fields are required for prioritization, risk, and outcome tracking. Keep the total minimal and enforce usage rules.

| Field | Type | Applies to | Required when | Notes |
| --- | --- | --- | --- | --- |
| Customer Impact | Single select (Low/Med/High) | Initiative, Epic, Story | Any item in "Ready" | Describes severity/benefit to customers. |
| Reach | Number | Initiative, Epic | Prioritization | Estimated users/segments affected. |
| Confidence | Single select (Low/Med/High) | Initiative, Epic | Prioritization | Evidence strength for expected impact. |
| Effort | Number (points or person-days) | Epic, Story, Task | Planning | Use a single unit consistently. |
| Risk | Single select (Low/Med/High) | Initiative, Epic, Bug | Any item in "Ready" | Delivery or product risk level. |
| Dependencies | Text (short) | Initiative, Epic, Story, Task | When blocked | Plain list or links; avoid long narratives. |
| Target Release | Version picker | Epic, Story, Bug | When committed | Must map to a release version. |
| OKR Link | URL | Initiative, Epic | When tied to OKR | Link to OKR doc or tracker. |

## 4) Workflows

### 4.1 Discovery workflow (Initiatives/Epics/Spikes)
States:
- **Intake** → **Discovery** → **Validated** → **Planned** → **Done** → **Canceled**

Transitions:
- Intake → Discovery (triage complete)
- Discovery → Validated (evidence meets threshold)
- Validated → Planned (delivery plan exists, DoR met)
- Planned → Done (initiative/epic delivered)
- Any state → Canceled (explicit reason required)

Notes:
- Keep Discovery focused on hypotheses and evidence.
- Validation requires measurable success criteria.

### 4.2 Delivery workflow (Stories/Tasks)
States:
- **Backlog** → **Ready** → **In Progress** → **In Review** → **Done** → **Canceled**

Transitions:
- Backlog → Ready (DoR met)
- Ready → In Progress (assignee + start)
- In Progress → In Review (PR/QA)
- In Review → Done (acceptance criteria met)
- Any state → Canceled (explicit reason required)

Notes:
- Keep a single review state to avoid process bloat.
- Avoid additional statuses unless you can define explicit entry/exit rules.

### 4.3 Bug/Incident workflow
States:
- **Triage** → **Fix In Progress** → **Verify** → **Done**
- **Triage** → **Won’t Fix**

Transitions:
- Triage → Fix In Progress (severity set, owner assigned)
- Fix In Progress → Verify (fix ready for validation)
- Verify → Done (confirmed resolved)
- Triage → Won’t Fix (reason required)

Notes:
- Track severity as a standard field or label, not a custom workflow.

## 5) Conventions and rules of use

### 5.1 Naming conventions
- Initiative: "[Outcome] Increase activation by 20%"
- Epic: "[Theme] Improve onboarding flow"
- Story: "As a <role>, I can <capability> so that <benefit>"
- Task: "[Tech] Add caching for pricing API"
- Bug: "[Bug] Checkout fails on Safari"
- Spike: "[Spike] Evaluate usage-based billing options"

### 5.2 Labels usage rules
- Labels are **temporary and query-oriented**. Do not use them as taxonomy.
- Limit to 1–3 labels per issue.
- Use for short-lived campaigns or experiments (e.g., `pricing-test`, `beta`).

### 5.3 Components ownership rules
- Components represent stable ownership boundaries (team or subsystem).
- Each component must have a single accountable owner.
- Use components for routing, not for prioritization.

### 5.4 Definition of Ready (DoR)
- Clear user or business outcome.
- Acceptance criteria written.
- Dependencies identified (if any).
- Risk and Customer Impact set.
- Estimated Effort set for planning.

### 5.5 Definition of Done (DoD)
- Acceptance criteria met.
- Tests or validation completed.
- Release notes updated if customer-facing.
- Target Release set and verified.
- Monitoring/rollback plan defined for high-risk items.

## 6) Rules-of-use and anti-patterns

### Rules-of-use
- Every Epic must link to a parent Initiative (or outcome label if no Advanced Roadmaps).
- Stories/Tasks must link to an Epic.
- Bugs must link to the Epic or release they impact.
- Spikes must produce a decision output (go/no-go or recommended path).

### Anti-patterns to avoid
- Overloading labels as permanent taxonomy.
- Creating new fields without a usage rule and owner.
- Adding workflow states without entry/exit criteria.
- Logging every small task as a Story.
- Mixing discovery and delivery in the same workflow state.

## 7) Minimum configuration checklist
- Issue types created and mapped to workflows.
- Custom fields added with contexts and screen assignments.
- Components configured with owners.
- Versions/releases defined and used.
- DoR/DoD documented and enforced in “Ready” and “Done”.
