# DuitWise MY — AI Personal Finance Information Assistant

> Answers young Malaysians' questions about income tax reliefs, EPF (KWSP) accounts and budgeting in plain language, grounded in official LHDN, KWSP and AKPK documents with source citations — information only, no investment advice.

## Problem Statement
Many young working Malaysians do not understand which tax reliefs they can claim, how their EPF accounts and withdrawals work, or how to budget and manage debt. The official rules are spread across separate agency websites, written in technical language and updated every year, so people miss reliefs, withdraw savings without understanding the cost, and keep little emergency money. A retrieval-augmented AI assistant can answer their questions in their own words from the official documents and cite the source and year, which keyword search and static FAQs cannot do.

## Evidence of the Problem
- 61% of Malaysians would have difficulty coming up with RM1,000 in an emergency (BNM Financial Capability and Inclusion Survey) — Malay Mail (2024, Nov 17), https://www.malaymail.com/news/money/2024/11/17/malaysians-financial-literacy-surpasses-global-average-but-61pc-still-lack-emergency-savings-says-bnm-deputy-governor/157181
- Only about 36% of active EPF members aged 18–55 had reached their Basic Savings level as at October 2024 — New Straits Times (2024, Dec), https://www.nst.com.my/business/corporate/2024/12/1147217/epf-only-36pct-members-meet-current-basic-savings-benchmark

## Initial Related Work / Existing Approaches
- **Official agency portals** (LHDN, KWSP, AKPK) — https://www.hasil.gov.my/en/individu/pelepasan-cukai/ — authoritative, but fragmented across agencies, technical, and organised by year; no single place to ask a plain-language question.
- **Comparison sites** (RinggitPlus, iMoney) — https://ringgitplus.com — useful articles and calculators, but product-focused with commercial incentives, and not tailored to the user's question.
- **FinanceBench** — Islam et al. (2023), arXiv:2311.11944, https://arxiv.org/abs/2311.11944 — GPT-4-Turbo with retrieval answered incorrectly or refused 81% of sampled financial questions, showing general LLMs are unreliable for finance; but it covers company filings, not Malaysian personal finance.

## Engineering Gap / Limitation
No existing tool lets Malaysians ask plain-language questions about their own tax reliefs, EPF accounts and budgeting across official sources at once, and get a grounded answer that cites the current document and year. Portals are fragmented, comparison sites are product-focused, and general chatbots may state wrong amounts or outdated rules.

## Project Objectives
- O1: To design a document-grounded personal finance information assistant based on public official documents from LHDN, KWSP and AKPK.
- O2: To develop the assistant using retrieval-augmented generation with source and year citations, and guardrails that refuse investment advice and flag possibly outdated rules.
- O3: To evaluate the assistant on answer accuracy, groundedness, latency and correct refusal of advice-seeking questions, using a test set of realistic user questions.

## Target Users
Young working adults in Malaysia (about 20–35), such as fresh graduates and first-time taxpayers; secondarily, university students preparing to enter the workforce.

## Data Sources
- LHDN individual tax reliefs by year of assessment — https://www.hasil.gov.my/en/individu/pelepasan-cukai/
- KWSP account restructuring guide and FAQ — https://www.kwsp.gov.my/en/member/account-centre/account-restructuring
- AKPK financial education (budgeting and debt management) — https://www.akpk.org.my/financial-education

> Source documents are used locally for coursework only and are not committed to this repository.

## Repository Structure
```
docs/         requirements, architecture, weekly reports
data/         data card and source list (no copyrighted documents)
src/          application source code
notebooks/    Colab notebooks (from Week 3)
evaluation/   test questions and evaluation results
prompts/      system prompts and prompt versions
```

## Week 1 Deliverables
- [Project scope](docs/week1_project_scope.md) — individual
- [Observation sheet — ChatGPT](docs/week1_observation_sheet.md) — Group 2 (Part A), with [evidence screenshots](docs/evidence/)
- [Conceptual AI system map — ChatGPT](docs/week1_system_map.png) — Group 2 (Part B)

## Weekly Progress
| Week | Focus | Deliverable | Status |
|------|-------|-------------|--------|
| W1 | System map + project scope | README + docs | ✅ |
