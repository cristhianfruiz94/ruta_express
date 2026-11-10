# Problem Framing — Ruta Express AI Claims Agent

**Product Zero moment:** S1 Understand · **Role:** AI PdM · **Author:** Cristhian Ruiz · **Date:** 2026-11-05 · **For:** Laura Mendez (Decision Maker) and the pod · **Detail:** [`02_DATA_APPENDIX.md`](02_DATA_APPENDIX.md)

**Sources:** `00_Engagement_Brief.md` (B:n), `01_SME_Interview_Transcript.md` (T Lxx), `02_Claims_Sample.csv` (C row n, header = row 1), `03_Infrastructure_Description.md` (I:n), `04_Stakeholder_Messages.md` (M:n). All sample figures come from `analyze_claims.py` on the cleaned sample (23 claims: duplicate C row 14 and test record C row 21 removed; labels normalized, C rows 5, 8, 16, 25).

**The problem in one line:** 22 agents work ~4,800 claims/month by hand across four systems, and volume is outgrowing the team (B:13, T L02–L03, I:18–22). The client frames it as a handle-time problem (M:8); the data says it is a **cost-of-process** problem: handle time alone explains only ~6.7 FTE of a 22-agent team, so most of the cost sits outside the minutes we can see.

## 1. Claims by type and country (sample, 1–12 Sep 2026)

| Type | n | Share of claims | Share of minutes | Avg min (sd) | CO / MX avg |
|---|---|---|---|---|---|
| Late delivery | 11 | 48% | 28% | 7.6 (1.1) | 7.6 / 7.7 |
| Damaged | 7 | 30% | 47% | 20.0 (2.2) | 19.4 / 21.5 |
| Lost | 2 | 9% | 18% | 26.5 (2.1) | 28 / 25 |
| Wrong address | 2 | 9% | 8% | 11.5 (0.7) | 12 / 11 |
| No type (CLM-1015, C row 17, open) | 1 | 4% | — | 10 | — |
| **Total** | **23** | **16 CO / 7 MX** | | **13.5 (7.0)** | **13.0 / 14.6** |

Three readings. (a) Within a type, handle time is tight (sd ≤ 2.2 min, CV ≤ 15%): the work is rule-driven, which is what an agent automates well. (b) Country makes no visible difference per type (CO vs MX within 2 min everywhere; n is too small to call a gap). (c) Late is half the volume but damaged + lost are two thirds of the minutes: the SME's "agent alone" scope (late + clear-GPS redelivery, T L29, L31) covers ~55% of claims but only ~32% of minutes.

**How much to trust this:** n = 23 against ~1,900 claims in the same 12 days. A share estimated from 23 claims carries ±20 pp at 95%; the mean handle time is 13.5 ± 3.0 min. To size the business case at ±5 pp we need **~360 claims**; at ±10 pp, ~95. The first S2 request to Felipe is a 1–3 month export (I:26) with shipping fee, business-account flag, re-open count and resolution timestamp.

## 2. Hypotheses to test before S2 fixes the spec

| # | Hypothesis | Evidence today | How we test it (S1–S2) |
|---|---|---|---|
| H1 | **Hidden cost.** The real cost per claim is 2–3× the logged handle time (waits, re-opens, customer back-and-forth, three-day answers, T L34). | 4,800 × 13.5 min = 1,078 h/mo = 6.7 FTE vs 22 agents (B:13). | Ask Mariana what else the team does; get timestamps (created → first reply → resolved) and re-open counts in the export. |
| H2 | **Damage liability is not Ruta's by default.** Damage splits between store packaging, carrier handling and already-damaged goods. Ruta does last mile with a third-party carrier (B:7, I:12), yet refunds the full declared value (T L10) with no recovery. | Notes: "box crushed", "caja mojada", "screen cracked" (C rows 3, 8, 5) suggest handling vs. packaging causes; no proof-of-condition at pickup or delivery exists in any system (I:7–14). | Ask Mariana/Felipe: is there a pickup scan or delivery photo? Does the carrier contract allow chargeback? Classify the 7 damaged notes with Ops. |
| H3 | **Proof of good delivery is the lever for automatic refunds.** If the driver captures a photo at handover, "damaged" claims become a comparison of two photos, which an agent can score and a human can approve fast. | Today the only photo is the customer's, 48–72 h later (T L10–L11; C row 16). | Prototype the two-photo flow with synthetic data in S3 (B:23, B:30); measure human approval time. |
| H4 | **Channel is a hypothesis, not a fact.** WhatsApp fits shoppers (M:18) but Ruta's paying customers are online stores (B:7); stores may need a corporate channel (portal/API) and claims may need to be filed per store, not per parcel. | No channel data in the pack; the "customer" who files (B:13) is never defined; business accounts undefined (T L25–L27). | Ask Camila for the channel mix of current claims and the store/shopper split; decide channel after that, not before. |

## 3. Outcome: hybrid OKR (baselines from the sample; targets are *to be proven at Gate 2*, B:32)

**Objective:** Resolve delivery claims faster and cheaper, with fewer wrong payouts, without connecting to any live system until the client decides to take it.

| KR | Area | Baseline (sample / SME) | Gate 2 target (prototype, simulated) |
|---|---|---|---|
| KR1 Avg human minutes per claim | Customer service | 13.5 min (T L03) | ≤ 6.75 min overall (Laura's "half", M:8), reached mainly through KR2 |
| KR2 Claims resolved with no human action | Customer service / autonomy | 0% (T L03) | ≥ 55% is what the SMEs already allow (T L29, L31); **80% is the stretch** and requires damaged claims to move to agent-alone (T L30, H3). We do not promise it (M:28, B:32). |
| KR3 Monthly agent hours on claims | Resourcing | ~1,078 h/mo logged; real figure under H1 | Measured, not targeted, until H1 is tested. The business-case number Andres asked for (cost per claim, M:13) comes from here. |
| KR4 Refund error rate (paid when it should not) | Finance / fraud | Unknown; "we pay some refunds we shouldn't" (T L35), fraud cases since August (T L16) | Baseline from last quarter's reversed/disputed refunds; target: no increase vs. human baseline at any autonomy level |
| KR5 Time to first answer | Customer experience | "three days" today (T L34) | Same day for every claim the agent triages, including those it hands to a person |
| KR6 Damage claims with liability attributed (store / carrier / Ruta) | Logistics / last mile | 0% — no field exists (C row 1) | Attribution captured on 100% of damaged claims in the prototype; recovery process is a client decision (H2) |
| KR7 Integration readiness | Technical | Simulated Atlas, Rastreo, PayCo, Helpdesk (B:30, I:7–14) | Prototype passes the same test cases against simulated and documented real interfaces (Atlas API docs, I:27); WhatsApp only if H4 holds |

## 4. Stakeholder asks

| Who | Ask | Label | Why |
|---|---|---|---|
| Laura Mendez, VP Ops, Decision Maker (B:38; M:8) | Cut handle time in half by Q2; AI approves refunds alone | **Problem** | The problem and KR1. The target is adopted as a Gate 2 target, not a promise (B:32); self-approval is bounded by M:13 and T L30. Handle time is the visible part of the cost (H1). |
| Andres Pardo, CFO (B:41; M:13) | No automatic refunds above 100,000 COP / 500 MXN until Gate 2; cost per claim | **Constraint** | Bounds KR2. Cost per claim is KR3's output; a credible number needs the fuller export and H1 answered. |
| Camila Reyes, Head of CX (B:42; M:18) | Run the prototype on WhatsApp | **Solution ask** | "Customers live on WhatsApp" is unsupported by data in the pack and ignores the stores (B:7). Reframed as H4; a channel decision after the channel mix is known, not a build decision now. |
| Felipe Torres, IT Director (B:43; M:23) | Connect to live Rastreo and PayCo | **Out of Product Zero scope** | "We never ask for system access" (B:30); his own document says the access cannot be shared (I:32–34). We take Atlas API docs and Helpdesk screenshots instead (I:27–28) and simulate the rest (KR7). |
| Wizeline Account Director, internal (M:28) | Promise 80% fully automated in the proposal | **Cannot promise** | No evidence for 80%: today's authorised scope is ~55% of claims and 32% of minutes. 80% is kept as the KR2 stretch, to be proven at Gate 2 (B:32). Internal message; must not reach the client (B:31). |

**Decisions needed from Laura this sprint:** (1) accept the hybrid OKR as the Gate 1/2 scorecard; (2) authorise the 1–3 month export; (3) name who owns the damage-liability question (H2), because it decides how far automatic refunds can go.
