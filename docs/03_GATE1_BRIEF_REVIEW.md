# Tier 2 Gate 1 Review: Opportunity Brief (AI draft v0.3)

**Status:** Final for submission · **Reviewer / artifact owner:** Cristhian Ruiz, AI PdM · **Date:** 2026-11-05
**Client Pack (source of truth):** `00_Engagement_Brief.md`, `01_SME_Interview_Transcript.md`, `02_Claims_Sample.csv`, `03_Infrastructure_Description.md`, `04_Stakeholder_Messages.md`. The draft is not a source. Tier 1 (`PROBLEM_FRAMING.md`, `DATA_APPENDIX.md`) is used for continuity, not as evidence.
**Draft:** `AI_PdM_Opportunity_Brief_AI_DRAFT.md`, 60 lines. Draft line numbers are file lines. Interview references give the file line and the transcript tag (L##). [D] = derived by script from `02_Claims_Sample.csv`, 23 valid rows (duplicate CLM-1012 at row 14 and TEST-0001 at row 21 excluded, labels normalized, as in Tier 1).
**Rule applied:** an item is an issue only if it directly contradicts the Client Pack. Everything else is "Odd but correct", however much I might have written it differently.

## Issues found (3)

| What is wrong | Where it is in the draft | Evidence from the client pack | What I changed |
|---|---|---|---|
| The average handle time is given as 20 minutes and attributed to the SME interview. The interview gives about 14 minutes overall; 20 minutes is the damaged-claims figure. | Section "Baseline", first bullet (line 21) | `01_SME_Interview_Transcript.md` line 13 (L03): "Average handle time is about 14 minutes per claim." Line 15 (L04): "The damaged ones take longer. More like 20 minutes." `02_Claims_Sample.csv` [D]: 13.5 min over 23 valid claims; 20.0 min for the 7 damaged claims (rows 3, 5, 8, 11, 16, 22, 26). | Line 21 now reads: "Average handle time is about 14 minutes per claim overall; damaged claims take about 20 minutes (SME interview, L03 and L04; claims sample: 13.5 and 20.0 minutes over 23 valid rows)." |
| The outcome metric promises 80% of claims fully automated with no human involvement. The pack requires a person on damaged and lost claims and on unclear wrong-address cases, caps automatic refunds, forbids promising unproven outcomes, and the 80% exists only in an internal message. | Section "Outcome metric" (line 27) | `01_SME_Interview_Transcript.md` line 67 (L30): "For damaged and lost, a person signs off." Line 69 (L31): if not clear, "a person decides." Line 65 (L29): only late credits run alone. `04_Stakeholder_Messages.md` line 13 (message 2, CFO): no automatic refunds above 100,000 COP / 500 MXN until Gate 2. `00_Engagement_Brief.md` line 32: "We do not promise outcomes we cannot prove." `04_Stakeholder_Messages.md` lines 27–28 (message 5, internal, "not visible to the client"): "Can we promise them 80%…". [D]: 9 of 23 valid claims are damaged or lost, so at most 14/23 = 61% could avoid a person even before the GPS and cap limits. | Line 27 now reads: "Average agent handle time per claim. Baseline about 14 minutes (SME interview, L03). The VP asks to cut it in half by Q2 (message 1): a stated ask, not a commitment. No automation rate is promised; a person signs off damaged and lost claims (L30)." This is KR1 of the Tier 1 OKR; the autonomy boundaries (L29, L31) and the CFO cap (message 2) stay in the pack and the BA spec, and the full OKR is in the PdM notes below. |
| Backlog row 1 proposes connecting the prototype to the live Rastreo feed and the PayCo refunds API, ranked first because "IT says the prototype is not credible without it". Product Zero never connects to client systems, and the client's own systems document says the access cannot be shared. | Section "Prioritized backlog", row 1 (line 49) | `00_Engagement_Brief.md` line 30: "We never ask for system access… The prototype behaves as if connected, without being connected to anything." `03_Infrastructure_Description.md` lines 32–34: no production access; Rastreo owned by the carrier partner, access cannot be shared; PayCo credentials not given. Line 27: the Atlas API documentation *is* available. `04_Stakeholder_Messages.md` line 23 (message 4): the IT Director's request is a request, not a rule. | Line 49 now reads: "Simulate the Rastreo feed and the PayCo refunds API with synthetic data, using the systems description and the Atlas API documentation as interface contracts (no live connection)", with the Why: "Product Zero never connects to client systems; the client cannot share Rastreo access or PayCo credentials." Row kept at position 1 (see Odd but correct: backlog order). |

## Odd but correct (14 items, left unchanged)

| Item in the draft | Why it looked odd | Why I left it alone |
|---|---|---|
| Header: "AI-generated draft v0.3. Not reviewed." (line 3) | After this review it is reviewed | Inherited draft text, not a factual claim about the client. The review and gate call below record the status. |
| "Customers who file claims through ReclamaYa" (line 12) next to "four systems: Helpdesk, Atlas, Rastreo and PayCo" (line 17) | Reads as if some unknown rule routes claims into four systems | Both are correct and describe a sequential flow, not a routing rule: `03_Infrastructure_Description.md` line 9 (ReclamaYa creates the Helpdesk ticket) and lines 18–22 (agent looks up Atlas, checks Rastreo, pays via PayCo, replies from Helpdesk). The classification the agent must do is claim *type* (`00_Engagement_Brief.md` line 26), a spec item for the BA, not an error here. |
| "Volume and handling time are growing faster than the team" (line 17), no source | No data in the pack supports it | Reported in the Brief as the client's own statement, `00_Engagement_Brief.md` line 13. |
| 4,800 claims; 22 agents; four systems (lines 11, 17) | Specific figures | Match L02 (line 11), `00_Engagement_Brief.md` line 13, `03_Infrastructure_Description.md` lines 18–22. |
| "Customers wait about three days for an answer (SME interview)" (line 22) | Cannot be tested: the sample has only `created_date` (row 1 header), no reply or resolution timestamp | Supported by L34 (line 75): "the same day instead of in three days." Untestable is not wrong. The fuller export (`03_Infrastructure_Description.md` line 26) should carry timestamps; noted for the PdM below. |
| "Late delivery is the largest claim type" (line 23) | Strong claim | L06 (line 19): "biggest by far"; [D] 11 of 23 valid claims (48%). |
| "Triage of all four claim types" (line 32) and "applies the client's written rules" (line 33) while written policy and practice differ (photo window 48 vs 72 h, L10–L12; supervisor threshold 500k vs 300k, L15–L16) | The draft ignores the conflicts | It says *written* rules, and the written policy is what the SME states (L12 line 31, L15 line 37). The Brief says the agent "triages" and "applies the client's rules" (`00_Engagement_Brief.md` line 26). Omissions are not contradictions. |
| WhatsApp out of scope (line 37) | Head of CX asked for it loudly (`04_Stakeholder_Messages.md` line 18) | Not contradicted: WhatsApp is a channel request; nothing in the problem (`00_Engagement_Brief.md` lines 13, 26) or the claims flow (`03_Infrastructure_Description.md` lines 9 and 18–22) needs it. Consistent with Tier 1 (N02 / H4: decide after the channel mix is known). |
| Voice calls out of scope (line 38) | — | L32 (line 71): "not now, that's a different project." |
| Constraints list only the CFO cap and "usted" (lines 42–43); human sign-off on damaged/lost (L30) and "no system access" (`00_Engagement_Brief.md` line 30) are absent | The two missing constraints are what make issues 2 and 3 possible | Both listed constraints are correct (`04_Stakeholder_Messages.md` line 13; L24 line 55). An omission is not a contradiction; the two constraints now appear in the corrected metric (line 27) and backlog row 1 (line 49), so the fixed brief is internally consistent without adding bullets. |
| Refund cap 100,000 COP / 500 MXN (line 42) vs. 500,000 COP / 2,500 MXN in the interview (L15) | Numbers differ | Two different rules: the CFO's cap on *automatic* refunds until Gate 2 (`04_Stakeholder_Messages.md` line 13) vs. the *supervisor-approval* threshold (L15, line 37). |
| Backlog order: late auto-credit at row 6 "Simple rule" (line 54), lost-parcel at row 2 (line 50), damaged at row 3 (line 51) | Late is the largest type (L06) and the only item cleared to run alone (L29, line 65); lost is 9% of the sample and needs sign-off (L30) | The pack gives no priority ranking, so an order cannot contradict it. My recommended order is in the PdM notes below, not in the artifact. |
| Lost-parcel flow "highest refund value per claim" (line 50) | No source states it | Not contradicted and consistent with the sample [D]: lost claims average 650,000 COP (row 18) and 3,200 MXN (row 7); damaged average 462,000 COP and 2,200 MXN (rows 3, 5, 8, 16, 22 / 11, 26). Lost = full refund of declared value (L14, line 35). Thin: one lost claim per currency. |
| Wrong-address check "Clear rule, easy to automate" (line 52) | Could read as full automation | L20 (line 47) gives the 200 m rule; L31 (line 69) limits autonomy to clear driver error. The item says "check", not "resolve". |
| Damaged assistant "about 20 minutes, longer than the average claim" (line 51); Mexico routing (line 53); business-account flag "definition pending" (line 55); cost per claim (line 56); Gate 1 recommendation placeholder (line 60) | Line 51 repeats the "20" from the wrong baseline; line 60 is empty | Line 51 is correct here: 20 min *is* the damaged time (L04, line 15), the clash came from line 21 (issue 1). Lines 53, 55, 56 match L22 (line 51), L25/L27 (lines 57, 61), `04_Stakeholder_Messages.md` line 13. Line 60 is the draft's instruction to me; the task places the gate call in this review, so the placeholder stays and the call is below. |

## Flags for the PdM

1. **Not reported on purpose:** the policy-vs-practice conflicts (photo window L10–L12; supervisor threshold L15–L16) and the undefined business account (L25–L27). Omissions, not contradictions; they are open questions Q1, Q2 and Q8 in the Tier 1 BA log and must be answered before S3, not fixed in this brief.
2. **Baseline "three days" is untestable from the sample.** Left as SME-attributed. The fuller export should carry `created_ts`, `first_reply_ts`, `resolved_ts` so Gate 2 can measure it.
3. **Triage is in scope (line 32) but not in the backlog.** The sample shows a missing type (row 17, CLM-1015) and four label variants (rows 5, 8, 16, 25). Not an error in the brief; a backlog item for the BA/PdM to add in S3 planning.
4. **The fixed brief changes lines 21, 27 and 49 only.** The other 57 lines are identical to the draft, verified with `diff`. Client company name and "Not reviewed" header are inherited from the draft and left.

## Gate Call

**Would you sign off this artifact for Gate 1 as drafted? No.**

- The outcome metric promises 80% full automation; the SMEs require a person on damaged and lost claims (L30) and the Brief forbids promising unproven outcomes (line 32).
- Backlog row 1 asks for live Rastreo and PayCo access, which Product Zero never requests (Brief line 30) and the client cannot give (Infrastructure lines 32–34).
- The baseline states 20 min average handle time; the interview says about 14 (L03) and the sample 13.5.

**The fixed version is ready for Gate 1.** My recommendation to the Decision Maker is **Go, scoped** (notes below).

## PdM notes for Gate 1 (continuity with Tier 1; not changes to the artifact)

**Recommendation: Go, scoped.** S3 builds what the pack already specifies: late-delivery credits agent-alone (L29), triage of the four types (Brief line 26), clear-GPS redelivery (L31), and human-approved damaged and lost flows (L30), all on simulated systems (Brief lines 23, 30). "Extend discovery" is not needed: the open items block *sizing* the benefit, not *building* the prototype.

**Conditions for S3 week 1:** (1) fuller export of ≥ 360 claims over 1–3 months with `shipping_fee`, `business_account`, timestamps, `reopen_count`, `refund_amount`, `refund_reversed` — with n = 23 any share carries ±20 pp (Tier 1 `DATA_APPENDIX.md` §D); (2) written answers on the supervisor threshold (L15 vs L16) and on whether the CFO cap covers late credits (message 2 vs L08); (3) a named owner for the business-account definition (L27).

**Suggested backlog order for S3 planning** (a PdM judgment, not a correction): late auto-credit (largest type, clear rule, agent-alone) → triage → wrong-address → damaged assistant → lost flow → Mexico routing → business-account flag → cost estimate, with the simulated systems (row 1) as the enabler built first.

**OKR carried from Tier 1 as the Gate 2 scorecard:** KR1 human minutes per claim 13.5 → ≤ 6.75 (the VP's ask); KR2 share resolved with no human action, ≥ 55% authorised today, 80% a stretch only if damaged claims leave sign-off; KR3 monthly agent hours on claims (measured; 4,800 × 13.5 min ≈ 6.7 FTE of a 22-agent team, so most cost sits outside logged minutes); KR4 refund error rate not above the human baseline (L35); KR5 same-day first answer (L34); KR6 damage liability attributed; KR7 integration readiness on simulated interfaces.

**First cost view for the CFO (pod assumptions, replaced by AI Engineering in backlog row 8):**

| Line | Assumption | Per claim | Per month (4,800) |
|---|---|---|---|
| Human handle time today | Fully loaded CS agent ≈ USD 900/month, 160 h → USD 0.094/min × 13.5 min | USD 1.27 | ≈ USD 6,100 |
| AI inference | ~20k input / 2k output tokens per claim at mid-tier LLM list prices; photo analysis on damaged | USD 0.09–0.12 | USD 450–580 |
| Orchestration, hosting, observability, evals | Fixed ≈ USD 1,500/month, shared | USD 0.31 | USD 1,500 |
| API usage (Helpdesk, Atlas, PayCo) | Within existing licences; Rastreo is the carrier's feed | ~0 | — |
| **AI run cost** | | **≈ USD 0.40–0.45** | **≈ USD 2,000** |
| Post-Gate 2 integration (real Atlas, Helpdesk, PayCo, Rastreo) | 8–12 engineering-weeks, one-off; outside Product Zero | — | — |

Reading: at the KR1 target the agent saves ≈ USD 3,000/month of *logged* handle time against ≈ USD 2,000/month to run. On logged minutes alone the case is thin; it rests on the hidden cost beyond logged minutes (KR3), the refunds paid that should not have been (KR4, L35) and the volume growth the team cannot hire for (Brief line 13). Those three are what the export must show at Gate 2.
