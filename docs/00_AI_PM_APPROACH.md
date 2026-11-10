# The AI Product Manager Approach

How this case was worked from an AI Product Manager's seat: what the role owns, the decisions it made, and why. The other documents in this folder are the evidence; this one is the reasoning that connects them.

**Case:** Ruta Express, a last-mile delivery company in Colombia and Mexico (fictional). 22 agents handle ~4,800 delivery claims a month by hand across four systems. The idea on the table: an AI claims agent.
**Role:** AI Product Manager. Owns the *what*, the outcome, and what will and will not be promised. Engineering owns the *how*.
**Method:** AI-Driven Development Lifecycle (AI-DLC) as the working frame: the AI plans and drafts, people decide at explicit checkpoints, and every artefact traces back to a stated intent.

---

## 1. Start from the problem, not the request

Every stakeholder arrived with an answer: halve handle time by Q2 and let the AI approve refunds (VP Operations), run it on WhatsApp (Head of CX), connect to live tracking and payments (IT Director), promise 80% automation (an internal account lead). The first job was to sort these into **problem, constraint, solution ask, out of scope and cannot promise**, so the team would not build the loudest request.

The data then reframed the problem. The claims sample gives ~13.5 minutes per claim, which matches the SME's "about 14". But 4,800 claims × 13.5 minutes is only ~1,078 hours a month, about **6.7 full-time agents against a team of 22**. Logged handle time explains roughly a third of the team's capacity. The real cost is in waits, re-contacts, approvals and three-day turnarounds that the data does not capture. That turned "cut handle time" into a **cost-of-process** problem, and it is why the outcome metric travels with guardrails.
→ [`01_PROBLEM_FRAMING.md`](01_PROBLEM_FRAMING.md), [`02_DATA_APPENDIX.md`](02_DATA_APPENDIX.md)

## 2. Treat the data as evidence, with its limits stated

- **Clean before counting.** The 25-row sample had a duplicate, a test record and five spellings of the same claim types. Raw average: 12.8 min. Clean: 13.5 min. The SME's figure matches the clean one.
- **Measure dispersion, not just means.** Within a claim type the standard deviation is ≤ 2.2 minutes; the variation is *between* types. For an agent, that means classifying the claim correctly decides most of the work.
- **Say how much to trust it.** With 23 claims, any share carries ±20 percentage points. Sizing a business case at ±5 points needs ~360 claims. The sample is good for understanding the rules and bad for sizing the benefit, and the documents say so.

## 3. One outcome, with guardrails

**Primary metric: average human minutes per claim, from ~14 to ≤ 6.75** (the VP's "half", adopted as a target to prove, not a promise).

A minutes-only metric can be gamed by paying faster, which is the wrong win. So it is paired with:

| Guardrail | Why |
|---|---|
| Share of claims resolved with no human action | Shows how the minutes were saved; ~40–55% is what the client's own rules allow today |
| Refund error rate | "We pay some refunds we shouldn't" (Ops supervisor); fraud cases since August |
| Same-day first answer | The SME's own definition of success |
| Monthly agent hours on claims | Tests the hidden-cost hypothesis before setting a target |

## 4. Design autonomy, don't assume it

The most important product decision is **what the agent does alone and where a person stays in the loop**. The client's SMEs had already drawn the line; the PM's job was to make it explicit and testable.

| Agent alone from day one | A person decides |
|---|---|
| Late delivery: credit 10% of the shipping fee when > 24 h late, or a "no credit" reply when ≤ 24 h | Every damaged and lost claim, including rejections |
| Wrong address when the delivery scan is > 250 m away (200 m rule + 50 m buffer for GPS error) | Wrong address at ≤ 250 m |
| Triage and the customer reply for every claim | Any refund above the CFO's cap (100,000 COP / 500 MXN); above 500,000 COP / 2,500 MXN, a supervisor of that country |

On the client's 20 open claims, that is 8 resolved alone (40%). The late credits are small money (700–1,200 COP), so **the agent-alone scope is almost free of financial risk**; the risk lives in the human-in-the-loop scope, which is where the controls go.

**Why not 80%?** Reaching it requires damaged claims to leave human sign-off, which the client has not authorised. The documents keep 80% as a stretch to be earned with evidence at Gate 2, never as a commitment.

## 5. Conflicts become settings with an owner

Policy and practice disagreed in three places: the photo window for damaged claims (written 48 h, practised 72 h), the supervisor threshold (written 500k, a temporary 300k since August), and what counts as a "clear" GPS reading. The PM does not settle the client's policy. Each conflict became a **setting with a default and a named owner**, so the build is not blocked and the client decides without a code change. The prototype makes this visible: lowering the cap moves claims between "resolved alone" and "needs a decision" instantly.

## 6. Prototype to learn, not to impress

The prototype is throwaway, built in under an hour with synthetic data. It exists to test the *what*. It taught five things (see the notes), the main one being: **the agent's output is a case file, not a verdict.** Showing the rule-by-rule reasoning next to a proposed decision turns a 20-minute investigation into a 2-minute check. Most of the value sits in preparing claims for people, not in the share the agent closes alone.

The notes are equally explicit about what it does **not** prove: no handle-time measurement, no triage from free text, no photo analysis, no volume.
→ [`06_PROTOTYPE_NOTES.md`](06_PROTOTYPE_NOTES.md)

## 7. Write the brief so an agent and a pod can build from it

The Opportunity Brief opens with a one-sentence intent split into four clauses (I-1 to I-4). Every constraint, edge case and acceptance criterion carries a trace to one of them; anything that traces to nothing is cut. Edge cases come from the client's own data (a photo at 60 hours, a scan at 210 m, a "lost" claim filed after 3 business days, a blank account type), so each one can be tested with a synthetic twin at Gate 2.
→ [`04_OPPORTUNITY_BRIEF.md`](04_OPPORTUNITY_BRIEF.md)

## 8. Review AI drafts like a gatekeeper

Part of the case was reviewing an AI-drafted brief before it reached the decision maker. The rule applied: **an issue is a direct contradiction of the client's evidence**; things that are merely odd or written differently stay as they are. Three real errors were fixed (a baseline quoting the damaged-claim time as the average, an 80% metric taken from an internal message, a backlog item asking for live system access), each changing only its own line. Fourteen odd-but-correct items were documented and left alone.
→ [`03_GATE1_BRIEF_REVIEW.md`](03_GATE1_BRIEF_REVIEW.md)

## 9. Cost: an approach, not a number

The CFO asked for running cost per claim. The honest answer at Gate 1 is the method: instrument every claim for tokens in and out, AI calls, tool calls and image tokens by claim type; multiply by model prices; weight by the claim mix × 4,800 claims; add fixed platform cost; report a low–high range. An illustrative range (~USD 0.40–0.45 per claim vs ~USD 1.27 of logged human time) is shown, labelled as an assumption. The conclusion it supports: on logged minutes alone the case is thin; it rests on hidden minutes, avoided refund errors and growth the team cannot hire for.
→ [`05_GATE1_DECISION_MEMO.md`](05_GATE1_DECISION_MEMO.md)

## 10. Privacy is a product requirement

The client's fuller export arrived with names, phones, e-mails and addresses, with a note that it "has everything, so you can make the prototype realistic". Realistic is not the same as real. The personal columns were dropped at load time, never printed, and every deliverable was scanned against the source before sharing. The prototype uses invented claims with no personal fields in its data model.
→ [`07_PRIVACY_CHECK.md`](07_PRIVACY_CHECK.md)

## 11. The decision

**Go, scoped**, with three conditions: a fuller export to size the baseline, a written answer on whether the CFO's cap covers late credits, and an owner for the business-account priority rule. The rules for the day-one scope are fully specified, and the prototype runs on synthetic data, so what is missing blocks *sizing the benefit*, not *building*.
