## Problem Statement
Analysts lack a single place to review activation metrics, causing delays in reporting.

## Hypothesis
If we provide a self-serve quality dashboard with activation metrics, analysts will reduce reporting time by 20%.

## Success Metrics
- Activation report cycle time baseline 10 days -> target 7 days
- Analyst satisfaction survey from 3.2 -> 4.0

## Acceptance Criteria
- Given the dashboard is live, when analysts load it during business hours, then P95 latency remains under 1.5 seconds.
- When activation data is unavailable, the system displays clear fallback messaging.
- Dashboard uptime meets 99.5% SLO monthly.

## Risks
- Data source downtime
- Adoption blocked by access provisioning
