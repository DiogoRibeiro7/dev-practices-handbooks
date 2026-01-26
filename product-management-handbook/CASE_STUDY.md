# Case Study: PulseBoard Analytics

## Fictional product + org
**Product:** PulseBoard, a B2B SaaS analytics tool that helps mid-market companies onboard customers faster and expand usage.

**Org structure**
- **Squad A (Growth):** onboarding + activation
- **Squad B (Monetization):** pricing, upgrades, billing
- **Platform team:** data pipelines, permissions, audit logs
- **Customer support:** intake, incident reporting, feedback routing

## Seed data

### Initiatives (2)
| ID | Initiative | Owner | Outcome metric | OKR link | Priority |
| --- | --- | --- | --- | --- | --- |
| INIT-1 | Reduce onboarding time by 30% | PM (Growth) | Median time-to-first-dashboard | OKR-A | P0 |
| INIT-2 | Increase self-serve upgrades by 20% | PM (Monetization) | Upgrade conversion rate | OKR-B | P0 |

### Epics (6)
| ID | Epic | Initiative | Team | Target release | Priority | Dependencies |
| --- | --- | --- | --- | --- | --- | --- |
| EPIC-1 | Guided setup checklist | INIT-1 | Growth | R1 | P0 | Platform auth events |
| EPIC-2 | CSV import + API | INIT-1 | Growth | R1 | P1 | Platform data pipeline |
| EPIC-3 | In-app tips and nudges | INIT-1 | Growth | R2 | P2 | None |
| EPIC-4 | Pricing page refresh | INIT-2 | Monetization | R1 | P0 | Design system update |
| EPIC-5 | Self-serve upgrade flow | INIT-2 | Monetization | R1 | P0 | Billing service changes |
| EPIC-6 | Audit log + permissions | INIT-2 | Platform | R2 | P1 | None |

### Issue list (30 items)
| ID | Type | Summary | Team | Priority | Status | Depends on |
| --- | --- | --- | --- | --- | --- | --- |
| OPP-1 | Opportunity | Slow onboarding causing churn | Growth | P0 | Discovery | None |
| OPP-2 | Opportunity | Upgrade friction for SMBs | Monetization | P0 | Discovery | None |
| EXP-1 | Experiment | Shorter setup wizard test | Growth | P1 | Done | OPP-1 |
| EXP-2 | Experiment | Pricing page A/B test | Monetization | P1 | In Progress | OPP-2 |
| SPK-1 | Spike | Evaluate bulk import API limits | Growth | P1 | Done | EPIC-2 |
| ST-101 | Story | Setup checklist UI | Growth | P0 | In Progress | EPIC-1 |
| ST-102 | Story | Setup completion tracking | Growth | P0 | Ready | EPIC-1 |
| ST-103 | Story | Checklist reminders | Growth | P1 | Backlog | EPIC-1 |
| TS-104 | Task | Add onboarding event schema | Platform | P0 | In Progress | EPIC-1 |
| ST-105 | Story | CSV import screen | Growth | P1 | Ready | EPIC-2 |
| ST-106 | Story | Import validation errors | Growth | P1 | Backlog | EPIC-2 |
| TS-107 | Task | Pipeline batching for imports | Platform | P0 | In Progress | EPIC-2 |
| ST-108 | Story | API token creation for import | Growth | P2 | Backlog | EPIC-2 |
| ST-109 | Story | Nudge banner variants | Growth | P2 | Backlog | EPIC-3 |
| ST-110 | Story | Behavior-based tips | Growth | P2 | Backlog | EPIC-3 |
| TS-111 | Task | Tip content library | Growth | P2 | Backlog | EPIC-3 |
| ST-112 | Story | Pricing page redesign | Monetization | P0 | Ready | EPIC-4 |
| ST-113 | Story | Pricing FAQ | Monetization | P1 | Backlog | EPIC-4 |
| TS-114 | Task | Design system tokens | Platform | P1 | In Progress | EPIC-4 |
| ST-115 | Story | Upgrade CTA in app | Monetization | P0 | In Progress | EPIC-5 |
| ST-116 | Story | Checkout stepper | Monetization | P0 | Ready | EPIC-5 |
| TS-117 | Task | Billing webhook handler | Platform | P0 | In Progress | EPIC-5 |
| ST-118 | Story | Upgrade confirmation page | Monetization | P1 | Backlog | EPIC-5 |
| TS-119 | Task | Audit log storage | Platform | P1 | Ready | EPIC-6 |
| ST-120 | Story | Permissions UI | Platform | P1 | Backlog | EPIC-6 |
| ST-121 | Story | Role-based access | Platform | P1 | Backlog | EPIC-6 |
| BG-122 | Bug | Import fails on large files | Growth | P0 | Triage | EPIC-2 |
| BG-123 | Bug | Pricing page layout shift | Monetization | P1 | Fix In Progress | EPIC-4 |
| BG-124 | Bug | Upgrade email not sent | Monetization | P0 | Fix In Progress | EPIC-5 |
| BG-125 | Bug | Audit log export timeout | Platform | P1 | Backlog | EPIC-6 |
| BG-126 | Bug | Setup checklist not saving | Growth | P0 | Verify | EPIC-1 |

### Key dependencies
- EPIC-1 depends on TS-104 (Platform event schema).
- EPIC-2 depends on TS-107 (Pipeline batching).
- EPIC-4 depends on TS-114 (Design system tokens).
- EPIC-5 depends on TS-117 (Billing webhook handler).

### Priorities (default order)
1) EPIC-1 and EPIC-5 (P0, revenue + activation)
2) EPIC-2 and EPIC-4 (P1, support + conversion)
3) EPIC-6 and EPIC-3 (P2, enterprise requirements)
