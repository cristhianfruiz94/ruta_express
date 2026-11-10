# Data Appendix — Problem Framing (Tier 1, AI PdM)

**Author:** Cristhian Ruiz · **Date:** 2026-11-05 · **Script:** `analyze_claims.py` (run on `02_Claims_Sample.csv`; every figure below is script output, nothing is estimated by hand).

## A. Data cleaning

| Issue | Where | Action |
|---|---|---|
| Duplicate record | CLM-1012, C row 14 repeats C row 13 | Kept one |
| Test record | TEST-0001, C row 21, "TEST - ignore", handle time 0, value 1 COP | Removed |
| Label variants | `Damaged` (C row 5), `Dañado` (C row 8), `damaged ` with trailing space (C row 16), `Tarde` (C row 25) | Normalized to `damaged` / `late` |
| Missing type | CLM-1015, C row 17, status open, "cliente no especificó el tipo" | Kept as its own bucket; excluded from per-type stats |

Raw file: 25 rows, mean 12.8 min. Clean: 23 claims, mean 13.5 min. The dirty rows pull the raw average down by 0.7 min; the SME's "about 14" (T L03) matches the clean figure, not the raw one.

## B. Dispersion by type (clean, minutes)

| Type | n | Mean | SD | CV | Min–Max | 95% CI of mean |
|---|---|---|---|---|---|---|
| Late | 11 | 7.6 | 1.1 | 15% | 6–9 | 7.6 ± 0.8 |
| Damaged | 7 | 20.0 | 2.2 | 11% | 17–23 | 20.0 ± 2.0 |
| Lost | 2 | 26.5 | 2.1 | 8% | 25–28 | not meaningful (n=2) |
| Wrong address | 2 | 11.5 | 0.7 | 6% | 11–12 | not meaningful (n=2) |
| **All** | 23 | 13.5 | 7.0 | 52% | 6–28 | 13.5 ± 3.0 |

Reading: the overall CV of 52% is almost entirely *between* types, not within them. Once type is known, handle time is predictable to ±2 min. For an AI agent this means the type decides the work, so triage quality (CLM-1015 shows it is not always given) is the first thing to get right.

## C. By country (clean)

| Country | n | Mean | SD | Late | Damaged | Lost | Wrong addr |
|---|---|---|---|---|---|---|---|
| CO | 16 | 13.0 | 6.8 | 7.6 (n=8) | 19.4 (n=5) | 28 (n=1) | 12 (n=1) |
| MX | 7 | 14.6 | 7.8 | 7.7 (n=3) | 21.5 (n=2) | 25 (n=1) | 11 (n=1) |

The 1.6-min gap between countries is a mix effect (MX has proportionally more damaged claims in the sample), not a per-type difference. No evidence yet that the two queues (I:10) or the two legal entities (B:9, T L22) behave differently; the separate MX approval chain (T L22) will matter for KR5, not for handle time.

## D. Sample size

Population: ~4,800 claims/month (T L02) ≈ 1,900 in the 12-day window of the sample. Clean n = 23.

| Question | Result |
|---|---|
| Margin of error on any share (e.g. "48% late") at 95%, worst case p = 0.5, with finite-population correction | **± 20.4 pp** |
| n needed for ± 10 pp | 95 claims |
| n needed for ± 5 pp | 356 claims |
| n needed for ± 3 pp | 874 claims |
| 95% CI on mean handle time (t, df = 22) | 13.5 ± 3.0 min |

Formula: n₀ = z²·p(1−p)/e², then n = n₀ / (1 + (n₀−1)/N) with N = 4,800. Conclusion: the sample is fine for *understanding* (types, rule shape, dirty-data patterns) and not fine for *sizing* (shares, savings). The brief calls it "a sample of 25 claim records" (B:50) and Felipe said a fuller export may follow (I:26): ask for ≥ 360 claims covering 1–3 months, both countries, with `shipping_fee`, `business_account`, `created_ts`, `first_reply_ts`, `resolved_ts`, `reopen_count`, `refund_amount`, `refund_reversed`.

## E. Workload and resourcing (H1)

| Input | Value | Source |
|---|---|---|
| Claims / month | 4,800 | T L02 |
| Mean handle time | 13.5 min | sample (SME: ~14, T L03) |
| Logged handle time / month | 64,700 min = **1,078 h** | computed |
| FTE equivalent at 160 h/month | **6.7 FTE** | computed |
| Team size | 22 agents | B:13 |
| Share of team capacity explained by logged handle time | **~31%** | computed |

Either the 22 agents spend ~70% of their time on work that is not a claim (then "agents drowning", M:8, is not mainly a claims problem) or handle_time_min excludes most of the real effort: waiting on Rastreo, re-contacting customers, supervisor approvals (T L15–L16), re-opened tickets, and the three-day turnaround (T L34). Both readings change what the agent should optimise: minutes of human attention *and* elapsed time *and* number of touches per claim. This is why KR3 is "measured, not targeted" until Mariana answers.

## F. Where the minutes go (clean, typed, n = 22)

| Type | Share of claims | Share of minutes |
|---|---|---|
| Late | 50% | 28% |
| Damaged | 32% | 47% |
| Lost | 9% | 18% |
| Wrong address | 9% | 8% |

SME-authorised "agent alone" scope today (late credits, T L29; clear-GPS redelivery, T L31, taken as half of wrong-address): **~55% of claims, ~32% of minutes**. The Account Director's 80% (M:28) therefore cannot be reached on late claims alone; it needs damaged claims to leave human sign-off (T L30), which is exactly what H2/H3 (liability and proof of delivery) must settle first.

## G. Damaged claims, one by one (H2 raw material)

| Row | Claim | Country | Declared value | Min | Note | Possible cause (to classify with Ops) |
|---|---|---|---|---|---|---|
| 3 | CLM-1002 | CO | 240,000 COP | 18 | box crushed | handling (carrier) or packaging (store) |
| 5 | CLM-1004 | CO | 1,200,000 COP | 22 | screen cracked | handling or pre-existing; above both supervisor thresholds (T L15–L16) |
| 8 | CLM-1007 | CO | 310,000 COP | 19 | caja mojada | handling / exposure (carrier) |
| 11 | CLM-1010 | MX | 1,800 MXN | 20 | — | unknown |
| 16 | CLM-1014 | CO | 150,000 COP | 17 | photo sent day 3 | accepted outside the 48 h policy (T L10–L12) |
| 22 | CLM-1019 | CO | 410,000 COP | 21 | — | unknown |
| 26 | CLM-1023 | MX | 2,600 MXN | 23 | — | unknown |

No record says where the damage happened or whether the parcel was checked at pickup or handover. Three of seven notes point at handling or exposure during transport, which is the carrier partner's leg (I:12), yet the refund is paid by Ruta (T L10) with no recovery mentioned anywhere. That is the data gap behind H2 and KR6.
