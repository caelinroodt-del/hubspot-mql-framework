# HubSpot MQL Framework

**Stack:** HubSpot · Custom Properties · Workflow Automation · Sales Notifications

---

## Problem

When I joined The Recruitment Network, leads entered HubSpot from events, content downloads, and referrals and were routed directly to whichever sales rep was available. There was no qualification logic, no scoring, and no structured handoff. Reps were spending time on leads that weren't ready and missing signals on ones that were. Pipeline was invisible and conversion was inconsistent.

---

## Objective

Build a scoring model that distinguishes warm leads from cold ones, automates the handoff to sales at the right moment, and creates a feedback loop that improves over time.

---

## Scoring Model Design

Leads are scored across three dimensions, each weighted differently:

### 1. Fit Score (max 40 points)
Measures how well the lead matches the Ideal Customer Profile.

| Criterion | Points |
|-----------|--------|
| Decision-maker title (Owner, MD, Director) | 20 |
| Company size 5-50 employees | 10 |
| Active recruitment business (verified) | 10 |

### 2. Intent Score (max 40 points)
Measures demonstrated interest and buying signals.

| Behaviour | Points |
|-----------|--------|
| Attended a live webinar | 20 |
| Downloaded a gated asset | 15 |
| Visited pricing or membership page | 15 |
| Opened 3+ emails in 30 days | 10 |
| Clicked a sales CTA | 10 |

*Intent score decays: signals older than 30 days reduce by 50%*

### 3. Recency Score (max 20 points)
Measures how recently the lead has been active.

| Recency | Points |
|---------|--------|
| Active in last 7 days | 20 |
| Active in last 14 days | 15 |
| Active in last 30 days | 10 |
| Active in last 60 days | 5 |

---

## MQL Threshold

**Total score ≥ 55 = MQL**

This threshold was set initially based on ICP assumptions and refined over 60 days based on which MQLs actually converted to sales conversations.

---

## Workflow Logic

```
Lead score reaches 55+
        │
        ▼
Lifecycle stage automatically updated: Lead → MQL
        │
        ▼
Internal notification sent to assigned sales rep containing:
- Contact name, company, role
- Score breakdown (fit / intent / recency)
- Last 3 actions taken (what they engaged with)
- Suggested opening line based on most recent engagement
- Direct link to contact record in HubSpot
        │
        ▼
Lead enrolled in MQL nurture sequence (if rep does not action within 48hrs)
        │
        ▼
Sales rep logs outcome → feedback loop updates scoring weights
```

---

## Suppression Logic

Leads are excluded from MQL triggering if:
- Already a customer or member
- Previously marked as not a fit
- Unsubscribed from all communications
- Competitor or partner organisation

---

## Calibration Process

After 60 days of live data I ran a conversion analysis:

- Which MQLs became discovery calls?
- Which discovery calls became proposals?
- Which proposals closed?

This identified that **webinar attendance** was the highest-converting single intent signal (3x more likely to close than content downloads alone), which led to increasing the webinar attendance score from 15 to 20 points.

---

## Results

Over 6 months following implementation:

- Pipeline opportunity value: £1.09m → £1.91m (+75% YoY)
- Deal volume: 298 → 566 (+90% YoY)
- Closed-won revenue: £971k → £2.12m (+118% YoY)
- Sales rep feedback: "I now know why I'm calling someone before I pick up the phone"

---

## What I Would Build Next

- Negative scoring for disengagement signals (e.g. email unsubscribes, long periods of inactivity)
- Automated re-nurture trigger when a previously high-scoring lead goes cold
- A/B testing framework for different MQL thresholds by lead source
# hubspot-mql-framework
