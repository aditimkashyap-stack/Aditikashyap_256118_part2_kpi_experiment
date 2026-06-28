[README.md](https://github.com/user-attachments/files/29429105/README.md)
# Part 2: KPI Experiment Analysis — Onboarding & Activation Campaign

## Business Context

A subscription-based digital product company launched a new onboarding and activation campaign to improve user conversion and early engagement. Users were randomly split into:
- **Control group (693 users):** Existing onboarding experience
- **Treatment group (715 users):** New campaign experience

Leadership needs a data-backed decision on whether to roll the treatment out to all users.

---

## Dataset Description

**File:** `data/campaign_experiment_data.xlsx`  
**Rows:** 1,408 users | **Columns:** 16

| Column | Description |
|---|---|
| user_id | Unique user identifier |
| signup_date | Date of signup (Excel serial number) |
| experiment_group | Control or Treatment |
| region | North / South / East / West |
| device_type | Desktop / Mobile / Tablet |
| traffic_source | Email / Organic / Paid Search / Referral / Social |
| plan_type | Basic / Free / Premium |
| visited_landing_page | Binary (0/1) |
| started_trial | Binary (0/1) |
| completed_onboarding | Binary (0/1) |
| converted_to_paid | Binary (0/1) — primary outcome |
| revenue_30d | Revenue generated in 30 days |
| support_tickets_30d | Number of support tickets raised |
| refund_requested | Binary (0/1) |
| days_to_convert | Days from signup to conversion (only for converters) |
| engagement_score | Continuous score (0–100) |

---

## Data Quality Checks (Task 4)

| Check | Finding | Handling |
|---|---|---|
| Missing values | `device_type`: 18, `traffic_source`: 24, `engagement_score`: 14, `days_to_convert`: 1,336 (only populated for converters) | `days_to_convert` nulls are structurally correct (non-converters). Others marked as "Unknown" in segment analysis |
| Group counts | Control: 693, Treatment: 715 | Groups are balanced (~50/50 split); no reweighting needed |
| Duplicate user IDs | 8 duplicate IDs found | Flagged; analysis uses full dataset as duplicates may represent re-signups; sensitivity check showed no impact on conclusions |
| Invalid binary values | All binary columns contain only 0 or 1 | No issues found |
| Revenue outliers | Max Control revenue: $8,610.72 vs. Treatment: $2,660.21 | One extreme outlier in Control confirmed; retained for ARPU but noted in ARPC analysis |
| Segment distribution | Regions, devices, and plan types are broadly balanced across groups | No major imbalances detected |

---

## North Star Metric

**Paid Conversion Rate** — the proportion of users who convert to a paid subscription.

This metric was chosen because:
- It directly measures business revenue generation
- It is the primary objective of the onboarding campaign
- All funnel steps (landing page visit, trial start, onboarding completion) are drivers of this metric
- It connects signup activity to actual monetization

**Risk of blind optimization:** Optimizing purely for conversion rate could mask declining revenue quality (lower ARPC), increased support burden, or premature conversions that later churn.

---

## KPI Tree Summary

```
Paid Conversion Rate (North Star)
├── Funnel Engagement
│   ├── Landing Page Visit Rate
│   └── Trial Start Rate
├── Activation Quality
│   ├── Onboarding Completion Rate
│   └── Engagement Score
└── Revenue Impact
    ├── Average Revenue per User (ARPU)
    └── Average Revenue per Converted User (ARPC)

Guardrail Metrics
├── Refund Rate
├── Support Ticket Rate
└── Days to Convert
```

Full KPI tree visualization: `outputs/kpi_tree.png`

---

## Experiment Analysis Approach

1. Loaded raw data and performed data quality checks
2. Computed group-level summary metrics for all required KPIs
3. Performed segment-level breakdowns by Region, Device Type, and Traffic Source
4. Ran a one-tailed z-test for proportions on the Paid Conversion Rate (primary metric)
5. Evaluated guardrail metrics for regression signals
6. Synthesized findings into a recommendation

Full analysis: `analysis/experiment_analysis.xlsx`

---

## Hypothesis Test Summary

- **H₀:** Paid conversion rate is the same in Control and Treatment
- **H₁:** Treatment paid conversion rate > Control paid conversion rate
- **Test:** One-tailed z-test for proportions
- **Significance level:** α = 0.05
- **Result:** Z = 3.25, p-value = 0.00057
- **Decision:** Reject H₀ — Treatment significantly outperforms Control

Full notes: `analysis/hypothesis_test_notes.md`

---

## Guardrail Metrics Considered

| Guardrail Metric | Control | Treatment | Risk? |
|---|---|---|---|
| Refund Rate | 0.0% | 0.42% | Low — small absolute rate |
| Support Ticket Rate | 14.7% | 24.8% | **HIGH** — statistically significant increase |
| Average Revenue per Converted User | $1,630.10 | $770.41 | **MEDIUM** — Treatment converts more but at lower value |
| Days to Convert | 8.9 days | 6.4 days | Positive — faster conversion |

---

## Final Recommendation

**Launch to all users** — with monitoring.

The Treatment group showed a **2.2× improvement in paid conversion rate** (3.2% → 7.0%), statistically significant at p < 0.001. Guardrail risks (higher support volume, lower ARPC) are manageable and merit monitoring dashboards post-launch.

Full rationale: `outputs/recommendation_memo.md`

---

## Assumptions and Limitations

- Analysis uses 30-day revenue window; longer-term LTV impact is unknown
- 8 duplicate user IDs were retained; if these are data errors, results could shift slightly
- ARPC decline may reflect plan-type mix differences among converters
- No pre-experiment balance verification (SRM check not performed)
- Engagement score missing for 14 users; likely negligible impact

---

## Screenshots Included

| File | Shows |
|---|---|
| `screenshots/summary_metrics.png` | Control vs. Treatment summary comparison table |
| `screenshots/hypothesis_test_output.png` | Z-test calculation and output |
| `screenshots/kpi_tree_preview.png` | KPI tree visualization |
