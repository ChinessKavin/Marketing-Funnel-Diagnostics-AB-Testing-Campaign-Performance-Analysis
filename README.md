# 📊 Marketing Funnel Diagnostics: A/B Testing & Campaign Performance Analysis

An analysis of a multi-channel marketing dataset (10K+ records) to evaluate campaign performance, diagnose a real conversion-rate drop, quantify its business cost, and test whether email personalization actually improves conversion.

## What this project covers

- **Performance overview** - conversion rate, retention rate, and channel-level quality across House Ads, Email, Facebook, Instagram, and Push
- **Root-cause investigation** - traced a sudden conversion drop to a language targeting bug in House Ads, using a channel → day-of-week → language diagnostic funnel
- **Business impact modeling** - estimated **~24 lost subscribers** from the bug using a pre-bug baseline conversion model
- **A/B testing** - tested email personalization against a control group, found a significant **+38.85% lift (p = 0.0065)**, then segmented the result by language and age group to check if it held up across the whole audience

## Key finding

The overall A/B test result looked like a clear win -  but segmenting it revealed personalization **significantly helps users under 30** and **significantly hurts users 30 and older**. Averages hid the real story; this only surfaced once the data was broken down by segment.

## Tech stack

Python · pandas · numpy · matplotlib · scipy.stats

## 📖 Want the full breakdown?

This README covers the highlights. For the detailed walkthrough - the investigation step-by-step, the reasoning behind each decision, and the full segment results - read the write-up on Medium:

**[Medium Page](https://happinesskanife.medium.com/)(#)**

## Files

- `marketing_analysis.ipynb` - full notebook with code, charts, and analysis
- `marketing.csv` - source dataset
