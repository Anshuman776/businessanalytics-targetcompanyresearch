# Data Validation

## Purpose
Document validation checks and quality metrics applied to raw data.

## Validation Checks
- Schema checks: required fields present (name, URL, revenue, employees).
- Consistency: matching company names across sources.
- Plausibility: revenue/employee ratios within expected bounds.
- Duplicate detection and resolution.

## Missing Data Handling
- Impute conservatively or mark fields as `N/A`.
- Flag companies with critical missing fields for manual review.

## Quality Metrics
- Completeness percentage, conflict rate, duplicate rate.

## Versioning & Audit
- Timestamped exports; keep raw snapshots in `raw_research/`.
