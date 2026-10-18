# Week 1 — Project Scope

| Field | Details |
|-------|---------|
| **Project Name** | ScholarGuide MY — AI Scholarship Information Assistant |
| **Domain** | Higher Education / Student Financial Aid (Malaysia) |

## Problem Statement

Malaysian school leavers and undergraduates find it hard to work out which scholarships they are eligible for, what the requirements are, and when to apply. The information is spread across many separate official sources (JPA, MARA, Yayasan Khazanah, PTPTN and others), each with its own eligibility rules, income caps, bonding conditions and application windows that change every year. Students therefore miss deadlines or skip scholarships they could have qualified for.

A keyword search or a fixed rule-based filter is not enough: students ask questions in natural language that mix conditions ("I got 7A in SPM, my family earns RM6,000, can I apply for JPA to study engineering overseas?"), and the answer has to combine rules from several documents. AI engineering makes it possible to build a **document-grounded question-answering assistant (RAG)** that answers these questions from official scholarship documents and cites the source, instead of guessing.

## Evidence of the Problem

- **Evidence 1:** In a global survey of more than 2,200 prospective international students, almost 40% said they had difficulty finding information about scholarships and funding.
  **Source / citation:** QS TopUniversities (2014). *Students Online: Global Trends Report 2014*. https://www.topuniversities.com/publications/students-online-global-trends-report-2014
- **Evidence 2:** A 2023 survey of 102 scholarship recipients found most students hear about scholarships informally (teachers, friends, community), many institutions have no formal channel for sharing scholarship information, and confusing terms and document errors cause students to miss awards.
  **Source / citation:** Kumar, R., & Bhandari, S. (2025, June 6). *Why many marginalised students can't access scholarships*. India Development Review. https://idronline.org/article/education/why-many-marginalised-students-cant-access-scholarships/

> Note: Both sources are from outside Malaysia. In Week 2, add at least one Malaysia-specific source (e.g. a local study on scholarship awareness, or a short survey of classmates) to strengthen the local evidence.

## Initial Related Work

- **Study / solution 1:** Official sponsor portals (JPA Penajaan / eSILA, MARA Pendidikan, Yayasan Khazanah scholarship pages)
  **Source / citation:** https://www.jpa.gov.my ; https://www.mara.gov.my/sektor-pendidikan-lp/ ; https://www.yayasankhazanah.com.my/scholarship-programmes
  **Key limitation:** Authoritative, but each sponsor publishes separately and in different formats (web pages, PDF notices, Bahasa Melayu and English). Students must read several documents to compare options; there is no way to ask a question across all of them.

- **Study / solution 2:** Scholarship aggregator websites (e.g. Unienrol, Afterschool.my)
  **Source / citation:** https://unienrol.com/scholarships ; https://afterschool.my/scholarships
  **Key limitation:** Useful listings and filters, but they are search/browse tools, not question answering. Eligibility details are summarised by third parties and can go out of date (application windows change every year), and the student still has to interpret the rules themselves.

- **Study / solution 3:** RAG-based university admission chatbot
  **Source / citation:** Chen, Z., Zou, D., Xie, H., Lou, H., & Pang, Z. (2024). Facilitating university admission using a chatbot based on large language models with retrieval-augmented generation. *Educational Technology & Society, 27*(4), 454–470. https://doi.org/10.30191/ETS.202410_27(4).TP02
  **Key limitation:** Shows RAG raises answer accuracy from 41.4% (plain ChatGPT 3.5) to 89.5%, but it targets admissions, not scholarships, uses university data from the US, Kenya and Hong Kong (not Malaysian sponsors), and was tested with a small user group (53 students). It also shows that a general-purpose chatbot alone is unreliable for this kind of question.

## Engineering Gap / Limitation

**Gap / Limitation:** No existing tool lets Malaysian students ask natural-language eligibility questions **across multiple official scholarship sources at once** and get an answer that is grounded in, and cites, the current official document. Official portals are fragmented, aggregators are browse-only and can be outdated, and general-purpose AI assistants are not grounded in official sponsor documents and may invent requirements or deadlines.

## Initial Project Objectives

- **O1:** To design a document-grounded scholarship information assistant based on publicly available official scholarship documents from Malaysian sponsors.
- **O2:** To develop the assistant using retrieval-augmented generation with source citations and guardrails that flag out-of-date information and refuse to make final eligibility decisions.
- **O3:** To evaluate the assistant in terms of answer accuracy, groundedness (citation correctness), latency, and safe handling of out-of-scope questions, using a test set of real student-style questions.

## Target Users

Malaysian SPM / STPM / matriculation / foundation leavers and first-year undergraduates who are looking for scholarships or sponsorship to fund their first degree. Secondary users: school counsellors and university student-affairs staff who answer scholarship questions.

## Data Sources

- **Source 1:** JPA scholarship programme notices and application guides (Penajaan) — URL: https://www.jpa.gov.my (application portal: https://esilav2.jpa.gov.my)
- **Source 2:** MARA education sponsorship information — URL: https://www.mara.gov.my/sektor-pendidikan-lp/
- **Source 3:** Yayasan Khazanah scholarship programme pages (e.g. Khazanah Watan Scholarship) — URL: https://www.yayasankhazanah.com.my/scholarship-programmes
- **Source 4 (optional):** PTPTN education loan information (for comparing scholarship vs. loan) — URL: https://www.ptptn.gov.my

> Documents will be downloaded for local course use only. Only the URLs and a data card are committed to the public repository, not the copyrighted documents themselves.

## Similar to EduAdvisor

- Document-grounded Q&A assistant using retrieval-augmented generation over official PDFs/web pages.
- Same type of user (students) and same quality dimensions: accuracy, groundedness, latency, safety.
- Uses source citations so users can verify answers in the official document.

## Different from EduAdvisor

- **Multiple organisations instead of one university:** documents come from several independent sponsors with different formats and rules, so retrieval must keep track of which sponsor each rule belongs to.
- **Time-sensitive data:** application windows and requirements change every year, so the system must store the document year/intake and warn users about outdated information.
- **Eligibility reasoning:** questions combine several conditions (grades, household income, field of study, citizenship/Bumiputera status), which is harder than looking up a single prerequisite.
- **Bilingual content:** many official notices are in Bahasa Melayu, while users may ask in English (or vice versa).

## Responsible AI Risks

- **Risk 1 — Hallucination / outdated information:** The model invents a requirement or states last year's deadline as current → a student misses the real application window or does not apply for a scholarship they qualify for. *Control:* answer only from retrieved documents, always cite source and year, show "check the official portal" for dates.
- **Risk 2 — Fairness:** Some scholarships have eligibility tied to income, region or Bumiputera status, and some sponsors have far more documents than others → the system may over-recommend well-documented scholarships, or explain eligibility poorly for certain groups (e.g. non-Bumiputera students, arts/humanities students). *Control:* test accuracy across user profiles and sponsors; present eligibility rules neutrally as stated by the sponsor.
- **Risk 3 — Privacy:** Students may type personal details (IC number, family income, results slips) into the chat → sensitive personal data could be logged or exposed. *Control:* never ask for identifiers, warn users not to share them, and mask/avoid storing personal data in logs.
- **Risk 4 — Over-reliance / decision authority:** A student treats the assistant's answer as a final eligibility decision → may not apply, or may submit an incomplete application. *Control:* the assistant gives information only and states that the sponsor makes the final decision.
