# AI Governance Case Studies

> Real-world AI deployments examined against EU law, democratic accountability, and cognitive security risks.

**Author:** King Che Magnusson
**Focus:** GRC · AI Governance · IT Risk · Cognitive Security
**LinkedIn:** [linkedin.com/in/kingchemagnussonhr-sudo](https://www.linkedin.com/in/kingchemagnussonhr)

---

## About This Repository

This repository contains structured case studies of AI systems deployed by public authorities and private actors — analysed through three lenses:

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
| [KAIROS — Autonomous Background Agent](cases/kairos-autonomous-agent.md) | Global | Persistent autonomous agent · Software development lifecycle | EU AI Act · GDPR | Complete |

---

## Framework Documents

| Document | Description |
|---|---|
| [Governance Checklist — Biometric AI](framework/governance-checklist-biometric.md) | Pre/during/post deployment checklist for high-risk biometric AI systems |
| [Cognitive Vulnerability Reference](framework/cognitive-vulnerability-reference.md) | CV-01 through CV-07 — cognitive vulnerabilities in AI-assisted decision-making |
| [Control Framework — Autonomous Agents](framework/autonomous-agent-controls.md) | Identity-based and post-attribution controls for persistent AI agents |

---

## Case Highlight: KAIROS

On March 31, 2026, Anthropic published version 2.1.88 of Claude Code on npm. A source map file of approximately 59.8 MB was unintentionally included, exposing internal TypeScript source code. Within this material, a system referred to as **KAIROS** was identified: a persistent, proactive autonomous agent designed to act on its own initiative — independent of user sessions.

**Key characteristics:**

- Persistent background execution via tick mechanism
- AutoDream: memory consolidation transforming observations into stable internal knowledge without human validation
- Adaptive autonomy: increases independence when the user is inactive
- Undercover Mode: suppresses AI origin disclosure in commits, reviews, and outputs

**Primary failure modes:**

| Failure mode | Mechanism |
|---|---|
| Temporal drift | Autonomous decisions peak precisely when oversight is lowest — nights and weekends |
| Hidden authorship | AI-generated outputs are architecturally indistinguishable from human-generated outputs |
| Memory mutation | AutoDream rewrites internal knowledge; historical reasoning cannot be reconstructed |
| Accountability gap | No human approved the action, no human was present — responsibility is structurally unassigned |

**Legal implications:** Undercover Mode is a designed transparency failure, not a bug. KAIROS adaptive autonomy directly inverts the human oversight requirement in EU AI Act Article 14: oversight decreases as user absence increases.

**Governance conclusion:**

> We accept that we cannot know who pressed the button. Therefore we build the system so that no one — human, AI, or KAIROS — can press the dangerous button alone.

The full case study covers legal mapping against EU AI Act Articles 9, 13, and 14, GDPR Articles 5, 6, and 22, five identity-based controls, four post-attribution controls (blast radius control, semantic honey-pots, hardware-bound attestation, runtime integrity), and a complete behavioural metrics framework for autonomy cap measurement.

[Read full case study](cases/kairos-autonomous-agent.md)

---

## Legal Framework Reference

| Regulation | Scope |
|---|---|
| **EU AI Act (2024/1689)** | Risk classification, prohibited practices, high-risk requirements, human oversight, logging |
| **GDPR (2016/679)** | Personal data processing, data subject rights, DPIA |
| **EU Directive 2016/680** | Law enforcement data processing — basis for Swedish Brottsdatalagen |
| **Brottsdatalagen (2018:1177)** | Swedish implementation of Directive 2016/680 |
| **EKMR art. 5 & 8** | Right to liberty, right to private life |

---

## Structure

ai-governance-case-studies/
├── README.md
├── cases/
│   ├── clearview-sweden.md
│   ├── palantir-sweden.md
│   └── kairos-autonomous-agent.md
├── framework/
│   ├── governance-checklist-biometric.md
│   ├── cognitive-vulnerability-reference.md
│   └── autonomous-agent-controls.md
└── images/
└── person-of-interest-series.png

---

## Disclaimer

This repository is produced for educational and portfolio purposes. It does not constitute legal advice. Legal analysis reflects the author's interpretation of publicly available sources and regulatory texts. The KAIROS analysis is based on publicly available source code artifacts following the March 31, 2026 exposure. Certain interpretations of system behaviour are inferred from code structure and internal comments and should be understood as analytical reconstruction rather than confirmed deployed functionality.

---

*Living repository — updated as case law, regulatory guidance, and new cases develop.*
