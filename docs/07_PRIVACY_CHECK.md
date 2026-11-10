# Privacy Check — Tier 3

**Name:** Cristhian Ruiz · **Discipline:** AI PdM · **Date:** 2026-11-05 · **Rule applied:** treat every client file as real data; personal data of end customers never enters a deliverable, a prototype or a synthetic dataset (Working Agreements, Tier 1 rule carried forward; task sheet line 65).

## 1. What personal data was found

| File | Field | Rows | Nature |
|---|---|---|---|
| `05_Claims_Export_Full.csv` | `customer_name` | 20 of 20 | Full names of individuals (and three company names for business accounts) |
| | `customer_phone` | 20 of 20 | Mobile and landline numbers, CO and MX formats |
| | `customer_email` | 20 of 20 | Personal and company e-mail addresses |
| | `delivery_address` | 20 of 20 | Street addresses with city |
| `02_Claims_Sample.csv` | — | — | No personal fields (checked in Tier 1) |
| Transcript, brief, messages | Names of client staff and stakeholders | — | Professional context, used by role and name as in Tiers 1–2 (PdM decision 9A); not end-customer data |

The e-mails use `example.com` domains and the phones use `555` ranges, which suggests fabricated values. I did not rely on that: the engagement rule is to treat the data as real, and a prototype that "looks realistic" because it carries real-looking people is the exact trap in the IT Director's message (G:8).

## 2. What I did with it

1. **Dropped on load.** `scripts/apply_rules.py` removes the four columns before any row is read (`PII` set at the top of the script) and never prints or writes them. Two derived files exist and neither has a personal column: `data/05_Claims_Export_Full_NO_PII.csv` (the export with the four columns removed, made at the planning step so every later analysis reads it) and `data/05_sanitized_rules_applied.csv` (claim_id, country, account type, claim type, amounts, rule outcome and flags).
2. **Cited by claim_id only.** The brief and memo reference export rows as `CLM-20xx`, never by a person, address or contact.
3. **Synthetic prototype.** The console uses six invented claims `SYN-3001…3006` with no name, phone, e-mail or address field in the data model at all; the customer is addressed as "Estimado(a) cliente" and the reply header reads "customer of SYN-300x". Amounts and timings follow *patterns* in the export (a 60 h photo, a 1.2 M COP business claim) without copying any row.
4. **Scanned every deliverable.** `scripts/pii_scan.py` loads the four personal columns from the export and searches each deliverable for any full value, any e-mail local part, and any phone- or e-mail-like pattern. Result on the final files: **0 hits** across `OPPORTUNITY_BRIEF.md`, `GATE1_DECISION_MEMO.md`, `PROTOTYPE_NOTES.md`, the prototype HTML, the sanitized CSV and the scripts.
5. **The export itself is not shipped.** `Tier_3/` contains no copy of `05_Claims_Export_Full.csv`; the sanitized derivative is the only data file.

## 3. What the check caught

- **False positives in the first scan (v1).** Splitting names into tokens flagged "Laura", "Jorge", "Felipe", "Torres" and "Ruiz" — stakeholder first names from the pack and my own surname. The scan was tightened to full values (v2) and the change is commented in the script. The lesson for S3: a PII scanner on real data needs an allow-list of staff names or it will block every memo.
- **A reply that read as sent before approval.** The prototype's customer reply for human-in-the-loop claims said "hemos aprobado" while the claim was still waiting; not a privacy issue, but a truthfulness one. Labelled "draft, sent only after approval".

## 4. What stays open for S3

- The ≥ 360-claim export we are requesting will carry the same four columns. The S3 ingestion must drop or hash them at the boundary and keep the raw file outside the repository; synthetic data generation must be seeded from distributions, not rows.
- Photos in damaged claims may show people, homes or documents. The S3 photo-assessment step needs a rule for that before any image leaves the simulated store.
- Business-account names (e.g. a store) are company data, not personal data, but they identify the client's customers; the same drop rule applies until the client says otherwise.
