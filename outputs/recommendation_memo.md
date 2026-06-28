# Recommendation Memo: New Onboarding Campaign Decision

**To:** Leadership / Product Decision-Makers  
**From:** Business Analytics  
**Date:** June 2026  
**Re:** A/B Test Results — Onboarding & Activation Campaign

---

## Executive Summary

The new onboarding and activation campaign should be **launched to all users**. The Treatment group demonstrated a statistically significant 2.2× improvement in paid conversion rate (3.2% → 7.0%, p < 0.001). The improvement is consistent across all regions and device types. Two guardrail metrics — support ticket volume and revenue per converted user — show regressions that require monitoring but do not block launch.

---

## Business Problem Statement

**Decision to be made:** Whether to replace the existing onboarding experience with the new campaign experience for all new users.

**Who is impacted:** All new users who sign up for the product going forward; the Customer Support team (operational impact); Finance (revenue quality impact).

**Metric that should improve:** Paid Conversion Rate — the share of users who convert from free/trial to a paid plan within 30 days of signup.

**Risks to monitor:** A campaign that accelerates conversions could attract lower-value users, increase support burden, or produce higher refund rates. These must not be ignored even when the headline metric improves.

**Evidence required:** A statistically significant improvement in paid conversion rate (p < 0.05), with no unacceptable regression in guardrail metrics.

---

## North Star Metric

**Paid Conversion Rate**

| Why this is the North Star | Why other metrics are supporting metrics |
|---|---|
| Directly measures revenue-generating behavior | Landing page visit rate measures awareness, not outcomes |
| Primary objective of the campaign | Trial start rate is a funnel step, not a business result |
| Connects user activation to subscription growth | Engagement score predicts conversion but isn't the outcome itself |
| Used for launch/no-launch threshold decision | ARPU is influenced by plan mix, not purely by campaign success |

**What could go wrong if optimized blindly:** A campaign that drives many low-value conversions (e.g., Basic/Free plan upgrades) could inflate conversion rate while reducing total revenue per cohort. This is exactly what the ARPC guardrail is designed to detect.

---

## KPI Tree Explanation

```
[North Star] Paid Conversion Rate
      │
      ├── [Driver 1] Funnel Engagement
      │       ├── Landing Page Visit Rate      (+9.5pp, Control 63.6% → Treatment 72.6%)
      │       └── Trial Start Rate              (+3.98pp, Control 25.1% → Treatment 29.1%)
      │
      ├── [Driver 2] Activation Quality
      │       ├── Onboarding Completion Rate   (+5.67pp, Control 15.6% → Treatment 21.3%)
      │       └── Engagement Score              (+5.9 pts, Control 57.0 → Treatment 62.9)
      │
      └── [Driver 3] Revenue Impact
              ├── ARPU                          (Control $51.75 → Treatment $53.88)
              └── ARPC                          (Control $1,630 → Treatment $770 ⚠️)

[Guardrail Metrics]
  ├── Refund Rate                  (Control 0.0% → Treatment 0.42%)
  ├── Support Ticket Rate          (Control 14.7% → Treatment 24.8% ⚠️)
  └── Days to Convert              (Control 8.9d → Treatment 6.4d ✅)
```

The KPI tree shows that the Treatment campaign improves every funnel step. The ARPC decline is the primary concern and is explored further below.

---

## Experiment Result Summary

| Metric | Control | Treatment | Change | Direction |
|---|---|---|---|---|
| Users | 693 | 715 | +22 | — |
| Landing Page Visit Rate | 63.6% | 72.6% | +9.0pp | ✅ |
| Trial Start Rate | 25.1% | 29.1% | +4.0pp | ✅ |
| Onboarding Completion Rate | 15.6% | 21.3% | +5.7pp | ✅ |
| **Paid Conversion Rate** | **3.17%** | **6.99%** | **+3.82pp (+120%)** | ✅ |
| ARPU | $51.75 | $53.88 | +$2.13 | ✅ |
| ARPC | $1,630.10 | $770.41 | -$859.69 (-53%) | ⚠️ |
| Refund Rate | 0.00% | 0.42% | +0.42pp | ⚠️ |
| Support Ticket Rate | 14.7% | 24.8% | +10.1pp | ⚠️ |
| Avg Engagement Score | 57.03 | 62.93 | +5.90 pts | ✅ |
| Avg Days to Convert | 8.9 days | 6.4 days | -2.5 days | ✅ |

---

## Hypothesis Test Interpretation

A one-tailed z-test for proportions was conducted on the paid conversion rate.

- **Z-statistic:** 3.25
- **P-value:** 0.00057 (one-tailed)
- **Result:** Statistically significant at α = 0.05 (and α = 0.001)

The probability of observing a difference this large by chance is 0.057% — providing very strong evidence that the Treatment campaign genuinely improves conversion, not just due to random variation.

---

## Guardrail Analysis

### 1. Refund Rate
Control: 0.0% | Treatment: 0.42%

A small but non-zero regression. Treatment produced 3 refund requests vs. 0 in Control. At current conversion volumes this is manageable, but should be tracked as the campaign scales. Not a blocker for launch.

### 2. Support Ticket Rate ⚠️
Control: 14.7% | Treatment: 24.8% (statistically significant increase)

This is the most material guardrail risk. 1 in 4 Treatment users raised a support ticket, vs. 1 in 7 in Control. This increase is likely driven by the new onboarding flow creating friction or unmet expectations for users who don't immediately understand the product. **Mitigation:** Increase support staffing ahead of full launch and audit the onboarding steps with the highest ticket correlation.

### 3. ARPC (Revenue per Converted User) ⚠️
Control: $1,630.10 | Treatment: $770.41

Treatment converts more users, but each converted user generates 53% less revenue. This could reflect: (a) Treatment attracts more Basic/Free plan converters, (b) converters in Treatment are less committed (driven by the campaign rather than genuine intent), or (c) one outlier inflates the Control ARPC ($8,610 single user). **Mitigation:** Segment ARPC by plan type post-launch and monitor 90-day LTV to confirm whether early converters in Treatment retain.

### 4. Days to Convert ✅
Control: 8.9 days | Treatment: 6.4 days

Faster conversion is positive — it reduces the window for churn before first payment and indicates higher funnel efficiency.

---

## Segment-Level Insight

Paid conversion improvement is **consistent across all segments**:

| Segment | Control Rate | Treatment Rate | Lift |
|---|---|---|---|
| Region: East | 2.5% | 6.4% | +3.9pp |
| Region: North | 3.4% | 8.9% | +5.5pp |
| Region: South | 3.3% | 7.6% | +4.3pp |
| Region: West | 3.4% | 5.0% | +1.6pp |
| Device: Desktop | 4.5% | 6.5% | +2.0pp |
| Device: Mobile | 2.6% | 7.3% | +4.7pp |
| Device: Tablet | 1.8% | 7.1% | +5.3pp |

The breadth of improvement (no segment shows a decline) significantly increases confidence in the overall recommendation. Mobile and Tablet users show the highest absolute lift, suggesting the campaign is particularly effective for non-desktop users.

---

## Final Recommendation

### **Launch to all users.**

The evidence is clear:
1. Paid conversion rate more than doubled (p < 0.001)
2. All funnel metrics improved
3. Engagement score increased, indicating users are more activated
4. Conversion is faster, reducing time-to-revenue

Guardrail risks are real but manageable. The overall revenue impact (ARPU increased despite ARPC declining) confirms the volume gain offsets the per-user revenue drop.

---

## Risks and Limitations

- **ARPC decline** could compound if lower-value converters churn faster; 90-day LTV cohort analysis is essential
- **Support volume increase** requires proactive staffing; failure to do so could harm NPS and brand
- **30-day window** may not capture full conversion behavior; some delayed converters in Control may not yet appear
- **8 duplicate user IDs** were retained; these should be investigated by Engineering
- **No SRM check** was performed; if randomization was imperfect, results could be biased

---

## Next Steps

1. **Pre-launch:** Increase support team capacity by ~15% to absorb expected ticket volume
2. **Launch:** Roll out Treatment to 100% of new users
3. **Day 7:** Review early refund rate and support ticket trends
4. **Day 30:** Validate ARPU and conversion rate match experiment predictions
5. **Day 90:** Run LTV cohort analysis to confirm ARPC regression doesn't compound
6. **Ongoing:** Build monitoring dashboard for all KPI tree metrics; set alert thresholds for refund rate > 1.0% and support rate > 30%
