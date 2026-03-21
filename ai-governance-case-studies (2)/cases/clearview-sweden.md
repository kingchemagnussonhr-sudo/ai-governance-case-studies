# Case Study: Clearview AI & Swedish Police (2019–2023)
**AI Governance · GRC · Cognitive Security**

> *Part of the [AI Governance Case Studies](../README.md) series examining real-world AI deployments against EU law, democratic accountability, and cognitive security risks.*

---

## Overview

| Field | Detail |
|---|---|
| **Case name** | Clearview AI  Swedish Police Authority |
| **Jurisdiction** | Sweden / European Union |
| **Time period** | Autumn 2019 – November 2023 |
| **AI system type** | Facial recognition post remote biometric identification |
| **Legal framework** | Brottsdatalagen (2018:1177), GDPR, EU AI Act (2024/1689) |
| **Outcome** | IMY found unlawful use · Sanction overturned on procedural grounds · No court ruling on the merits |
| **Status** | Closed no substantive judicial determination |

---

## Background

In autumn 2019, Swedish police officers attending a Europol conference in The Hague were introduced to Clearview AI, a facial recognition tool that matches uploaded images against a database of billions of photographs scraped from the internet and social media platforms.

Officers returned to Sweden and the application spread to colleagues without formal authorisation, policy review, or legal assessment.

Between autumn 2019 and March 2020, personnel at the National Operations Department (NOA) and the National IT Crime Centre used Clearview AI in active criminal investigations. The Police Authority's own Data Protection Officer had already concluded that use of the tool would violate data protection law, before the use became public.

---

## How It Came to Light

The case was not surfaced by a supervisory authority acting on its own initiative. It was broken by investigative journalism.

SVT and Sveriges Radio Ekot submitted requests for correspondence between the Police Authority and Clearview AI. The Police Authority initially denied using the tool. When email lists were requested, they were classified. A report of misconduct was filed.

The Police Authority subsequently confirmed use of the tool in a small number of operational cases.

**The sequence matters:** press inquiry → denial → document request → classification → misconduct report → confirmation → IMY investigation → sanction decision.

Without press scrutiny, this case would not have entered public record.

---

## Legal Analysis

### 1. Brottsdatalagen (Criminal Data Act, 2018:1177)

The *Brottsdatalagen* (BDL) implements EU Directive 2016/680 in Swedish law. It governs the processing of personal data by competent authorities for law enforcement purposes and applies in place of GDPR for this category of processing.

**Key violations identified by IMY:**

| Requirement | Status |
|---|---|
| Processing of biometric data requires a particularly high legal threshold strict necessity within a lawfully defined purpose | Not met no documented necessity assessment |
| Data Protection Impact Assessment (DPIA) required before high-risk processing | Not conducted |
| Organisational measures to ensure lawful processing | Insufficient |
| Controller accountability for third-party processors | Not exercised no visibility over what Clearview did with ingested data |

IMY found the Police Authority in breach of BDL on three counts. A sanction of SEK 2.5 million was issued in February 2021.

### 2. EU AI Act (2024/1689)

The EU AI Act was not in force at the time of the Clearview use. However, it provides the definitive legal framework for evaluating what would now be required.

**Article 5.1(e) — Prohibited practice:**

> AI systems whose purpose is to create or expand facial recognition databases through the untargeted scraping of facial images from the internet or CCTV footage are prohibited.

Clearview AI's core function is precisely this. Under current EU law, the database itself is an unlawful AI system. No governance framework could make use of Clearview lawful, the prohibition applies to the system's fundamental design.

**Annex III  High-risk classification:**

All biometric identification systems used by law enforcement are classified as high-risk. This classification triggers mandatory requirements including:

- Technical documentation and risk management systems
- Data governance and bias testing
- Human oversight measures (Article 14)
- Automatic logging throughout the system lifecycle (Article 12)
- Registration in the EU AI database (Article 49)
- Fundamental Rights Impact Assessment prior to deployment (Article 27)

**Article 14 — Human oversight:**

Deployers of high-risk AI systems must ensure that human oversight is substantive, not symbolic. Specifically, Article 14 requires that those exercising oversight are able to understand the system's tendency to produce outputs that users may be inclined to rely on without sufficient scrutiny, a direct reference to the risk of automation bias.

**Article 12 — Automatic logging:**

High-risk AI systems must automatically log events throughout their operational lifecycle. This logging is not merely a technical compliance requirement. it is the democratic control mechanism that makes retrospective accountability possible. Without it, there is no basis for scrutiny of why a specific decision was made.

### 3. Third-country transfers and Schrems II

Clearview AI operates American servers. Data ingested into the system left EU jurisdiction without a legal transfer framework.

Following the *Data Protection Commissioner v. Facebook Ireland* ruling (C-311/18, "Schrems II", 2020), transfers of sensitive personal data to the United States require supplementary measures beyond standard contractual clauses, particularly because US intelligence legislation (including FISA 702) may allow access to data held by US-based companies regardless of contractual protections.

Biometric data presents a specific and permanent risk in this context: **it is irreversible**. A compromised password can be changed. A compromised facial geometry cannot. This irreversibility is one of the reasons the BDL imposes such a high necessity threshold for biometric processing, and one of the reasons third-country transfers of biometric data require particularly robust justification and technical safeguards.

The Police Authority could not account for what happened to the data after it was ingested into Clearview. IMY confirmed this in its supervisory decision.

---

## Judicial Outcome

| Stage | Date | Outcome |
|---|---|---|
| IMY supervisory decision | February 2021 | Unlawful use, SEK 2.5M sanction |
| Administrative Court (Förvaltningsrätten) | September 2021 | Upheld IMY decision |
| Court of Appeal (Kammarrätten) | November 2022 | Overturned on procedural grounds, Police Authority had not been given adequate opportunity to respond to all material before the decision |
| Supreme Administrative Court (HFD) | 2023 | No leave to appeal granted |

**Critical observation:** Kammarrätten did not rule on the merits. It did not find that the Police Authority had acted lawfully. The sanction was overturned because of a procedural deficiency in IMY's process, the Police Authority had not been given sufficient opportunity to respond to all elements of the investigation before the sanction decision was issued.

IMY's substantive finding, that the use was unlawful, was never contradicted by any court. No court has ruled that the use was lawful.

**Final legal position:** Unlawful according to the supervisory authority. Sanction not upheld due to procedural error. No judicial determination on the substantive legality of the use.

---

## Cognitive Security Analysis

This case illustrates how AI systems can introduce cognitive vulnerabilities into decision-making processes, independent of the legal violations involved. The following risks are structural features of systems of this type, not assertions about what occurred in specific Clearview investigations.

### CV-01 — Anchoring bias

When an AI system identifies a person of interest early in an investigation, that identification functions as an anchor. Subsequent evidence tends to be interpreted in relation to the initial match rather than evaluated independently. Governance frameworks for AI-assisted investigation must include mechanisms to counteract this tendency for example, requiring investigators to document alternative hypotheses before acting on an AI-generated match.

### CV-02 — Confirmation bias

Systems trained on historical data reflect historical patterns of investigation and outcome. A system that is more likely to generate matches for certain demographic groups will tend to confirm existing investigative biases rather than challenge them. The FRIA process under AI Act Article 27 is designed in part to surface this risk before deployment.

### CV-03 — Opacity bias

Systems that operate as black boxes, where the parameters driving output are not visible to the operator, tend to be perceived as more objective than they are. Opacity does not confer neutrality; it conceals the value choices embedded in the system's design. Transparency is therefore not merely a legal requirement under AI Act Article 13. It is a precondition for genuine human oversight.

### CV-04 — Automation bias

There is substantial research demonstrating that people place excessive trust in algorithmic recommendations, particularly when systems are presented as technically advanced or objective. AI Act Article 14 directly addresses this by requiring that human oversight personnel are equipped to understand and counteract the tendency to defer to system outputs. This is a risk category not a finding about specific conduct in this case.

---

## Governance Checklist

The following checklist reflects what would be required today for lawful deployment of a comparable high-risk biometric AI system by a law enforcement authority in Sweden.

### Before deployment

- [ ] **Legal basis established** Processing of biometric data justified under the strict necessity standard applicable to sensitive data under BDL
- [ ] **Purpose defined and documented** Specific, documented purpose per use case, not generic authorisation
- [ ] **DPIA completed** Data protection impact assessment under BDL, including consultation with IMY if high risk
- [ ] **FRIA completed** Fundamental rights impact assessment under EU AI Act Article 27, addressing each right independently
- [ ] **FRIA covers cognitive risks** Assessment includes risk of anchoring, confirmation bias, and automation bias in operational use
- [ ] **Vendor documentation reviewed** Technical documentation, risk management, and testing information obtained from provider
- [ ] **Third-country transfer framework in place** Legal basis for transfer established, supplementary measures assessed and documented
- [ ] **Data irreversibility risk assessed** Specific assessment of risks arising from permanent nature of biometric data
- [ ] **Logging capability confirmed** Automatic event logging across system lifecycle in place (AI Act Article 12)
- [ ] **Human oversight protocols established** Training, review procedures, and escalation paths documented
- [ ] **System registered** Registration in EU AI database completed where required under AI Act Article 49

### During operation

- [ ] **Each use documented** Individual use cases documented with necessity justification
- [ ] **Human review substantive** Reviewers trained to actively question system output, not rubber-stamp
- [ ] **AI output not sole basis for action** No deprivation of liberty or significant legal consequence based solely on AI match
- [ ] **Logs maintained and accessible** Event logs retained and available for supervisory review
- [ ] **Alternative hypotheses documented** Investigators required to document alternative explanations before acting on AI output

### After deployment

- [ ] **Periodic review of necessity** Ongoing assessment of whether continued use remains justified
- [ ] **DPIA and FRIA updated** Assessments updated when operational context changes
- [ ] **Supervisory authority notified** IMY notified of use of real-time biometric systems as required
- [ ] **Incident logging** Any failures or unexpected outputs logged and reviewed
- [ ] **Data minimisation enforced** Data not retained beyond documented necessity

---

## Sources

### Swedish legal and supervisory sources

| Source | URL |
|---|---|
| IMY supervisory decision — Clearview AI (2021) | https://www.imy.se/nyheter/fel-av-polisen-att-anvanda-app-for-ansiktsigenkanning/ |
| Brottsdatalagen (2018:1177) | https://www.riksdagen.se/sv/dokument-och-lagar/dokument/svensk-forfattningssamling/brottsdatalag-20181177_sfs-2018-1177/ |
| Proposition 2017/18:232 — Brottsdatalag (legislative history) | https://www.riksdagen.se/sv/dokument-och-lagar/dokument/proposition/brottsdatalag_h503232/html/ |
| SVT — Police confirms Clearview use (March 2020) | https://www.svt.se/nyheter/inrikes/ekot-polisen-bekraftar-anvandning-av-kontroversiell-app |
| SVT — Police fined SEK 2.5M (February 2021) | https://www.svt.se/nyheter/inrikes/polisen-far-miljonboter-for-att-han-anvant-ansiktsigenkanningsprogrammet-clearview |
| SVT — New details on scope (December 2020) | https://www.svt.se/nyheter/inrikes/nya-uppgifter-svensk-polis-anvande-kritiserade-ai-tjansten-fler-ganger-an-tidigare-kant |
| SecurityUser — Kammarrätten overturns sanction (2022) | https://www.securityuser.com/se/Nyheter/Samhalle/polisen-doms-for-ansiktsigenkanning |

### EU legal sources

| Source | URL |
|---|---|
| EU AI Act (2024/1689) — full text | https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng |
| EU AI Act Article 5 — prohibited practices | https://artificialintelligenceact.eu/article/5/ |
| EU AI Act Article 12 — automatic logging | https://artificialintelligenceact.eu/article/12/ |
| EU AI Act Article 14 — human oversight | https://artificialintelligenceact.eu/article/14/ |
| EU AI Act Article 27 — FRIA | https://artificialintelligenceact.eu/article/27/ |
| EU AI Act Annex III — high-risk classification | https://artificialintelligenceact.eu/annex/3/ |
| EU Directive 2016/680 — law enforcement data protection | https://eur-lex.europa.eu/legal-content/SV/TXT/?uri=CELEX%3A32016L0680 |
| Schrems II — C-311/18 (CJEU, 2020) | https://curia.europa.eu/juris/document/document.jsf?docid=228677 |
| EDPB Recommendations 01/2020 — Supplementary Measures | https://www.edpb.europa.eu/our-work-tools/our-documents/recommendations/recommendations-012020-measures-supplement-transfer_en |
| EDPB Guidelines — Facial recognition in law enforcement | https://www.edpb.europa.eu/system/files/2023-05/edpb_guidelines_202304_frtlawenforcement_v2_en.pdf |

---

## About This Document

This case study was produced as part of an open portfolio examining AI governance, GRC compliance, and cognitive security risks in real-world AI deployments.

**Framework reference:** Cognitive vulnerability analysis draws on concepts from the OWASP HCI Cognitive Layer  a companion framework to the OWASP LLM Top 10 that treats human cognition as an attack surface in AI-assisted decision-making.

**Scope:** Legal analysis covers Swedish law (Brottsdatalagen) and EU law (EU AI Act, GDPR framework, Directive 2016/680). This document does not constitute legal advice.

**Status:** Living document updated as case law and regulatory guidance develops.

---

*Author: King Che | https://www.linkedin.com/in/king-che-magnusson-833081138/ | [GitHub](https://github.com/kingchemagnussonhr-sudo)*
