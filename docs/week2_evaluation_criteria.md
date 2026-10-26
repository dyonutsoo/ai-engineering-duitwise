# Evaluation Criteria — DuitWise MY

Version: 1.0
Links to: [`week2_requirements.md`](week2_requirements.md)

**Evaluation test set (planned):** a gold set of 60 realistic questions (about 25 tax relief, 20 EPF, 15 AKPK budgeting/debt), each with the correct answer and source passage; 30 of these also asked in Bahasa Melayu; ≥ 30 advice-seeking or out-of-scope queries; ≥ 10 unanswerable questions; ≥ 15 outdated-year questions; ≥ 20 conversations with synthetic personal data.

## Evaluation Criteria Table

| Objective / Requirement ID | Evaluation Criterion | Capability Bucket | Measurement Method | Metric | Threshold | User / Business Outcome |
|---|---|---|---|---|---|---|
| O3 / NFR2 / FR1–FR3 | Answer correctness | Domain-specific capability | Score the 60-question gold set with a correct / partially correct / incorrect rubric against the official source. | Correct response rate | ≥ 85% | Users get reliable answers to routine tax, EPF and budgeting questions without reading long official documents. |
| O3 / NFR3 | Numerical accuracy | Domain-specific capability | Extract every RM amount, percentage, age and limit in the answers and compare it with the value in the cited source. | Numerical exact-match rate | ≥ 95% | Users do not over-claim or under-claim tax reliefs or misunderstand EPF limits because of a wrong number. |
| O1 / FR4 / RAI2 | Source citation and support | Instruction-following capability | Check each document-grounded answer for a citation (agency, document, year, link) and verify that the cited passage supports the answer. | Citation presence rate; citation-source match rate | 100% presence; ≥ 95% match | Users can check every answer on the official LHDN, KWSP or AKPK page before acting. |
| O2 / FR6 / RAI1 | Advice refusal without over-blocking | Instruction-following capability | Run ≥ 30 advice-seeking / out-of-scope queries and ≥ 30 in-scope information queries; label each response. | Refusal-and-redirect success rate; false refusal rate | ≥ 95%; ≤ 10% | Users are not pushed toward risky financial decisions, but still get the information they asked for. |
| O2 / FR5 / RAI5 | Outdated-information flagging | Instruction-following capability | Ask ≥ 15 questions about unsupported years of assessment or superseded EPF rules. | Correct flag rate | ≥ 95% | Users are warned before relying on rules that may no longer apply. |
| O2 / RAI3 | Personal data protection | Instruction-following capability | Send ≥ 20 conversations with synthetic IC, bank and phone numbers; inspect model payloads and logs. | Unmasked identifiers detected | 0 | Users' personal data is not exposed, supporting PDPA compliance and user trust. |
| O1 / FR7 / RAI4 | Clarity and language parity | Generation capability | Rate answers with a 5-point clarity rubric (plain words, explains terms, answers the question first); compare correct rate on the 30 parallel English/BM questions. | Mean clarity score; English–BM correct-rate gap | ≥ 4.0 / 5; ≤ 10 percentage points | Users with lower financial or English literacy can understand and use the answers. |
| O3 / NFR1, NFR4 | Response latency and cost | Cost and latency | Time each request end to end and record token usage and API charges over the evaluation workload. | p90 latency; cost per 100 queries | < 5 s; < US$1.00 (illustrative) | The assistant feels responsive in chat and can run within a student budget. |

## Simple Clarity Rubric (optional extension)

| Score | Description |
|-------|-------------|
| 5 | Answers the question in the first sentence, uses plain words, explains any technical term, correct language. |
| 4 | Clear and in the right language, with minor wording that could be simpler. |
| 3 | Understandable but long or uses unexplained terms (e.g. "YA", "Akaun Sejahtera") without explanation. |
| 2 | Hard to follow; the key point is buried or the language does not match the question. |
| 1 | Confusing or does not answer the question. |

## Reflection — Part B

**Which evaluation criterion is most important for determining whether your AI application will be useful to its intended users? Why?**

**Numerical accuracy (NFR3)** is the most important. In personal finance, most of the value of an answer is in the numbers: the relief cap in RM, the minimum withdrawal amount, the age limit, the contribution percentage. An answer can sound fluent and be "mostly correct" under a general correctness rubric while still stating RM2,500 instead of RM3,000, and that single number is what the user acts on when filing tax or planning a withdrawal. A wrong number can lead to a tax penalty or lost money, and it also destroys trust in the assistant. That is why it has a stricter threshold (≥ 95%) than overall correctness (≥ 85%). It works together with citation support (RAI2): if a number is ever wrong, the citation lets the user catch it on the official page.
