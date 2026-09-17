# D2C E-Commerce Funnel & Root Cause Analysis

## From 120,000 visitors to 8,181 purchases

High traffic can create the illusion of growth. This project began with that exact problem: the business was attracting **120,000 customer sessions**, but only **8,181 resulted in a purchase**. The overall conversion rate was **6.82%**.

The question was not simply, "How many people purchased?" It was:

> **Where are customers leaving the funnel, which segments leak the most, and what should the business investigate first?**

This project follows the customer journey from website visit to completed purchase, then breaks that journey down by acquisition channel, device, user type, campaign, region, and time.

The analysis was completed in Python using Pandas and NumPy. The final interactive dashboard was designed in Power BI so that stakeholders can select a segment and immediately see how its funnel performance changes.

---

## Executive Summary

The funnel shows a clear concentration of leakage:

| Funnel stage | Users | Retention from previous stage | Drop from previous stage |
|---|---:|---:|---:|
| Visited | 120,000 | Baseline | Baseline |
| Viewed product | 77,870 | 64.89% | 35.11% |
| Added to cart | 27,156 | 34.87% | 65.13% |
| Started checkout | 16,234 | 59.78% | 40.22% |
| Purchased | 8,181 | 50.39% | 49.61% |

The largest leak occurs between **product viewing and cart addition**. Only **34.87% of product viewers add an item to their cart**, making this the first investigation priority.

The most important segment patterns are:

- **Email** has the highest overall conversion at **7.31%**.
- **Paid Ads** has the lowest overall conversion at **6.72%**.
- **Email + Desktop** is the strongest cross-segment combination at **8.13%**.
- **Paid Ads + Returning users** is the weakest reported cross-segment combination at **6.41%**.
- Device, campaign, region, and month-to-month differences are relatively small compared with the main funnel leaks.

The data identifies strong investigation signals. It does not prove that a specific page design, campaign, price, payment method, or customer experience caused the drop because those explanatory fields are not present in the dataset.

---

## Business Problem

The business had substantial traffic but weak conversion. Looking only at the final conversion rate would hide the customer journey behind one number.

A 6.82% conversion rate could result from several different problems:

- Visitors may not reach a product page.
- Product viewers may not see enough value to add an item to cart.
- Cart users may abandon before checkout.
- Checkout users may fail to complete payment.
- A specific channel or audience may be bringing low-intent traffic.

The goal was to separate these possibilities using measurable funnel stages instead of relying on assumptions.

---

## Project Objectives

1. Clean and validate 120,000 customer-session records.
2. Measure the complete funnel from visit through purchase.
3. Calculate retention and drop-off at every stage.
4. Compare performance across five business dimensions.
5. Investigate important combinations such as channel by device.
6. Check whether performance changed across six months.
7. Turn the findings into an interactive Power BI dashboard.
8. Produce practical recommendations for the next investigation or experiment.

---

## Dataset

The dataset contains **120,000 records** covering **July through December 2025**.

Each row represents a customer session or funnel observation. The analysis uses:

- `date`
- `channel`
- `device`
- `user_type`
- `campaign_type`
- `region`
- `visited_website`
- `viewed_product`
- `added_to_cart`
- `checkout_started`
- `purchase_completed`
- `discount_applied`
- `revenue`

### Funnel definition

```text
Visited -> Viewed Product -> Added to Cart -> Checkout Started -> Purchase Completed
```

The funnel measures progression between stages. Retention is calculated against the immediately preceding stage, while overall conversion is calculated as purchases divided by visitors.

---

## Data Preparation

The initial exploration checked:

- Data types
- Missing values
- Duplicate records
- Category values
- Date formatting
- Logical funnel progression

The six funnel-status fields originally contained `Yes` and `No` values. They were converted to numeric indicators:

```text
Yes -> 1
No  -> 0
```

This allowed the funnel stages to be summed by group and used directly in Power BI measures.

Categorical fields remained as text. For example, `Email`, `Paid Ads`, `Desktop`, and `Returning` were kept as meaningful labels rather than replaced with arbitrary numeric codes.

The date column was converted using the actual source format, `%m/%d/%Y`.

### Data-quality corrections

Two issues were identified and fixed during preparation:

1. The source dates used month/day/year formatting, not day-month-year formatting.
2. The region value was `Non-Metro` with a hyphen. An earlier mapping used `Non Metro`, which created missing values. The mapping was corrected.

After cleaning, all **120,000 rows** had valid values for the required analysis fields, with no remaining nulls reported in the cleaned dataset.

### Funnel validation

The following logical checks were performed:

- No checkout without a cart.
- No purchase without checkout.
- No purchase with zero revenue.

These checks passed, so the stage counts could be interpreted as a logical progression rather than unrelated event totals.

---

## Overall Funnel Analysis

The final funnel is:

```text
120,000 Visited
 77,870 Viewed product       64.89% retained
 27,156 Added to cart        34.87% retained
 16,234 Started checkout     59.78% retained
  8,181 Purchased             50.39% retained
```

The biggest loss is the **View -> Cart** step:

- **50,714 users** viewed a product but did not add an item to cart.
- The stage drop-off was **65.13%**.

The next largest percentage drop is **Checkout -> Purchase**, where **49.61%** of checkout users did not complete a purchase.

This creates a prioritization order:

1. Product-view to cart behavior.
2. Checkout completion and payment experience.
3. Traffic quality and campaign-to-landing-page alignment.

---

## Segment Analysis

### Channel

| Channel | Visitors | Purchases | Conversion |
|---|---:|---:|---:|
| Email | 12,092 | 884 | 7.31% |
| Organic | 35,946 | 2,448 | 6.81% |
| Social | 18,071 | 1,230 | 6.81% |
| Paid Ads | 53,891 | 3,619 | 6.72% |

Email leads Paid Ads by **0.59 percentage points** on the overall channel table. Email also performs best at checkout completion, with **51.34%** retention from checkout to purchase.

Paid Ads performs weakest at the two earlier commercial-intent stages:

- View -> Cart: **34.44%**
- Cart -> Checkout: **59.39%**

The evidence suggests that Paid Ads should be investigated first for traffic quality, audience targeting, creative-message alignment, and landing-page consistency. These are hypotheses, not proven causes.

### Device

| Device | Visitors | Purchases | Conversion |
|---|---:|---:|---:|
| Desktop | 35,994 | 2,488 | 6.91% |
| Mobile | 84,006 | 5,693 | 6.78% |

Mobile represents the larger audience, but the conversion gap is only **0.13 percentage points**. Mobile should still receive checkout quality assurance because it has slightly lower retention, but the data does not support treating device as the primary bottleneck.

### User type

| User type | Visitors | Purchases | Conversion |
|---|---:|---:|---:|
| New | 77,969 | 5,398 | 6.92% |
| Returning | 42,031 | 2,783 | 6.62% |

Returning users trail new users by **0.30 percentage points**. They are weaker across the downstream funnel, especially at Cart -> Checkout and Checkout -> Purchase.

This points to a useful reactivation question: are returning customers receiving relevant products, offers, and messaging, or are they being treated like first-time visitors?

### Campaign type

| Campaign | Visitors | Purchases | Conversion |
|---|---:|---:|---:|
| Discount | 60,121 | 4,107 | 6.83% |
| Influencer | 23,815 | 1,626 | 6.83% |
| New Launch | 36,064 | 2,448 | 6.79% |

Campaign performance is effectively flat, with only a **0.04 percentage-point** spread between the highest and lowest conversion rates. Campaign type should not be presented as a meaningful differentiator without more granular campaign, creative, product, or cost data.

### Region

| Region | Visitors | Purchases | Conversion |
|---|---:|---:|---:|
| Metro | 72,014 | 4,909 | 6.82% |
| Non-Metro | 47,986 | 3,272 | 6.82% |

The region conversion rates are identical after rounding. Region is therefore useful as a dashboard filter, but it is not a current explanation for the conversion problem.

---

## Cross-Segment Analysis

Cross-segment cuts were used to find combinations that disappear in broad averages.

### Channel by device

- Email + Desktop: **8.13%**, highest reported combination.
- Email + Mobile: **6.97%**.
- Paid Ads + Mobile: **6.71%**, lowest reported combination in this table.

This suggests a practical test area: compare the Paid Ads mobile journey with the Email desktop journey, especially the product page and checkout path.

### Channel by user type

- Email + New: **7.60%**.
- Paid Ads + Returning: **6.41%**, lowest reported combination.

This combination deserves a targeted reactivation analysis. The next step would be to compare offer exposure, product relevance, time since last purchase, and landing-page experience.

### Device by campaign

- Desktop + New Launch: **7.44%**.
- Mobile + New Launch: **6.51%**.

The difference is directionally useful but does not establish that the campaign itself caused the gap.

---

## Root Cause Analysis

### Finding 1: Product-view to cart is the primary bottleneck

Only **34.87%** of product viewers add an item to cart. This is the largest stage loss in the funnel at **65.13%**.

Possible explanations include unclear value communication, price or shipping friction, weak product information, insufficient trust signals, or an ineffective add-to-cart call to action.

Recommended investigation:

- Review product-page engagement and CTA clicks.
- Compare price, discount, shipping, review, and availability information.
- Test clearer product benefits and stronger purchase prompts.
- Compare the experience for Paid Ads and Email traffic.

### Finding 2: Paid Ads is the weakest acquisition channel

Paid Ads converts at **6.72%**, compared with **7.31%** for Email. Paid Ads also has the lowest View -> Cart and Cart -> Checkout retention among channels.

Possible explanations include lower purchase intent, weak audience targeting, or a mismatch between the ad promise and landing-page experience.

Recommended investigation:

- Break Paid Ads down by campaign, audience, creative, and landing page.
- Compare click-to-product and product-to-cart behavior.
- Pause or redesign segments with low downstream retention.
- Evaluate conversion alongside acquisition cost and revenue, not conversion alone.

### Finding 3: Checkout is a major secondary leakage point

Only **50.39%** of checkout users complete a purchase. This means almost half of users who begin checkout do not finish.

Possible explanations include unexpected fees, payment failures, complicated forms, limited payment methods, or weak trust signals.

Recommended investigation:

- Measure payment error and failure rates.
- Review checkout abandonment by device and channel.
- Test simplified forms and clearer final pricing.
- Verify shipping, return, and security information at the final step.

### Finding 4: Returning users need separate treatment

Returning users convert at **6.62%**, below new users at **6.92%**. The weakest reported cross-segment is Paid Ads + Returning at **6.41%**.

Possible explanations include irrelevant reactivation messages, weak personalization, or returning users encountering a less suitable offer.

Recommended investigation:

- Segment returning users by recency and previous purchase history.
- Compare personalized and generic offers.
- Separate true remarketing audiences from broad Paid Ads targeting.
- Measure repeat purchase conversion independently from first purchase conversion.

---

## Power BI Dashboard

The dashboard is designed as an interactive investigation tool rather than a collection of disconnected charts.

### Main components

- KPI cards for visitors, purchases, revenue, and conversion rate.
- Funnel chart for Visited -> Viewed -> Cart -> Checkout -> Purchased.
- Channel conversion comparison.
- Stage-retention comparison.
- Monthly purchase and conversion trend.
- Drill-down or matrix view for channel, device, user type, campaign, and region.
- Slicers for all major segment dimensions.

### Interaction design

Selecting a bar, matrix row, or slicer value filters the other charts and KPI cards. All dashboard visuals use the same cleaned source table and measures so that the filtered totals remain consistent.

The funnel stages are represented with a stage table and a measure that switches between the five stage counts. Segment filters continue to work because the measures calculate directly from the filtered session table.

### Expected unfiltered validation values

- Visitors: **120,000**
- Viewed product: **77,870**
- Added to cart: **27,156**
- Started checkout: **16,234**
- Purchases: **8,181**
- Overall conversion: **6.82%**
- View -> Cart retention: **34.87%**
- Checkout -> Purchase retention: **50.39%**

---

## Analytical Approach

The analysis used counts and recomputed rates from numerators and denominators.

```text
Stage retention = current-stage users / previous-stage users
Overall conversion = purchased users / visited users
Stage drop-off = 100% - stage retention
```

Rates were rounded for presentation only. The dashboard should recalculate rates from filtered counts rather than average subgroup percentages.

The analysis deliberately distinguishes:

- **Observed evidence:** measured funnel counts and segment rates.
- **Interpretation:** what the pattern may indicate.
- **Hypothesis:** what should be tested using additional product, campaign, or payment data.

This prevents the dashboard from presenting correlation as confirmed causation.

---

## Limitations

The dataset is sufficient to locate funnel leakage, but it cannot fully prove why users dropped off.

Unavailable fields include:

- Product or category
- Price and shipping cost
- Ad creative and audience
- Landing page
- Product-page interaction events
- Payment failure reason
- Checkout error logs
- Customer lifetime value
- Acquisition cost
- Experiment or control-group assignment

The six-month period also shows stable conversion movement rather than a causal seasonal explanation. The monthly range was approximately **5.9% to 8.3%**, with no clear seasonal trend.

The next analytical version should join funnel data with product analytics, campaign metadata, payment logs, and cost data.

---

## Recommendations

### Immediate

1. Prioritize the View -> Cart stage.
2. Audit Paid Ads landing pages and message alignment.
3. Review checkout abandonment and payment failures.
4. Compare Paid Ads + Mobile and Paid Ads + Returning users separately.

### Next experiments

1. Test product-page CTA and value-proposition improvements.
2. Test clearer total pricing before checkout.
3. Test personalized returning-user offers.
4. Test Paid Ads audiences and landing pages independently.

### Success metrics

Track:

- View -> Cart retention
- Cart -> Checkout retention
- Checkout -> Purchase retention
- Overall conversion rate
- Revenue per visitor
- Paid Ads conversion and acquisition cost
- Returning-user repeat purchase rate

The first success metric should be **View -> Cart retention**, because it represents the largest observed loss and is closest to the primary bottleneck.

---

## Project Structure

```text
.
|-- d2c_marketing_funnel_data.csv   # Original source data
|-- cleaned_data.csv                # Validated and transformed data
|-- funnel_overall.csv              # Overall funnel counts and rates
|-- pivot_01_channel.csv            # Channel summary
|-- pivot_02_device.csv             # Device summary
|-- pivot_03_user_type.csv          # User-type summary
|-- pivot_04_campaign.csv           # Campaign summary
|-- pivot_05_region.csv             # Region summary
|-- pivot_06_monthly_trend.csv      # Monthly channel trend
|-- DATA_DICTIONARY.txt             # Field transformation reference
|-- analysis/                       # Python analysis scripts or notebooks
|-- powerbi/                        # Power BI report file and screenshots
`-- README.md                       # Project documentation
```

---

## Tools Used

- **Python**: data cleaning, validation, aggregation, and funnel analysis.
- **Pandas**: data manipulation and grouped summaries.
- **NumPy**: numeric calculations and missing-value handling.
- **Power BI**: interactive dashboard, KPI cards, funnel chart, slicers, and cross-filtering.
- **GitHub**: version control and portfolio presentation.

---

## Final Takeaway

The project started with a traffic-versus-conversion question and ended with a prioritized investigation plan.

The traffic problem is not evenly distributed across the funnel. The clearest loss happens after customers view a product but before they add anything to their cart. Paid Ads is the weakest broad channel, while Email provides a useful benchmark for stronger performance. Checkout completion is a second major concern, and returning users deserve a separate reactivation strategy.

The dashboard turns these conclusions into an interactive workflow: select a segment, inspect its funnel, compare retention by stage, and identify where the next business experiment should focus.

The most important conclusion is therefore not simply that conversion is **6.82%**. It is that the business now knows **which stage to investigate first, which segments provide useful contrasts, and which additional data is needed to confirm the cause**.
#   D 2 C - E - C o m m e r c e - F u n n e l - R o o t - C a u s e - A n a l y s i s  
 