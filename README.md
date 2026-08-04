# Marketing Funnel Diagnostics: Campaign Performance & A/B Testing Analysis

An end-to-end analysis of a multi-channel marketing dataset, evaluating campaign performance, diagnosing a real conversion-rate drop, quantifying its business impact, and using A/B testing to guide optimization decisions.

---

## Overview

This project analyzes marketing campaign data to help a business understand:

- Which marketing channels perform best
- How different customer segments (age, language, subscribing channel) behave
- Why a sudden drop in conversion rate occurred and what it cost the business
- Whether a personalization test on email campaigns actually improved conversions

The analysis moves from exploratory data analysis → automated reporting functions → root-cause investigation → A/B test evaluation, mirroring how a marketing analytics workflow would run in practice.

---

## Business Problem

The marketing team needed to:

1. Evaluate campaign performance across channels (House Ads, Instagram, Facebook, Email, Push)
2. Understand the factors driving conversion and retention
3. Investigate and diagnose an unexplained drop in conversions
4. Use A/B testing to decide whether a new email personalization strategy should be rolled out

---

## Project Structure

| Section | Description |
|---|---|
| **1. Import Libraries & Data** | Load `pandas`, `numpy`, `matplotlib`, and the raw `marketing.csv` dataset |
| **2. Data Exploration & Preprocessing** | Inspect structure, fix data types (booleans, datetimes) |
| **3. Feature Engineering** | Create derived fields: `is_house_ads`, `is_correct_lang`, `channel_code`, `DoW` |
| **4. Exploratory Analysis & Summary Statistics** | Conversion rate, retention rate, channel/language/age-group breakdowns, daily trends |
| **5. Automated Reporting Functions** | Reusable `conversion_rate()` and `plotting_conv()` functions for repeatable analysis |
| **6. Conversion Drop Investigation** | Root-cause analysis of a sharp conversion drop on **January 11** |
| **7. A/B Testing Analysis** | Statistical evaluation of an email personalization test, including segment-level breakdowns |

---

## Data Cleaning & Feature Engineering

- Converted `converted` and `is_retained` columns to boolean types
- Parsed `date_served`, `date_subscribed`, and `date_canceled` into datetime objects
- Engineered new features:
  - `is_house_ads` - flags whether the channel was House Ads
  - `is_correct_lang` - flags whether an ad was shown in the user's preferred language
  - `channel_code` - numeric encoding of marketing channel
  - `DoW` — day of week the user subscribed

---

## Key Analyses & Findings

### Overall Performance
- Calculated **conversion rate** (visitors → subscribers) and **retention rate** (subscribers who stayed subscribed)
- Broke down retention by `subscribing_channel` to compare channel quality, not just channel volume

### Segmentation
- Conversion rate by **language displayed** and **age group**
- Marketing channel distribution across **age groups**
- Language preference trends **over time** and **across age groups**

### Root-Cause: The January 11 Conversion Drop
A custom `conversion_rate()` + `plotting_conv()` pipeline was built to automate the drop investigation across dimensions:

1. **By channel** → isolated **House Ads** as the affected channel
2. **By day of week** → ruled out a weekly seasonality effect
3. **By language** → found English conversions dropped sharply after Jan 11, with **no ads served in other languages for a two-week period**
4. **Language-targeting audit** → confirmed a **language-targeting bug**: users were being shown ads in the wrong language at a much higher rate starting Jan 11

### Quantifying the Business Impact
Using pre-bug conversion rates as a baseline, an **index model** was built to estimate what conversion rates *should* have been for Spanish, Arabic, and German users (relative to English performance), and compared against what actually happened during the bug window.

**Estimated impact: 24 lost subscribers** during the affected period - a meaningful loss for a growing company, especially one expanding into new-language markets.

---

## A/B Testing: Email Personalization

Evaluated whether personalizing email content improved conversion versus a control (generic) version.

**Method:**
1. Measured test allocation across `control` vs `personalization` groups
2. Compared conversion rates between groups
3. Built a reusable `lift()` function to quantify the relative improvement
4. Ran an independent samples **t-test** (`scipy.stats.ttest_ind`) to test statistical significance
5. Built an `ab_segmentation()` function to re-run the lift/t-test analysis **within** subgroups (by language and by age group), to check whether the effect held consistently across segments — or was being driven by one group

**Why this matters:** a positive average lift can mask the fact that the effect is concentrated in (or absent from) specific segments, segment-level testing avoids rolling out a change that helps some users and hurts others.

---

## Tech Stack

- **Python** - `pandas`, `numpy`
- **Visualization** - `matplotlib`
- **Statistics** - `scipy.stats` (independent t-test)


---

## Key Takeaways

- **Data quality issues can silently erode revenue** - a language-targeting bug went undetected until a structured, channel-by-channel investigation surfaced it.
- **Reusable functions** (`conversion_rate`, `plotting_conv`, `lift`, `ab_segmentation`) turned a one-off analysis into a repeatable diagnostic toolkit.
- **Segment-level A/B testing** is essential - an overall "winning" variant can still underperform for specific audiences.

---

## Future Improvements

- Automate anomaly detection to flag conversion drops in near real-time
- Extend the A/B testing framework to support multi-variant (A/B/n) tests
- Build a lightweight dashboard (e.g., Streamlit) for non-technical stakeholders to explore channel performance
- Add confidence intervals alongside lift and p-values for more robust reporting

---

## Contact

Feel free to reach out if you'd like to discuss the analysis, methodology, or potential extensions to this project.
