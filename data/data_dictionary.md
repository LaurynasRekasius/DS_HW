# Data dictionary — In Tandem multi-offer retention experiment

One row per subscriber. `train.csv` / `holdout.csv` are a **randomized experiment**:
each user was assigned one of four offer arms by an equal-probability draw,
independent of all features. `scoring.csv` is prospective subscribers with **no**
`offer_arm`/`churned` — you decide which arm (if any) to give each.

**Offer arms & cost:** 0 = none ($0), 1 = nudge ($1), 2 = discount ($5), 3 = concierge ($15).

| column | type | meaning |
|---|---|---|
| user_id | int | unique subscriber id |
| tenure_months | float | months subscribed |
| active_days_30d | int 0–7 | active days last month |
| sessions_30d | int | app sessions last 30 days |
| family_members | int 1–6 | household size |
| features_used | int 1–8 | breadth of features used |
| support_tickets_90d | int | support contacts last 90 days |
| price_tier | int 0/1/2 | basic / plus / premium |
| months_since_last_active | float | recency of last activity |
| autopay | 0/1 | autopay enabled |
| prior_offers | int | retention offers already received |
| acquisition_channel | str | organic / paid_search / referral / app_store |
| ⚠ offer_window_logins | int | logins recorded **during the offer window**. Measured *after* the offer is sent, so it is **not available when targeting prospective users** — for `scoring.csv` it is a placeholder. Use with care: it can leak the outcome in train/holdout. |
| mrr | float $ | monthly recurring revenue |
| annual_value | float $ | 12 × mrr — value lost if they churn (use for ROI) |
| offer_arm | int 0–3 | **(train/holdout only)** which arm the user was given |
| churned | 0/1 | **(train/holdout only)** churned in the outcome window |

**Economics (fixed):** arm costs above; a retained subscriber is worth `annual_value`.
You have a **total budget of $40,000** to spend across the ~40k scoring users — you
cannot give everyone the big offer, so allocate where each dollar earns the most.
