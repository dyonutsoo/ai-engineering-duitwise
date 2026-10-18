# ScholarGuide MY — AI Scholarship Information Assistant

> Answers Malaysian students' questions about scholarship eligibility, requirements and application steps, grounded in official sponsor documents (JPA, MARA, Yayasan Khazanah) with source citations.

## Problem Statement
Malaysian school leavers and undergraduates struggle to find out which scholarships they qualify for, because eligibility rules, income caps, bonding conditions and deadlines are scattered across many separate official sources that change every year. Their questions combine several conditions and need rules from more than one document, which keyword search and fixed filters cannot handle well. A retrieval-augmented AI assistant can answer these natural-language questions from the official documents and cite its source.

## Evidence of the Problem
- Almost 40% of 2,200+ prospective students surveyed reported difficulty finding scholarship and funding information — QS TopUniversities (2014), *Students Online: Global Trends Report 2014*, https://www.topuniversities.com/publications/students-online-global-trends-report-2014
- Most students hear about scholarships informally, many institutions have no formal channel for scholarship information, and confusing terms cause students to miss awards — Kumar & Bhandari (2025), *Why many marginalised students can't access scholarships*, India Development Review, https://idronline.org/article/education/why-many-marginalised-students-cant-access-scholarships/

## Initial Related Work / Existing Approaches
- **Official sponsor portals** (JPA, MARA, Yayasan Khazanah) — https://www.jpa.gov.my — authoritative, but fragmented across sponsors and formats; no way to ask one question across all of them.
- **Scholarship aggregators** (Unienrol, Afterschool.my) — https://unienrol.com/scholarships — good listings, but browse/search only, summarised by third parties, and can be out of date.
- **RAG university admission chatbot** — Chen et al. (2024), *Educational Technology & Society, 27*(4), 454–470, https://doi.org/10.30191/ETS.202410_27(4).TP02 — RAG raised accuracy from 41.4% (plain ChatGPT 3.5) to 89.5%, but covers admissions not scholarships, non-Malaysian data, and only 53 test users.

## Engineering Gap / Limitation
No existing tool lets Malaysian students ask natural-language eligibility questions across multiple official scholarship sources and get a grounded, cited answer that reflects the current intake. Portals are fragmented, aggregators are browse-only, and general-purpose chatbots may invent requirements or deadlines.

## Project Objectives
- O1: To design a document-grounded scholarship information assistant based on public official Malaysian scholarship documents.
- O2: To develop the assistant using retrieval-augmented generation with source citations and guardrails for outdated information and out-of-scope requests.
- O3: To evaluate the assistant on answer accuracy, groundedness, latency and safety using a test set of student-style questions.

## Target Users
Malaysian SPM / STPM / matriculation / foundation leavers and first-year undergraduates looking for first-degree scholarships; secondarily, school counsellors and student-affairs staff.

## Data Sources
- JPA scholarship notices and application guides — https://www.jpa.gov.my
- MARA education sponsorship information — https://www.mara.gov.my/sektor-pendidikan-lp/
- Yayasan Khazanah scholarship programmes — https://www.yayasankhazanah.com.my/scholarship-programmes
- PTPTN education loan information (optional, for comparison) — https://www.ptptn.gov.my

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
