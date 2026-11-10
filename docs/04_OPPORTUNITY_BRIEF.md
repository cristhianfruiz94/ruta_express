# Opportunity Brief: AI Claims Agent — Ruta Express

**Product Zero moment:** S2 Specify, into Gate 1 · **Owner:** Cristhian Ruiz, AI PdM · **Version:** 1.0, 2026-11-05 · **Decision Maker:** Laura Mendez (B:38)
**Sources:** B:n `00_Engagement_Brief.md` · T Lxx `01_SME_Interview_Transcript.md` · C row n `02_Claims_Sample.csv` · I:n `03_Infrastructure_Description.md` · M:n `04_Stakeholder_Messages.md` · **E:** `05_Claims_Export_Full.csv` (20 open claims, cited by claim_id; personal columns never used, see `PRIVACY_CHECK.md`) · **G:n** `06_Gate1_Request.md` · T1/T2 = Tier 1 and Tier 2 deliverables.
**Method:** AI-DLC. The AI planned and asked ten questions; the PdM decided each one before any drafting (`PROMPT_LOG.md`, Prompts 1–4). Every in-scope item and acceptance criterion carries a trace to the intent (I-1 … I-4).

## 1. Intent

> **Cut the human minutes Ruta Express spends per delivery claim — from about 14 today — by letting an AI agent resolve on its own the claims the client has already cleared for autonomy, and prepare every other claim for a fast human decision, without paying any refund a person should have approved.**

| Trace | Clause | Evidence |
|---|---|---|
| **I-1** | human minutes per claim, from ~14 | T L03; sample 13.5 min (T1 `DATA_APPENDIX.md` §B); Laura's "cut in half" (M:8) |
| **I-2** | resolve on its own what the client already cleared | late credits "the agent can do those alone" (T L29); wrong address when GPS clearly shows driver error (T L31) |
| **I-3** | prepare every other claim for a fast human decision | damaged and lost: "a person signs off" (T L30); same-day answer instead of three days (T L34) |
| **I-4** | no refund a person should have approved | "we pay some refunds we shouldn't" (T L35); fraud cases (T L16); CFO cap (M:13, G:22) |

## 2. Context — what, and for whom

**What.** An AI agent that triages each ReclamaYa claim (B:26), applies the client's written rules to the order (Atlas), tracking (Rastreo) and payment (PayCo) data, resolves the claims it is cleared to resolve, and prepares the rest — evidence, proposed decision, draft reply — for a person in Helpdesk (I:18–22). In Product Zero the four systems are simulated; the prototype never connects to anything (B:30).

**For whom.**
- 22 customer-service agents in Colombia and Mexico who handle ~4,800 claims a month by hand (B:13, T L02) — they get fewer claims and pre-worked cases.
- Supervisors in each country who approve high-value refunds (T L15, L22) — they get a queue with evidence instead of a ticket.
- Customers (shoppers and business accounts; the export shows an `account_type` field with `consumer` / `business`, E) — they get a same-day answer in formal "usted" (T L24, L34).
- Laura Mendez, who decides at Gate 1 and Gate 2 (B:38), and Andres Pardo, who caps automatic refunds until Gate 2 (M:13, G:22).

**Where the minutes are.** Late is the largest type (T L06; 48% of the sample, 35% of the export) but cheap to handle (7.6 min); damaged and lost are ~40% of claims and ~65% of the minutes (T1 §F). The export's late credits are small money: 700–1,200 COP or 8 MXN per claim (E: CLM-2001, 2010, 2017, 2020, 2003), all far below the CFO cap. The human minutes are the cost; the refunds are the risk.

## 3. Specifics — outcome metric and baseline, in numbers

| Metric | Baseline | Gate 2 target (to be proven on the simulated prototype, B:32) | Trace |
|---|---|---|---|
| **Primary — average human minutes per claim** | **~14 min** (SME, T L03); **13.5 min** in the sample, n = 23 (T1 §B) | **≤ 6.75 min** across the sample's claim mix (Laura's "half", M:8, as a stated ask) | I-1 |
| Share of claims resolved with no human action | 0% today (T L03) | **≥ 40%** from day one on the export's mix: 8 of 20 claims qualify under the day-one rules below (E); ~55% on the sample's mix. 80% is **not** a target (see §4) | I-2 |
| Time to first answer | "three days" (T L34; not measurable in the data, no timestamps in C or E) | Same day for 100% of triaged claims, including those handed to a person | I-3 |
| Refund error rate (paid when a person should have approved) | Unknown; "we pay some refunds we shouldn't" (T L35) | Zero automatic payments above the cap; no increase vs. human baseline on simulated cases | I-4 |

**Data caveat.** The export (E) is 20 *open* claims from one week with no handle times — richer in columns (shipping fee, GPS distance, photo timing, scan days, account type), not in rows. The ≥ 360-claim export requested in T2 (REVIEW.md, S3 conditions) is still open; the baseline above rests on the SME figure and the 23-claim sample (±3 min on the mean, ±20 pp on any share, T1 §D).

## 4. Constraints — what we will not do, and what we will not guarantee

| # | Constraint | Trace | Evidence |
|---|---|---|---|
| C1 | **No automatic refund or credit above 100,000 COP / 500 MXN until Gate 2.** Above the cap the agent prepares and a person approves. | I-4 | M:13, G:22 |
| C2 | **A person signs off every damaged and lost claim**, including rejections, "at least for now". The agent never rejects alone. | I-3, I-4 | T L30 |
| C3 | **Wrong-address: the agent acts alone only when the delivery scan is > 250 m from the address** (the 200 m rule plus a 50 m buffer for GPS error, PdM decision, Prompt 2). 200–250 m and ≤ 200 m go to a person. The buffer is a setting. | I-2 | T L20, L31; E: CLM-2014 at 210 m |
| C4 | **Photo window is a setting, default 48 h** (written policy, T L10, L12). Photos at 49–72 h are flagged "outside policy, inside current practice" (T L11) and a person decides; the agent never rejects. Policy owner: Mariana Ortiz. | I-3 | T L10–L12; E: CLM-2004 (60 h), 2011 (70 h), 2016 (100 h) |
| C5 | **Supervisor threshold is a setting per country, default 500,000 COP / 2,500 MXN** (written policy). The August 300,000 COP rule was temporary and the client settles it after Gate 1. | I-4 | T L15–L16; G:27; E: CLM-2013 (420,000 COP) sits between the two |
| C6 | **Mexico claims route to the Mexico queue and Mexico supervisors approve Mexico refunds.** | I-3 | T L22; B:9; I:10 |
| C7 | **Every customer reply is formal ("usted"), both countries.** | I-3 | T L24 |
| C8 | **No connection to any live client system**; Rastreo, PayCo, Atlas and Helpdesk are simulated from the systems description and the Atlas API docs. | — (engagement rule) | B:30; I:27, I:32–34; M:23 declined in T2 |
| C9 | **No personal data of end customers in any deliverable, prototype or synthetic dataset.** The export's name, phone, email and address columns are dropped on load. | — (engagement rule) | E; `PRIVACY_CHECK.md` |
| **We will not guarantee** | **80% (or any) automation rate**, "half by Q2", or a running cost per claim. Each is a stated ask or a stretch to be measured at Gate 2. | I-1, I-2 | B:32; M:8, M:28; G:32 |
| Out of scope | WhatsApp channel (M:18); voice calls (T L32); recovering damage costs from stores or the carrier (T1 H2, a client process decision). | — | T2 REVIEW.md |

## 5. Edge cases the agent must handle

Each case exists in the client's own data (E) or in the pack, so Gate 2 can test it with a synthetic twin.

| # | Edge case | Expected agent behaviour | Trace | Seen in |
|---|---|---|---|---|
| EC1 | Late claim but delivery ≤ 24 h after the window | No credit; reply explains the 24 h rule; closes alone | I-2 | E: CLM-2005 (10 h), 2015 (20 h) |
| EC2 | Late credit computed from shipping fee, not declared value | Credit = 10% × shipping fee; declared value ignored | I-2, I-4 | T L08; E: CLM-2001 → 900 COP, not 8,500 |
| EC3 | Photo uploaded at 49–72 h | Flag "outside 48 h policy, inside practice"; prepare for a person; no rejection | I-3 | E: CLM-2004, 2011 |
| EC4 | Photo uploaded > 72 h | Flag "outside policy and practice"; prepare proposed rejection; person signs | I-3, I-4 | E: CLM-2016 (100 h) |
| EC5 | Lost claim filed before 5 business days without scan | Not yet "lost"; reply with the date it becomes lost; re-check daily; no refund | I-3, I-4 | T L14; E: CLM-2007 (3 bd) |
| EC6 | Scan distance 200–250 m | Hand to a person with the map evidence; no redelivery scheduled alone | I-2 | C3; E: CLM-2014 (210 m) |
| EC7 | Scan distance ≤ 200 m (customer error) | Prepare "no refund" reply; person confirms (R10 not decided by the client) | I-3 | T L20; E: CLM-2009 (40 m) |
| EC8 | Refund above the CFO cap | Prepare; route to a person; never pay | I-4 | C1; E: 9 of 20 claims |
| EC9 | Refund above the supervisor threshold (and above the cap) | Route to a supervisor of the claim's country | I-4 | C5, C6; E: CLM-2004 (1.2 M COP), 2006 (3,200 MXN), 2012 |
| EC10 | Missing or non-standard claim type | Triage from text; if confidence is low, ask the customer one question; never guess a refund | I-3 | C row 17 (CLM-1015); C rows 5, 8, 16, 25 |
| EC11 | `account_type` blank | Treat as consumer for rules; flag for the priority rule | I-3 | E: CLM-2017 |
| EC12 | Business account | Priority in queue order (what "priority" means beyond order is open, T1 Q8) | I-3 | T L25; E: CLM-2004, 2010, 2012 |
| EC13 | Mexico claim | Mexico queue; MXN thresholds; Mexico supervisor | I-3 | C6; E: 6 of 20 |

## 6. Acceptance criteria — how anyone can check at Gate 2 that it is done

All criteria run on the simulated prototype with synthetic claims (B:23, B:30). "Pass" is binary.

| # | Criterion | How to check | Trace |
|---|---|---|---|
| AC1 | Average human minutes per claim ≤ 6.75 on a synthetic set that mirrors the sample's claim mix (48% late, 30% damaged, 9% lost, 9% wrong address, 4% untyped) | Time 20 agents' decisions on 40 prepared claims vs. 40 unprepared; report mean per type and overall | I-1 |
| AC2 | ≥ 40% of the synthetic set is closed with no human action, and every one of those is a late claim or a wrong-address claim > 250 m | Count closed-alone claims; inspect each one's type and distance | I-2 |
| AC3 | 0 automatic payments above 100,000 COP / 500 MXN; 0 damaged or lost claims closed without a human decision | Query the simulated PayCo log and the decision log | I-4, I-3 |
| AC4 | Every triaged claim has a customer reply drafted within the same session, in "usted", in the customer's country format | Read all replies; grep for "tú/tu/te" forms → must be zero | I-3 |
| AC5 | Each edge case EC1–EC13 has at least one synthetic twin and the agent's behaviour matches the table in §5 | Run the 13 twins; compare to expected behaviour | I-2, I-3, I-4 |
| AC6 | Settings (cap, supervisor threshold per country, photo window, GPS buffer) can be changed without code and the next claim obeys them | Change each setting, replay one claim | I-4 |
| AC7 | Refund error rate on the synthetic set is not above the human baseline measured on the same set | Two reviewers score each decision; compare agent-prepared vs. unassisted | I-4 |
| AC8 | No personal data appears in any screen, log or dataset of the prototype | Automated scan for the four PII column names and for phone/email patterns | C9 |
| AC9 | Per-claim cost telemetry exists: tokens in/out, AI calls, tool calls, by claim type | Export the telemetry for the 40 claims | — (memo, cost approach) |

**Open before S3 (owners):** ≥ 360-claim export with timestamps and handle time (Felipe Torres); supervisor threshold decision (Mariana Ortiz, after Gate 1, G:27); whether the CFO cap covers late credits (Andres Pardo / Laura Mendez); what "priority" means for business accounts (Jorge Salcedo, T L27).
