# Week 1 — Project Scope

| Field | Details |
|-------|---------|
| **Project Name** | DuitWise MY — AI Personal Finance Information Assistant |
| **Domain** | Personal Finance / Financial Literacy (Malaysia) |

## Problem Statement

Many young working Malaysians do not understand the basic money rules that affect them every year: which **income tax reliefs** they can claim, how their **EPF (KWSP) accounts** work and when they can withdraw, and how to **budget and manage debt**. The official information exists, but it is spread across separate agency websites (LHDN, KWSP, AKPK), written in formal or technical language, and updated every year (for example new tax reliefs each year of assessment, and the 2024 EPF account restructuring). As a result, people miss reliefs they are entitled to, make withdrawals without understanding the long-term cost, and have little emergency savings.

A keyword search or FAQ page is not enough, because people ask questions in their own words and the answer often depends on their situation ("I'm 26, I bought a laptop and paid for my mum's medical check-up, what can I claim?"). AI engineering makes it possible to build a **document-grounded question-answering assistant (RAG)** that explains these rules in plain language from official documents and cites the source and year. The assistant gives **information only**: it does not recommend investments or financial products, and it does not make decisions for the user.

## Evidence of the Problem

- **Evidence 1:** Bank Negara Malaysia's Financial Capability and Inclusion Survey found that 61% of Malaysians would have difficulty coming up with RM1,000 in an emergency, and BNM has urged making financial education mandatory for tertiary students.
  **Source / citation:** Malay Mail. (2024, November 17). *Malaysians' financial literacy surpasses global average but 61pc still lack emergency savings, says BNM deputy governor*. https://www.malaymail.com/news/money/2024/11/17/malaysians-financial-literacy-surpasses-global-average-but-61pc-still-lack-emergency-savings-says-bnm-deputy-governor/157181
- **Evidence 2:** Only about 36% of active EPF members aged 18–55 had reached their age-based Basic Savings level as at October 2024.
  **Source / citation:** New Straits Times. (2024, December). *EPF: Only 36pct members meet current basic savings benchmark*. https://www.nst.com.my/business/corporate/2024/12/1147217/epf-only-36pct-members-meet-current-basic-savings-benchmark

> Note: Both figures are reported by news outlets quoting BNM and EPF/Ministry of Finance. In Week 2, link the original BNM survey report or EPF release where available.

## Initial Related Work

- **Study / solution 1:** Official agency portals and tools — LHDN tax relief pages and e-Filing, KWSP i-Akaun and member guides, AKPK financial education modules
  **Source / citation:** https://www.hasil.gov.my/en/individu/pelepasan-cukai/ ; https://www.kwsp.gov.my/en/member/account-centre/account-restructuring ; https://www.akpk.org.my/financial-education
  **Key limitation:** Authoritative, but each agency publishes separately, often as long PDFs or tables organised by year. Users must already know what to look for, and there is no way to ask one plain-language question across tax, EPF and budgeting at once.

- **Study / solution 2:** Commercial personal finance comparison sites (e.g. RinggitPlus, iMoney)
  **Source / citation:** https://ringgitplus.com ; https://www.imoney.my
  **Key limitation:** Helpful articles and calculators, but they focus on comparing and selling financial products (credit cards, loans) and may have commercial incentives. Articles are written for general readers, not answered for the user's question, and can go out of date when rules change.

- **Study / solution 3:** FinanceBench — benchmark for LLM financial question answering
  **Source / citation:** Islam, P., Kannappan, A., Kiela, D., Qian, R., Scherrer, N., & Vidgen, B. (2023). *FinanceBench: A new benchmark for financial question answering*. arXiv:2311.11944. https://arxiv.org/abs/2311.11944
  **Key limitation:** Shows that even GPT-4-Turbo with a retrieval system answered incorrectly or refused 81% of sampled financial questions, so a general-purpose LLM cannot be trusted with finance facts without careful grounding and evaluation. However, it covers company financial filings, not personal finance rules for individuals, and not Malaysian sources.

## Engineering Gap / Limitation

**Gap / Limitation:** No existing tool lets Malaysians ask plain-language questions about their own tax reliefs, EPF accounts and budgeting **across official sources at once** and get an answer that is grounded in, and cites, the current official document and year. Official portals are fragmented and technical, comparison sites are product-focused, and general-purpose chatbots are not reliably grounded in Malaysian rules and may state wrong amounts or outdated reliefs (as FinanceBench shows for financial QA in general).

## Initial Project Objectives

- **O1:** To design a document-grounded personal finance information assistant based on publicly available official documents from LHDN, KWSP and AKPK.
- **O2:** To develop the assistant using retrieval-augmented generation with source and year citations, and guardrails that refuse investment or product recommendations and flag possibly outdated rules.
- **O3:** To evaluate the assistant on answer accuracy (amounts, limits, eligibility), groundedness (citation correctness), latency, and correct refusal of out-of-scope or advice-seeking questions, using a test set of realistic user questions.

## Target Users

Young working adults in Malaysia (roughly 20–35 years old), such as fresh graduates and first-time taxpayers, who need to file income tax, understand their EPF savings and manage a monthly budget. Secondary users: university students preparing to enter the workforce.

## Data Sources

- **Source 1:** LHDN individual tax reliefs (by year of assessment) and the BE form explanatory notes — URL: https://www.hasil.gov.my/en/individu/pelepasan-cukai/
- **Source 2:** KWSP account restructuring guide, FAQ and Akaun Fleksibel withdrawal guide — URL: https://www.kwsp.gov.my/en/member/account-centre/account-restructuring (FAQ PDF: https://www.kwsp.gov.my/documents/d/guest/account-restructuring-faq-en)
- **Source 3:** AKPK financial education materials on budgeting and debt management — URL: https://www.akpk.org.my/financial-education

> Documents will be downloaded for local course use only. Only the URLs and a data card are committed to the public repository, not the documents themselves.

## Similar to EduAdvisor

- Document-grounded Q&A assistant using retrieval-augmented generation over official PDFs and web pages.
- Answers must cite the source document so users can check them.
- Same core quality dimensions: accuracy, groundedness, latency and safety.

## Different from EduAdvisor

- **Different domain and users:** personal finance for young working adults instead of academic advising for students.
- **Several government agencies instead of one university:** documents come from LHDN, KWSP and AKPK, each with its own format and rules, so the system must track which agency and which year each rule comes from.
- **Numbers matter:** many answers involve exact amounts and limits (e.g. relief caps in RM, minimum withdrawal amounts), so a small error is a wrong answer. Evaluation must check numbers exactly, not just whether the text sounds right.
- **Rules change every year:** tax reliefs change each year of assessment, and EPF accounts were restructured in 2024, so the system must store the document year and warn when information may be outdated.
- **Stronger scope boundary:** the system must clearly refuse to give investment advice or recommend financial products, which EduAdvisor does not need to handle.

## Responsible AI Risks

- **Risk 1 — Hallucination / wrong amounts:** The model states a wrong relief cap or an outdated rule → the user over-claims and faces a tax penalty, or under-claims and loses money. *Control:* answer only from retrieved documents, always cite source and year of assessment, and remind users to confirm on the official portal before filing.
- **Risk 2 — Inappropriate financial advice:** A user asks "Should I withdraw my EPF to invest in crypto?" and the system gives a recommendation → the user may lose retirement savings. *Control:* scope guardrail that detects advice-seeking questions, explains the relevant rules neutrally, and refers the user to AKPK's free advisory service or a licensed adviser.
- **Risk 3 — Privacy:** Users may type their IC number, salary, EPF balance or bank details into the chat → sensitive personal data could be logged or exposed. *Control:* never ask for identifiers, warn users not to share them, and avoid storing personal data in logs.
- **Risk 4 — Fairness / accessibility:** Official documents are mainly in formal English and Bahasa Melayu → users with lower financial or language literacy may get answers they cannot understand. *Control:* plain-language answers, and testing answer quality in both English and Bahasa Melayu.
