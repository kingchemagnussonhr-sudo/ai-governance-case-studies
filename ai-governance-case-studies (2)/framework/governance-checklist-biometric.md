# Governance Checklist — High-Risk Biometric AI Systems

> *Part of the [AI Governance Case Studies](../README.md) framework.*
> *Based on EU AI Act (2024/1689), GDPR, EU Directive 2016/680, and Swedish Brottsdatalagen (2018:1177).*

---

## Overview

This checklist applies to public authorities deploying biometric AI systems classified as high-risk under EU AI Act Annex III — including facial recognition, biometric identification, and biometric categorisation systems used in law enforcement contexts.

It is structured in three phases: **before deployment**, **during operation**, and **after deployment**.

---

## Phase 1 — Before Deployment

### Legal basis

- [ ] Processing of biometric data justified under the **strict necessity** standard applicable to sensitive data under BDL / applicable national law
- [ ] Purpose **specific, documented, and legally defined** per use case — not generic authorisation
- [ ] Distinction confirmed between **real-time** (prohibited as default) and **post remote** biometric identification (high-risk, permitted under conditions)
- [ ] For real-time use: **prior authorisation** obtained with necessary and proportionate limitations on time, geography, and persons covered

### Risk assessments

- [ ] **DPIA completed** — Data Protection Impact Assessment under BDL / GDPR, documented before processing begins
- [ ] **Consultation with supervisory authority** (IMY in Sweden) conducted if DPIA identifies high residual risk
- [ ] **FRIA completed** — Fundamental Rights Impact Assessment under EU AI Act Article 27, conducted before first use
- [ ] FRIA addresses impact on each right **independently** — non-discrimination, freedom of expression, right to fair trial, right to liberty, privacy
- [ ] FRIA addresses **cognitive risks** — anchoring, confirmation bias, automation bias in operational use

### Vendor and system requirements

- [ ] Provider has supplied **technical documentation** as required under AI Act Article 11
- [ ] Provider has supplied **risk management documentation** and testing results
- [ ] **Logging capability confirmed** — automatic event logging across system lifecycle (AI Act Article 12)
- [ ] System **registered in EU AI database** where required under AI Act Article 49
- [ ] System is **not a prohibited AI practice** under AI Act Article 5 — particularly: not built on untargeted scraping of facial images

### Third-country transfers

- [ ] **Legal transfer basis established** for any processing outside EU/EEA
- [ ] **Supplementary measures** assessed and documented beyond standard contractual clauses (EDPB Recommendations 01/2020)
- [ ] **Irreversibility risk assessed** — specific documentation of risks arising from permanent nature of biometric data
- [ ] Data residency **confirmed and documented** — where data is stored, who has access

### Human oversight

- [ ] **Human oversight protocols established** — roles, responsibilities, review procedures
- [ ] **Training completed** — oversight personnel trained to understand system limitations and counteract automation bias
- [ ] Procedures in place ensuring **AI output is never the sole basis** for deprivation of liberty or significant legal consequence

---

## Phase 2 — During Operation

- [ ] Each use case **individually documented** with necessity justification
- [ ] Human review is **substantive** — reviewers equipped and expected to actively question system output
- [ ] **Alternative hypotheses documented** before acting on AI-generated match
- [ ] No frihetsberövande or significant legal action based **solely on AI output**
- [ ] **Event logs maintained** and accessible for supervisory review
- [ ] Any unexpected outputs or system failures **logged and escalated**

---

## Phase 3 — After Deployment

- [ ] **Periodic necessity review** — ongoing assessment of whether continued use remains justified
- [ ] **DPIA and FRIA updated** when operational context, system, or legal framework changes
- [ ] **Supervisory authority notified** of use of real-time biometric systems as required
- [ ] **Data minimisation enforced** — no retention beyond documented necessity
- [ ] **Incident log reviewed** — patterns of unexpected output identified and addressed
- [ ] **Performance monitored per demographic group** — differential error rates identified and mitigated

---

## Cognitive Risk Checklist

The following risks are structural features of AI-assisted decision-making. They should be addressed in the FRIA and in operational training.

| Risk | Description | Mitigation |
|---|---|---|
| **Anchoring bias** | Early AI output anchors subsequent interpretation | Require documented alternative hypotheses before acting on match |
| **Confirmation bias** | System output confirms pre-existing investigative assumptions | Structured adversarial review — what would disprove this match? |
| **Automation bias** | Operators defer to algorithmic output without critical evaluation | Training on AI limitations; oversight role requires active challenge |
| **Opacity bias** | Black-box systems perceived as more objective than they are | Transparency requirements; explainability documentation |
| **Alert fatigue** | High volume of flags desensitises operators to genuine signals | Threshold calibration; workload management; audit of flag rates |

---

## Key Legal References

| Source | Relevance |
|---|---|
| [EU AI Act (2024/1689)](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng) | Prohibited practices, high-risk classification, FRIA, logging, human oversight |
| [AI Act Article 5](https://artificialintelligenceact.eu/article/5/) | Prohibited practices including untargeted scraping |
| [AI Act Article 12](https://artificialintelligenceact.eu/article/12/) | Automatic logging requirements |
| [AI Act Article 14](https://artificialintelligenceact.eu/article/14/) | Human oversight — automation bias |
| [AI Act Article 27](https://artificialintelligenceact.eu/article/27/) | Fundamental Rights Impact Assessment |
| [Brottsdatalagen (2018:1177)](https://www.riksdagen.se/sv/dokument-och-lagar/dokument/svensk-forfattningssamling/brottsdatalag-20181177_sfs-2018-1177/) | Swedish law enforcement data processing |
| [EDPB Recommendations 01/2020](https://www.edpb.europa.eu/our-work-tools/our-documents/recommendations/recommendations-012020-measures-supplement-transfer_en) | Supplementary measures for third-country transfers |
| [EDPB Guidelines — Facial recognition in law enforcement](https://www.edpb.europa.eu/system/files/2023-05/edpb_guidelines_202304_frtlawenforcement_v2_en.pdf) | Biometric data in law enforcement context |

---

*Author: King Che Eliezer Ossei-Mensah | [LinkedIn](https://www.linkedin.com/in/kingchemagnussonhr-sudo) | [GitHub](https://github.com/kingchemagnussonhr-sudo)*
