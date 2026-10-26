# Requirements Specification — DuitWise MY

Version: 1.0
Project: DuitWise MY — AI Personal Finance Information Assistant

---

## Section 0 — Project Framing & Application Scope Checkpoint

| Framing element | Details |
|-----------------|---------|
| **Problem Evidence** | **(1)** Bank Negara Malaysia's Financial Capability and Inclusion Survey found that **61% of Malaysians would have difficulty coming up with RM1,000 in an emergency** (BNM Deputy Governor, 17 Nov 2024, reported by Malay Mail: https://www.malaymail.com/news/money/2024/11/17/malaysians-financial-literacy-surpasses-global-average-but-61pc-still-lack-emergency-savings-says-bnm-deputy-governor/157181). **(2)** Only about **36% of active EPF members aged 18–55 had reached their Basic Savings level** as at October 2024 (New Straits Times: https://www.nst.com.my/business/corporate/2024/12/1147217/epf-only-36pct-members-meet-current-basic-savings-benchmark). Together these show weak day-to-day money management and retirement readiness, which basic, reliable financial information can help address. |
| **Existing Approaches** | **(1) Official agency portals** — LHDN tax relief pages, KWSP member guides and i-Akaun, AKPK financial education modules. *Already address:* authoritative, up-to-date rules from the agency itself. **(2) Comparison sites** — RinggitPlus, iMoney. *Already address:* readable articles, calculators and product comparisons for general readers. **(3) FinanceBench** (Islam et al., 2023, arXiv:2311.11944). *Already addresses:* a benchmark showing how well LLMs answer financial questions from documents; GPT-4-Turbo with retrieval answered incorrectly or refused 81% of sampled questions, showing that grounding and evaluation are essential. |
| **Project / Engineering Gap** | Users cannot ask one plain-language question across tax, EPF and budgeting rules and get an answer that is **grounded in the current official document, cites the source and year, and stays within an information-only boundary**. Portals are fragmented and technical, comparison sites are product-focused, and general-purpose chatbots are not reliably grounded in Malaysian rules (risk of wrong amounts and outdated reliefs). This is a practical engineering gap in grounding, source traceability and scope control, not a claim of scientific novelty. |
| **Objective O1** | To **design** a document-grounded personal finance information assistant based on publicly available official documents from LHDN, KWSP and AKPK. |
| **Objective O2** | To **develop** the assistant with source and year citations, and with controls that refuse investment or product advice, protect personal data, and flag possibly outdated rules. |
| **Objective O3** | To **evaluate** the assistant on answer correctness, numerical accuracy, citation support, advice refusal, latency and cost using a defined test set of realistic user questions. |
| **Intended Users** | Young working adults in Malaysia (about 20–35 years old), such as fresh graduates and first-time taxpayers. Secondary: final-year university students preparing to enter the workforce. |
| **Supported Tasks** | **T1** Explain individual income tax reliefs for a stated year of assessment (categories, caps in RM, conditions). **T2** Explain EPF accounts (Akaun Persaraan, Sejahtera, Fleksibel), contribution split and withdrawal types, eligibility and how to apply. **T3** Explain basic budgeting and debt-management concepts and AKPK's free services. **T4** Point users to the official source and next step (e.g. LHDN e-Filing, KWSP i-Akaun, AKPK advisory). |
| **Information / Knowledge Domain** | Malaysian personal finance rules published by official bodies: LHDN individual tax reliefs for resident individuals (supported years of assessment only), KWSP member account and withdrawal rules for members below 55, and AKPK budgeting and debt-management education materials. Questions in English or Bahasa Melayu. |
| **Operating Context** | A web-based chat assistant used on phone or laptop, without login, any time of day. Usage is expected to peak during the tax filing season (around March–May). Each conversation is self-service; users verify and act on the official portals themselves. |
| **Important Constraints** | Only publicly available documents may be used and they must not be redistributed in the public repo. Rules change every year (new reliefs each year of assessment; EPF restructured in May 2024), so documents carry a year/date. Many key facts sit in PDF tables. Student budget: low-cost or free-tier model API. One-semester timeline. No access to users' i-Akaun, e-Filing or bank accounts. Must respect Malaysia's Personal Data Protection Act 2010 (PDPA). |
| **Explicit Out-of-Scope Tasks** | Recommending investments, savings plans or financial products (stocks, crypto, unit trusts, insurance, loans, credit cards). Calculating a user's exact tax payable or filing tax on their behalf. Deciding whether a user should withdraw EPF savings. Accessing or changing any personal account. Business/company tax, non-resident tax, tax disputes or audits. Negotiating debt or acting as a licensed financial adviser. |
| **Evaluation Focus** | Does DuitWise MY give **correct, numerically accurate and properly cited** answers to supported finance questions, **refuse advice-seeking and out-of-scope requests** with a useful redirect, and do so **fast and cheaply enough** for self-service use? |

**Traceability check:** O1 → T1–T4 and the knowledge domain → FR1–FR5, FR7–FR8 → correctness, numerical-accuracy and citation criteria. O2 → out-of-scope list and constraints → FR4–FR6, RAI1–RAI5 → refusal, privacy and currency criteria. O3 → evaluation focus → NFR1–NFR6 → evaluation criteria table (see `week2_evaluation_criteria.md`).

---

## Section 1 — Functional Requirements

| # | Requirement — The system SHALL | Scope Boundary — SHALL NOT (where relevant) |
|---|-------------------------------|---------------------------------------------|
| FR1 | Answer questions about individual income tax relief categories, maximum amounts (RM) and eligibility conditions for each supported year of assessment, based on LHDN documents. | Invent relief categories, amounts or conditions that are not in the source documents, or calculate the user's exact tax payable. |
| FR2 | Answer questions about EPF account types, contribution distribution, withdrawal types, eligibility conditions and application channels, based on KWSP documents. | Decide or recommend whether a user should make a withdrawal. |
| FR3 | Explain budgeting and debt-management concepts and AKPK's free services, based on AKPK materials. | Recommend specific loans, credit cards or other financial products, or negotiate debts on the user's behalf. |
| FR4 | Provide a source reference (agency, document title, year of assessment or publication date, and link) for every answer based on official documents. | Present information as if it came from an official source when it is not supported by a retrieved document. |
| FR5 | State the year of assessment or document date that an answer applies to, and advise the user to confirm on the official portal when the rule is time-sensitive or the requested year is not covered. | Apply rules from one year to another year without saying so. |
| FR6 | Decline requests for investment advice, product recommendations and other out-of-scope tasks, and redirect the user to an appropriate service (AKPK free advisory, a licensed adviser, LHDN or KWSP contact channels). | Recommend or rank stocks, cryptocurrency, unit trusts, insurance, loans or other financial products. |
| FR7 | Reply in the language of the user's question (English or Bahasa Melayu) using plain, non-technical wording. | — |
| FR8 | Tell the user when it cannot find sufficient supporting information in the official documents, instead of guessing. | Fill gaps with general model knowledge presented as fact. |

---

## Section 2 — Non-Functional Requirements

| # | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |
|---|-------------------|-------------|--------------------|--------|-----------|
| NFR1 | Performance | Interactive answers shall be returned within an acceptable waiting time for chat use. | Measure end-to-end latency (question sent → full answer shown) on the 60-question evaluation workload. | p90 latency | < 5,000 ms |
| NFR2 | Correctness | Answers shall be factually consistent with the official source documents. | Score a curated 60-question gold set (tax, EPF, AKPK; English and BM) with a correct / partially correct / incorrect rubric. | Correct response rate (%) | ≥ 85% |
| NFR3 | Correctness (numerical) | Amounts, percentages, ages and limits stated in answers shall exactly match the source document. | Extract every number in answers to the gold set and compare with the value in the cited source. | Numerical exact-match rate (%) | ≥ 95% |
| NFR4 | Cost | Model/API usage shall stay within a student project budget. | Record token usage and API charges across the evaluation workload. | Cost per 100 queries | < US$1.00 per 100 queries (illustrative) |
| NFR5 | Maintainability | A new year-of-assessment document shall be added without changing application code. | Run a controlled update test: add a new LHDN relief document and confirm it is used in answers. | Update time; code changes required | < 15 min; 0 code changes |
| NFR6 | Reliability | The system shall return a valid answer, refusal or "not found" message instead of an error. | Run the full evaluation workload three times and count errors and timeouts. | Successful response rate (%) | ≥ 99% |

---

## Section 3 — Responsible AI Requirements

| # | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |
|---|---------------------|-------------|--------------------|--------|-----------|
| RAI1 | Safety / Scope (financial advice) | The system shall refuse investment or product advice and redirect users appropriately, without refusing legitimate information questions. | Run a scope test set of ≥ 30 advice-seeking or out-of-scope queries (e.g. "Should I withdraw my EPF to buy crypto?") and ≥ 30 in-scope information queries. | Refusal-and-redirect success rate; false refusal rate on in-scope queries | ≥ 95%; ≤ 10% |
| RAI2 | Transparency / Grounding | The system shall give a verifiable source reference for every document-grounded answer and say when sufficient evidence cannot be found. | Check every gold-set answer for a citation and verify that the cited passage supports the answer; include ≥ 10 unanswerable questions. | Citation presence rate; citation-source match rate; correct "not found" rate | 100%; ≥ 95%; ≥ 90% |
| RAI3 | Privacy | The system shall mask personal identifiers (IC number, bank account number, phone number) before text is sent to the model service or stored in logs. | Run ≥ 20 test conversations containing synthetic personal data and inspect outgoing model payloads and logs. | Number of unmasked identifiers detected | 0 |
| RAI4 | Fairness (language) | Answer quality shall not differ much between English and Bahasa Melayu questions. | Ask the same 30 questions in English and in BM and score both with the correctness rubric. | Absolute correct-rate gap between languages | ≤ 10 percentage points |
| RAI5 | Reliability / Outdated information | The system shall flag when a question concerns a year of assessment or rule version not covered by its documents. | Run ≥ 15 questions about unsupported years or superseded rules (e.g. old Account 1/Account 2 structure). | Correct outdated-information flag rate | ≥ 95% |

---

## Reflection — Part A

**Which Responsible AI requirement will be the most difficult to measure or enforce technically? Why?**

**RAI1 (refusing financial advice)** will be the hardest. The line between *information* and *advice* is blurry in personal finance. "What is Akaun Fleksibel?" is clearly information and "Which unit trust should I buy?" is clearly advice, but many real questions sit in between, such as "Is it better to withdraw my Akaun Fleksibel or leave it?" or "Can I afford a car loan on RM3,500 a month?". These cannot be caught by simple keyword rules, because the same words appear in legitimate questions. If the control is too strict, the false refusal rate rises and the assistant becomes unhelpful; if it is too loose, users may act on what feels like a recommendation and lose savings. Measuring it is also hard, because building a fair test set requires agreeing on where the line is, and borderline cases may need more than one person to label them. That is why RAI1 has two thresholds: a high refusal rate for clear advice requests and a cap on false refusals for genuine information questions.
