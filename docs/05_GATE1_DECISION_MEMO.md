# Gate 1 Decision Memo — AI Claims Agent

**To:** Laura Mendez, VP Operations · **From:** Cristhian Ruiz, AI PdM, Wizeline pod · **Date:** 2026-11-05 · **Decision needed:** go / no-go / extend discovery (B:22)
**Intent (from the Opportunity Brief):** cut human minutes per claim from ~14 by letting the agent resolve alone what you have already cleared (I-2), prepare the rest for a fast human decision (I-3), and never pay a refund a person should have approved (I-4). Every line below traces to one of those.

## 1. Recommendation: **Go, scoped** — with three conditions for S3 week 1

**Why go.** The rules for the claims the agent will run alone are fully written and confirmed by your SMEs: the 24 h / 10% late credit (T L08, L29) and the 200 m wrong-address rule (T L20, L31). They cover the largest claim type (T L06) and the smallest money: every late credit in this week's export is 700–1,200 COP or 8 MXN (E), far below Andres's cap (G:22). S3 builds on synthetic data and simulated systems (B:23, B:30), so nothing we still lack from the client blocks *building*; it blocks *sizing the benefit*. [I-1, I-2]

**Why not extend.** Extending would wait for a fuller export and the supervisor-threshold decision (G:27). Both are needed to measure, neither to build the day-one scope. Product Zero validates the WHAT with a prototype, not with more discovery. [I-1]

**Conditions for S3 week 1.** (1) The ≥ 360-claim export with handle times and timestamps (Felipe; the 20-claim file is open claims only, E). (2) Your answer on whether the cap also covers late credits (Andres). (3) A named owner for the business-account priority rule (Jorge, T L27). [I-1, I-4]

## 2. What the agent resolves on its own from day one, and where a person stays in the loop

| Agent alone from day one | Person stays in the loop |
|---|---|
| **Late delivery:** credit 10% of the shipping fee when delivery is > 24 h after the window; "no credit" reply when ≤ 24 h (T L08, L29). | **Damaged:** agent checks the 48 h photo window, prepares evidence and a proposed amount; a person signs every decision, including rejections (T L30). Photos at 49–72 h are flagged, not rejected (T L11–L12). |
| **Wrong address, > 250 m:** driver error; agent schedules the free redelivery (T L20, L31; 200 m rule + 50 m GPS buffer, a setting). | **Lost:** agent computes the 5-business-day rule and prepares the full-refund case; a person signs (T L14, L30). |
| **Triage and customer reply** for every claim, in "usted" (B:26, T L24). | **Wrong address 200–250 m or ≤ 200 m:** person decides (T L31; R10 undecided). **Any refund above 100,000 COP / 500 MXN** (G:22); **above 500,000 COP / 2,500 MXN**, a supervisor of that country (T L15, L22). |

On this week's 20 open claims that is **8 resolved alone (40%)** and 12 prepared for a person — 9 of them only because the amount is above the cap (E). On the September sample it is ~55%. [I-2, I-3]

## 3. What we are confident we can show at Gate 2, and what we are not

**Confident.** Human minutes per claim on a synthetic set that mirrors your claim mix, against the 14-minute baseline (AC1). Zero automatic payments above the cap and zero damaged/lost closed without a person (AC3). All 13 edge cases from your own data behaving as specified — short-window photos, 210 m scans, 3-day "lost" claims, blank account types (AC5). Settings changed without code (AC6). Cost telemetry per claim (AC9). [I-1, I-3, I-4]

**Not confident.** That the result transfers to your real volume: the baseline rests on the SME's figure and 23 September claims (±3 min), and the new export has no handle times — hence condition (1). The refund error rate, because today's rate is unknown (T L35); we will show relative, not absolute, improvement (AC7). Whether same-day answers hold at 4,800 claims a month; the prototype proves the flow, not the throughput. And **80% automation**: the data supports ~40–55% with your current autonomy rules; 80% needs damaged claims to leave human sign-off, which is your decision after Gate 2, not ours to promise (B:32). [I-1, I-2, I-4]

## 4. How the pod will estimate running cost per claim (approach, not a promised number)

With AI Engineering, S3 instruments every claim the prototype handles and records, per claim and per claim type: tokens in and tokens out of each AI call; number of AI calls (triage, rule application, reply drafting, photo assessment); number of tool calls to the simulated Atlas, Rastreo, PayCo and Helpdesk; image tokens for damaged claims; retries. Per-type unit cost = Σ(tokens × model price) + Σ(tool calls × integration cost). Monthly range = Σ over types of (type share × unit cost × 4,800 claims) + fixed platform cost (hosting, orchestration, observability), with a low and a high bound from two model tiers and two context sizes. The number goes to Andres at Gate 2 with its assumptions attached. *Illustrative only, from Tier 2 assumptions: ≈ USD 0.40–0.45 per claim, ≈ USD 2,000 a month, against ≈ USD 1.27 of logged human time per claim — the business case rests on hidden minutes, avoided refund errors and growth, not on logged minutes alone.* [I-1, I-4]

## 5. Reply to the Account Director (internal, three lines)

> We can't put 80% in the Gate 1 deck: the client's own rules keep a person on damaged and lost claims, and the data supports 40–55% with those rules in place.
> What we can say is "the agent resolves late and clear wrong-address claims alone from day one, and Gate 2 measures the rest" — that is the number Laura will be able to defend.
> If she liked 80%, the honest path is to show her at Gate 2 what it would take to get there; promising it now breaks the one rule the brief asks us to keep (B:32).
