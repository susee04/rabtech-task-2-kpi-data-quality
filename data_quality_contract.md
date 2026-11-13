# Retail Orders — Data Quality Contract

**Dataset:** Retail Orders  
**Decision owner:** Operations Manager (business accountability); Finance Manager (revenue and payment measures)  
**Technical steward:** Data Analyst  
**Source:** `data/retail-orders-raw.csv`  
**Review cadence:** Daily monitoring; weekly owner review

## 1. Purpose and scope
This contract defines minimum quality expectations for retail order data used in order-volume, sales, discount, payment, segment, and category reporting. The raw source must remain unchanged; corrections are made in a governed cleaned layer with an audit trail.

## 2. Quality dimensions and rules

| Dimension | Rule / executable check | Target | Failure threshold | Escalation / action |
|---|---|---:|---:|---|
| Completeness | `order_id`, `order_date`, `customer_segment`, `city`, `category`, `quantity`, `unit_price`, `payment_status` are populated | 100% | Any missing required value | Quarantine affected row; notify Operations Data Owner same business day |
| Uniqueness | `order_id` is unique | 100% | Any duplicate ID | Stop order-count/revenue publication; investigate source duplication |
| Validity — quantity | Numeric whole number greater than 0 | 100% | Any invalid value | Quarantine row; request source correction |
| Validity — unit price | Numeric INR amount greater than or equal to 0 | 100% | Any invalid value | Quarantine row; Finance review |
| Validity — discount | Numeric percentage from 0 through 100 | 100% | Any invalid value | Block net-revenue KPI; Finance approval required |
| Validity — categories | Segment, category, and payment status belong to approved values after trimming/case normalization | 100% | Any unknown value | Map only with owner approval; otherwise quarantine |
| Consistency | Dates parse consistently; normalized category/status values use approved labels | 100% | Any unparseable date or unmapped label | Correct upstream format and rerun profile |
| Freshness | New/updated order data arrives by agreed daily refresh deadline | By 09:00 local time daily (proposed) | Feed not refreshed by 09:00 | Alert Data Steward; escalate to Operations Manager after 1 hour |

## 3. KPI publication rules
- Publish order counts only after duplicate `order_id` records are resolved.
- Publish revenue only for valid records with `payment_status = Paid`.
- Calculate gross sales as `quantity × unit_price`.
- Calculate discount amount as `quantity × unit_price × discount_pct / 100`.
- Calculate net revenue as `quantity × unit_price × (1 − discount_pct / 100)`.
- A missing `discount_pct` must not silently become zero. Obtain Finance owner approval or exclude the affected record from discount-sensitive KPIs.
- Exclude invalid or quarantined records from certified KPIs and disclose the excluded row count.

## 4. Monitoring and escalation workflow
1. Run the profiling notebook after every data refresh.
2. If any hard-threshold rule fails, mark the dataset **NOT CERTIFIED** for affected KPIs.
3. Data Analyst records the failed rule, affected order IDs, timestamp, and evidence.
4. Data Steward investigates and assigns correction to the source owner.
5. Operations Manager owns resolution priority; Finance Manager approves payment/discount-related corrections.
6. Rerun the notebook and record pass/fail status before republishing KPIs.

## 5. Current sample observations
The supplied 12-row sample contains known quality issues including a duplicate order ID, missing order date, missing city, missing discount, inconsistent casing, an invalid date, a negative quantity, a discount above 100%, and a non-numeric quantity. These should be treated as exceptions, not silently repaired in the raw file.

## 6. Sign-off
**Business owner:** Operations Manager — approval pending  
**Finance owner:** Finance Manager — approval pending  
**Data steward:** Data Analyst — responsible for monitoring  
**Contract version:** 1.0
