# AI Governance Case Studies

> Real-world AI deployments examined against EU law, democratic accountability, and cognitive security risks.

**Author:** King Che Eliezer Ossei-Mensah
**Focus:** GRC · AI Governance · IT Risk · Cognitive Security
**LinkedIn:** [linkedin.com/in/kingchemagnussonhr-sudo](https://www.linkedin.com/in/kingchemagnussonhr-sudo)

---

## About This Repository

This repository contains structured case studies of AI systems deployed by public authorities — analysed through three lenses:

1. **Legal compliance** — EU AI Act, GDPR, national law (Swedish Brottsdatalagen)
2. **Governance** — what was required, what was missing, what a compliant deployment would look like
3. **Cognitive security** — how AI systems introduce cognitive vulnerabilities into human decision-making

Each case follows a consistent structure: background, legal analysis, judicial outcome, cognitive risk analysis, governance checklist, and sources.

The cognitive vulnerability analysis draws on concepts from the **OWASP HCI Cognitive Layer** — a companion framework to the OWASP LLM Top 10 that treats human cognition as an attack surface in AI-assisted decision-making.

---

## Case Studies

| Case | Country | AI System | Legal Framework | Status |
|---|---|---|---|---|
| [Clearview AI — Swedish Police](cases/clearview-sweden.md) | Sweden | Facial recognition | BDL · EU AI Act · GDPR | Complete |
| [Palantir / Acus — Swedish Police](cases/palantir-sweden.md) | Sweden | Predictive analytics · Link analysis | BDL · EU AI Act | In progress |

---

## Framework Documents

| Document | Description |
|---|---|
| [Governance Checklist — Biometric AI](framework/governance-checklist-biometric.md) | Pre/during/post deployment checklist for high-risk biometric AI systems |
| [Cognitive Vulnerability Reference](framework/cognitive-vulnerability-reference.md) | CV-01 through CV-07 — cognitive vulnerabilities in AI-assisted decision-making |

---

## Legal Framework Reference

This repository primarily covers:

| Regulation | Scope |
|---|---|
| **EU AI Act (2024/1689)** | Risk classification, prohibited practices, high-risk requirements, human oversight, logging |
| **GDPR (2016/679)** | Personal data processing, data subject rights, DPIA |
| **EU Directive 2016/680** | Law enforcement data processing — basis for Swedish Brottsdatalagen |
| **Brottsdatalagen (2018:1177)** | Swedish implementation of Directive 2016/680 |
| **EKMR art. 5 & 8** | Right to liberty, right to private life |

---

## Structure

```
ai-governance-case-studies/
├── README.md
├── cases/
│   ├── clearview-sweden.md
│   └── palantir-sweden.md
├── framework/
│   ├── governance-checklist-biometric.md
│   └── cognitive-vulnerability-reference.md
└── images/
    └── person-of-interest-series.png
```

---

## Disclaimer

This repository is produced for educational and portfolio purposes. It does not constitute legal advice. Legal analysis reflects the author's interpretation of publicly available sources and regulatory texts.

---

*Living repository — updated as case law, regulatory guidance, and new cases develop.*
