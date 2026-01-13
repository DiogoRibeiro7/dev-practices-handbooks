# Tooling Overview

This directory contains CLI utilities referenced throughout the book.

## Template Generator (Chapter 15)
Automates requirement template creation.
```
pip install -r dashboards/requirements.txt  # reuse shared deps
python tools/template_generator.py --work-type feature --domain healthcare --risk high --requirement "Improve onboarding flow with 15% activation lift" --export markdown jira azure
```
Outputs appear in `tools/generated/` (Markdown, Jira JSON, Azure YAML) with acceptance-criteria suggestions and checklist validation.

## Quality Analyzer (Chapter 12)
Scores requirement documents for completeness, clarity, consistency, and acceptance-criteria logic.
```
python tools/quality_analyzer.py tools/data/sample_requirement.md --output tools/generated/quality_report.md
```
Produces Markdown/JSON scorecards with recommendations.

## Documentation Automation (Chapter 17)
Generates README templates, ADRs, model cards, and audience-specific requirement views.
```
python tools/doc_automation.py --mode readme --project-type ml_service --tech python fastapi mlflow --repo https://github.com/org/repo --deployment "Kubernetes (prod/eu-west-1)"
...
```

## Formal Methods Toolkit (Chapter 13)
Converts business rules to logic, builds state machines, decision tables, and solves constraint problems.
```
python tools/formal_methods.py --mode logic --rules "user_is_verified -> allow_purchase" "order_value > 100 -> flag_manual_review"
...
```

## Facilitation Toolkit (Chapter 2)
Supports translation workshops with assumption canvases, risk matrices, stakeholder dashboards, and mental-model comparisons.
```
python tools/facilitation.py --mode assumption --items "User trusts insights" "Data is refreshed daily" --evidence "Customer interviews" "ETL schedule"
python tools/facilitation.py --mode risk --items "Model drift" "API downtime" --probabilities High Medium --impacts High High
python tools/facilitation.py --mode stakeholder --items "VP Product" "Head of Data" --influence High Medium --alignment Supporter Neutral
python tools/facilitation.py --mode mental --items "Business" "Engineering" "Data Science" --dimensions "Success Metric" "Source of Truth" "Decision Cadence"
```
Artifacts land in `tools/generated/` for quick sharing.

## Experiment Planning Toolkit (Chapter 8)
Guides hypothesis formation, sample-size planning, experiment tracking, and reporting.
```
# Hypothesis worksheet
python tools/experiment_planner.py --mode hypothesis \
  --problem "Activation drop in onboarding" \
  --signal "Activation rate" --guardrail "Churn, CSAT"

# Sample size per variant
python tools/experiment_planner.py --mode sample --alpha 0.05 --power 0.8 --baseline 0.2 --mde 0.03

# Tracking template
python tools/experiment_planner.py --mode tracking --metrics "Activation" "Retention" --checkpoints "Week1" "Week2"

# Analysis report scaffold
python tools/experiment_planner.py --mode report --report-name "Onboarding Experiment"
```
Outputs drop into `tools/generated/` (Markdown + JSON) ready for Chapter 8 workflows.
