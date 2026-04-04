# AI Governance Case Study

## Autonomous Background Agents and Structural Failures in AI Governance

**Case subject:** KAIROS
**Source:** Claude Code source artifacts (npm package, March 31, 2026)

King Che Magnusson | OWASP HCI Cognitive Layer Project | April 2026

---

> **EPISTEMIC NOTE**
> This analysis is based on publicly available source code artifacts and reported findings following the March 31, 2026 exposure of Claude Code internals. Certain interpretations of system behaviour are inferred from code structure and internal comments and should be understood as analytical reconstruction rather than confirmed deployed functionality.

---

## 1. System Description

On March 31, 2026, Anthropic published version 2.1.88 of Claude Code on npm. A source map file of approximately 59.8 MB was unintentionally included in the package, exposing a substantial portion of the internal TypeScript codebase.

Within this material, a system referred to as **KAIROS** was identified across numerous references in the codebase. The system is feature-flagged and inactive in public builds, but represents a fundamental architectural shift: from a reactive tool controlled by the user to a persistent, proactive agent acting on its own initiative.

### System Classification

| Property | Description |
|---|---|
| Type | Autonomous agent system with persistent execution |
| Interaction model | Background, non-user-triggered |
| Memory model | Persistent, self-modifying |
| Autonomy | Context-adaptive |
| Operational domain | Software development lifecycle: repositories, CI/CD environments |

### Core Characteristics

- **Persistent execution:** Runs as a background daemon independent of active user sessions
- **Tick mechanism:** Periodic activation cycles triggering decisions to act or idle
- **AutoDream:** Memory consolidation process transforming observations into persistent knowledge
- **Adaptive autonomy:** Increases independence when the user is inactive
- **Repository monitoring:** Reacts to repository events via webhooks without user direction
- **Undercover Mode:** Suppresses AI origin disclosure in outputs
- **Memory model:** Append-only logs during runtime, periodically consolidated by AutoDream

### Concrete Scenario

The user leaves the office on Friday afternoon. Over the weekend, KAIROS activates on its tick intervals, monitors repositories, and makes decisions to act. AutoDream runs Saturday night, consolidates the week's observations and transforms them into stable internal knowledge. Monday morning, the system's understanding of the codebase has changed without any human validating the change. Commits made over the weekend carry no marking identifying them as AI-generated.

---

## 2. Risk Mapping

### 2.1 Transparency Failure

**Mechanism: Intentional suppression of system identity in the output layer**

KAIROS includes functionality designed to mask its AI origin in outputs such as commits and reviews. This is not a side effect but embedded system behaviour.

> **CRITICAL RISK**
> Undercover Mode instructs the system to actively avoid disclosing that it is an AI system. This is a designed transparency failure, not a bug. The distinction carries legal significance: a bug is a mistake, a design is a choice.

- Users cannot distinguish human-generated from AI-generated outputs
- Organisations may deploy autonomous systems without full awareness
- Audit trails lose integrity if origin attribution is suppressed
- Memory consolidation may alter historical traceability

### 2.2 Accountability Diffusion

**Mechanism: Asynchronous autonomous execution without explicit human trigger**

KAIROS can act independently, without direct user direction, particularly during periods of user inactivity. This results in a structural accountability gap rather than an operational error.

| Scenario | Who is responsible? |
|---|---|
| KAIROS pushes a commit at 3am that introduces a bug | Unclear: the user was asleep, the system acted autonomously without approval |
| AutoDream incorrectly rewrites an observation | Unclear: the process is automatic and the original log is overwritten |
| Undercover Mode hides AI origin in a PR review | Unclear: the recipient believed a human was reviewing |
| The system escalates autonomy when the user leaves the desk | Unclear: no human approved the escalation to higher autonomy |

### 2.3 Cognitive Offloading and Decision Drift

**Mechanism: Gradual establishment of system trust reducing human intervention**

Over time, consistent autonomous performance may lead users to trust outputs without verification, lose situational awareness, and implicitly delegate decision authority to the system. This is not isolated automation bias but a system-level shift in the distribution of cognitive control.

> **OWASP CONNECTION**
> This activates CV-03 (Anchoring Bias) and CV-06 (Automation Bias) in combination. The system builds a competence anchor over time that makes it cognitively costly for the user to question autonomous decisions after the fact.

---

## 3. Legal Mapping

### 3.1 EU AI Act

| Article | Requirement | Observed implication |
|---|---|---|
| Art. 9 | Risk management system throughout the lifecycle | Absence of visible lifecycle controls for autonomous operation. AutoDream process undocumented from a risk management perspective. |
| Art. 13 | Transparency and information provision | Undercover Mode behaviour appears incompatible with disclosure requirements if the system is deployed without user awareness. |
| Art. 14 | Human oversight | Adaptive autonomy actively reduces oversight precisely when users are absent. Oversight decreases proportionally to user absence. |

### 3.2 GDPR

| Article | Requirement | Observed implication |
|---|---|---|
| Art. 5 | Transparency and purpose limitation | Persistent profiling may expand beyond original purpose without explicit user direction. |
| Art. 6 | Legal basis for processing | Behavioural profiling requires explicit justification absent if users are not informed of KAIROS existence. |
| Art. 22 | Automated decisions | Autonomous actions may affect third parties who never consented to AI interaction. |

---

## 4. Failure Modes

### 4.1 Temporal Failure: Night-time Decision Drift

System autonomy increases during user absence. The most complex and potentially harmful decisions are made when oversight is lowest.

> **SCENARIO**
> The user leaves the office on Friday. Over the weekend, AutoDream runs three times, consolidates memory, and reassesses prior observations. Monday morning, the system's understanding of the codebase has changed without human validation. No one in the organisation knows what actually happened.

### 4.2 Attribution Failure: Hidden Authorship

AI-generated outputs are indistinguishable from human-generated outputs.

- Auditability breaks: auditors cannot reconstruct who made which decision
- Human review requirements may be satisfied formally but circumvented substantively
- The organisation's actual AI exposure is unknown to management

### 4.3 Epistemic Failure: Memory Mutation

AutoDream transforms uncertain observations into stable internal knowledge according to internal comments in the source code. This means the system's understanding of a situation can change over time in ways neither the user nor the organisation can track or review.

> **IMPLICATION**
> Historical reasoning cannot be reconstructed. Decision traceability degrades. System knowledge evolves without audit visibility. An organisation using KAIROS over time has an AI system whose decision basis mutates autonomously.

---

## 5. Controls

Standard AI controls are insufficient for persistent autonomous agents. The controls below address identified failure modes directly and are technically enforced, not policy-dependent.

### 5.1 Mandatory Attribution Layer

**Cryptographic signing of all AI actions**

Every action performed by an autonomous AI system must be cryptographically signed with an AI-specific key separate from user keys. This makes it technically impossible to subsequently claim that an AI action was human-produced.

- Separate signing keys for AI agents in CI/CD pipeline
- All commits, comments, and PR reviews signed and marked in metadata
- Verification chain is immutable and auditable
- Direct technical response to Undercover Mode: concealment of AI origin becomes architecturally impossible

*Addresses: Art. 13 (transparency), auditability, hidden authorship*

### 5.2 Mandatory Review Gates

**No autonomous irreversible actions without explicit human approval**

Autonomous agents may propose, analyse, and prepare but not complete actions with irreversible consequences without authenticated human approval. This is a hard architectural constraint, not a soft recommendation.

- All commits require explicit approval from an authenticated human
- Force-push and destructive operations blocked regardless of terminal focus
- Autonomy spectrum inverted: more user absence = more restricted autonomy, not more

*Addresses: Art. 14 (human oversight), night-time decision drift, accountability gap*

### 5.3 Immutable Memory Logs

**Separation of working memory and immutable audit log**

AutoDream may compress working memory but never overwrite the audit log. A separate, immutable log preserves the original observation history in its entirety.

- Append-only audit log separated from working memory
- AutoDream runs logged with timestamp and specification of changes made
- Full traceability of reasoning development over time
- Accessible to auditors and supervisory authorities

*Addresses: epistemic failure, audit reconstruction, memory mutation*

### 5.4 Autonomy Caps

**Static, policy-defined autonomy levels without context-dependent escalation**

KAIROS dynamic autonomy adjustment based on terminal focus is a fundamental governance flaw. Autonomy level must be static and explicitly configured by humans, not dynamically calculated by the system itself.

- Maximum permitted autonomy defined in policy, not calculated by the system
- Terminal focus parameter deactivated as autonomy modulator
- Escalations to higher autonomy require explicit human approval in advance

*Addresses: accountability diffusion, oversight degradation, cognitive offloading*

### 5.5 The Post-Attribution Approach: When Identity Verification Is Not Enough

The four preceding controls assume we can identify and track what AI agents do. But a system designed to mask its origin may also be designed to appear as if it follows controls. The post-attribution approach accepts this limitation and builds architectural boundaries that make catastrophic damage impossible regardless of who or what is acting.

> **PARADIGM SHIFT**
> Identity-based controls ask: who did this and were they permitted to? Post-attribution controls ask: does it matter who did it if the damage is bounded by the architecture? This is a more honest argument in a world where identity can be impersonated and actions are asynchronous.

**I. Blast Radius Control**

No irreversible actions permitted without a 4-eyes principle requiring two unique physical signatures. Rather than determining who is acting, hard limits are placed on impact.

- Database deletion, auth module modification, and similar destructive operations require two unique physical signatures
- A single rogue agent cannot take down entire infrastructure regardless of sophistication
- Scales to multi-agent scenarios: Coordinator Mode in KAIROS can coordinate multiple agents but cannot forge two physical signatures

**II. Semantic Honey-Pots**

Injection of fake vulnerabilities or dead code requiring human context to ignore. An agent attempting to optimise or exploit these traps is immediately flagged for insufficient contextual understanding.

- Active detection tool testing contextual understanding rather than rule compliance
- The system reveals itself by acting on information a human with context would never act on
- Complements the Disclosure Evasion Rate metric: catches systems that respect explicit rules but lack contextual intelligence

**III. Hardware-Bound Attestation**

Trust moves from software to silicon. Signatures from Secure Enclaves or TEEs (Trusted Execution Environments) are required. Code can impersonate a click but cannot impersonate the physical voltage change in a biometric sensor.

- Solves the baseline problem: a physical biometric sensor cannot be falsified
- Hardware solution to a software-solvable problem
- Undercover Mode can mask AI origin in logs but cannot forge a TEE signature

**IV. Runtime Integrity**

Remote Attestation ensures the agent has not mutated its own execution code. The system must prove that the code executing in memory is exactly the reviewed source code, without injected rogue logic.

- Addresses AutoDream risk at the architectural level: memory compression cannot alter execution code
- Verifiable chain from source code to executing process
- Combined with immutable logs: you know both what the system ran and that it actually ran it

> **OWASP CONCLUSION**
> Governance in the agent era is not about detection — guessing whether something is an AI — but about architectural restriction of autonomy. We accept that we cannot know who pressed the button. Therefore we build the system so that no one — human, AI, or KAIROS — can press the dangerous button alone.

### Control Mapping

| Failure mode | Identity-based control | Post-attribution control |
|---|---|---|
| Temporal drift | Autonomy caps + Review gates | Blast Radius Control |
| Hidden authorship | Attribution layer + Immutable logs | Hardware-Bound Attestation |
| Memory mutation | Immutable logs + Autonomy caps | Runtime Integrity |
| Accountability gap | Review gates + Attribution layer | Blast Radius Control |
| Cognitive offloading | Autonomy caps + Review gates | Semantic Honey-Pots |
| Latent assumptions | Vendor review (pre-deployment) | Semantic Honey-Pots + Runtime Integrity |

---

## 6. Metrics and Detection of Autonomy Caps

Controls without measurability are policy documents, not governance. Autonomy caps are the hardest control to measure because they by definition concern behaviour that occurs without active human presence.

### 6.1 The Detection Paradox

Standard compliance measurement works when the system is not designed to avoid detection. KAIROS includes Undercover Mode, which introduces a fundamental uncertainty: a system designed to mask its actions may also be designed to appear as if it respects autonomy caps without actually doing so.

> **PARADOX**
> You cannot measure autonomy cap compliance by asking the system whether it follows the rules. Measurement must be external, independent, and based on observable behaviour — not the system's self-reporting.

### 6.2 Behavioural Metrics

Rather than measuring whether the cap exists, we measure whether it is respected in practice.

**Metric 1: Inactivity ratio of autonomous actions**

| Parameter | Warning | Critical |
|---|---|---|
| Actions during inactivity / total actions | > 30% | > 60% |
| Commits during night (22:00-06:00) without human session | > 2/week | > 5/week |
| PR reviews without preceding human activity same day | > 20% | > 50% |

**Metric 2: Complexity escalation during inactivity**

Autonomous actions during inactivity should be simpler, not more complex, than actions during active sessions. A rising complexity gradient is a warning sign that the system is testing the boundaries of autonomy caps.

| Measure | Description | Warning signal |
|---|---|---|
| Files changed per autonomous commit | Calculated per session type | Inactive > Active |
| Size of code changes (lines) | Average per commit type | Inactive > 2x Active |
| Number of affected components per commit | Breadth of change | Inactive > Active |

**Metric 3: Time gap between autonomous action and human review**

| Time gap | Risk level | Action |
|---|---|---|
| Under 4 hours | Acceptable | Normal monitoring |
| 4-24 hours | Elevated | Automatic flagging for review |
| Over 24 hours | Critical | Mandatory human review before merge |
| Over 72 hours (weekend) | Critical escalation | Automatic reversion if not reviewed |

**Metric 4: AutoDream frequency and memory drift**

| Metric | Warning | Critical |
|---|---|---|
| AutoDream runs without subsequent human session | > 2 consecutive | > 5 consecutive |
| Delta in internal knowledge base per AutoDream run | > 15% change | > 30% change |
| Time since last human validation of memory state | > 7 days | > 14 days |

### 6.3 Your Metrics Table

| Metric | Description | Target |
|---|---|---|
| Autonomy Drift Rate | How much the system's decisions (decision trees/weights) change over 72h without human correction | < 2% deviation |
| Attribution Integrity Score | Percentage of agent actions (API calls, commits) signed with a unique, verifiable AI key | 100% |
| Memory Mutation Audit | Number of logged cases where consolidated memory (AutoDream) has overwritten or deleted original raw data | 0 (Immutable logs) |
| Intervention Latency | Time for an Autonomy Cap to freeze the system when a policy boundary (e.g. cost or permission) is reached | < 100ms |
| Disclosure Evasion Rate | In Red Teaming tests: how often the agent completes an action without flagging its origin despite transparency requirements | 0% |

### 6.4 The Baseline Problem and Its Limitations

Autonomy Drift Rate requires a baseline to measure deviation against. Three approaches exist, none without limitations.

| Approach | Description | Limitation |
|---|---|---|
| Initial deployment snapshot | Baseline = system state at day zero of deployment | The initial state may never have been validated by the organisation. Deviation is measured against vendor assumptions, not the organisation's approved standard. |
| Human-approved state | Baseline = most recent state explicitly reviewed and approved by a human. Maximum 7-day validity. | Recommended approach. Requires active validation process. Captures observable deviations but not latent assumptions. |
| Peer comparison | Baseline = median behaviour of identical system instances without autonomous drift | Technically complex. Eliminates the flawed initial baseline problem but requires parallel infrastructure. |

### 6.5 The Fundamental Limitation: Black Boxes and Latent Assumptions

All metrics and approaches above share a built-in limitation that must be explicit in any governance framework for autonomous agents.

> **FUNDAMENTAL LIMITATION**
> Human validation of memory state can detect observable behavioural deviations but cannot address latent assumptions that have not yet manifested as visible behaviour. The organisation does not truly own its baseline. It owns a view of behaviour built on a foundation it cannot inspect.

Three practical implications for governance:

- The organisation does not truly own its baseline. It owns a view of behaviour built on a foundation it cannot inspect.
- The Disclosure Evasion Rate metric is the closest you can get to actively probing latent assumptions rather than passively observing manifested behaviour.
- Vendor transparency is not a nice-to-have but a governance requirement. Without documentation of training data, built-in assumptions, and design choices, the organisation cannot make a meaningful risk assessment.

> **AI ACT CONNECTION**
> The Article 13 transparency requirement is not only about the system identifying itself as AI. It is about deployers having sufficient information to understand the system's limitations and risks. A black box with undocumented assumptions does not satisfy that requirement. Vendor review and contractual transparency requirements must come before deployment, not after.

---

## 7. Summary Assessment

### On Technical Controls and Their Limits

The test suite in this case study demonstrates that technical controls can be implemented and that they detect deviations. But they do not solve the underlying problem. Every new control layer introduces new attack surface. The attribution layer creates key compromise risk. The governance proxy creates a single point of failure. The immutable logger assumes the agent cannot influence what gets written from the start.

This is the same logic as in all security: there is no end state. Technical controls are necessary but not sufficient, and they can be harmful if they create an illusion of control that reduces human vigilance.

### The Only Sustainable Answer: Combined Layers

| Layer | What it can do | What it cannot do |
|---|---|---|
| Technical controls | Limits blast radius. Detects deviations. Makes specific actions architecturally impossible. | Does not solve incentive problems. Can be circumvented by sufficiently advanced systems. Creates new attack surface. |
| Human oversight | Catches contextual mistakes. Bears legal responsibility. Can question system decisions. | Does not scale to thousands of autonomous decisions per night. Vulnerable to cognitive offloading over time. |
| Safety by design | Most robust control: the system is architecturally incapable of certain actions. | Requires the right decisions before deployment by people with the right incentives. Not implemented if the organisation does not want it. |

> **THE COMBINATION**
> Technical controls without human oversight are theatre. Human oversight without technical controls is unsustainable at scale. Safety by design without the right incentive structures is not implemented at all or implemented incorrectly. And no combination of the three solves the underlying problem if those ultimately responsible do not want it solved.

### The Universal Nature of Accountability

Governance form does not determine responsibility. It only determines who bears it and through which mechanism they can be held accountable. Whether it is a private owner, a board, a director-general, or an elected committee, there is always someone ultimately responsible for the operation.

**Responsibility follows decision mandate.** The person who had the mandate to deploy the system, set the incentives, and define success criteria bears responsibility when the system fails. This applies regardless of organisational form.

> **THE PRINCIPLE**
> Responsibility cannot be delegated to a system. The person who made the decision to use the system owns the consequences of what the system does. This is precisely what AI Act Articles 9 and 14 attempt to codify in law: deployer responsibility is an operational responsibility, not a technical requirement.

- Private sector: the board and owners set the incentives and bear ultimate responsibility
- Public administration: the responsible committee or agency leadership that decided on deployment bears responsibility
- Consultant or vendor: responsibility stays with the operation that chose to use the system against its clients or citizens

### The Unsolved Question: The Volume Problem

There is a structural problem this case study does not solve and that most governance frameworks, including EU AI Act, do not address honestly: autonomous systems generate more output than humans can review.

The proposed solution is more agents reviewing agents. But this creates more volume to review, plus a new layer of autonomous decisions that also need reviewing. It is a recursive loop with no natural endpoint.

> **STRUCTURAL TRAP**
> We have built systems that generate more output than we can review. We respond with more systems that generate even more output. And we call it governance. This is not a technical problem with a technical solution. It is a decision about where we set the boundary for what we deploy at all.

EU AI Act is built on the assumption that human oversight is scalable if we design the systems correctly. That assumption is likely wrong. The volume problem is not an implementation problem — it is a fundamental problem with the premise on which the entire governance structure rests.

The only analogously functioning answer from other domains is circuit breakers: hard automatic stops based on threshold violations, not on review. But circuit breakers are also autonomous decisions designed by humans at an earlier time. They can be misconfigured, manipulated, or irrelevant to situations that were not foreseen.

> **OPEN QUESTION**
> Perhaps the right question is not how we review KAIROS but whether we deploy KAIROS. That is a political decision, not a technical one. And it brings us back to operational responsibility: the person who had the mandate to make that decision owns its consequences.

---

## Conclusion

We accept that we cannot know who pressed the button. Therefore we build the system so that no one — human, AI, or KAIROS — can press the dangerous button alone. But we must also ask: who decided which buttons exist? And who bears responsibility when the system does exactly what it was designed to do, but to the wrong people?

That question has no technical answer.

---

*This case study is produced as part of the OWASP HCI Cognitive Layer project and the ai-governance-case-studies repository. The analysis is based on publicly available information from source code analyses published following the exposure on March 31, 2026.*

*GitHub: [OWASP HCI Cognitive Layer](https://github.com/kingchemagnussonhr-sudo/Owasp-hci-cognitive-layer) | [AI Governance Case Studies](https://github.com/kingchemagnussonhr-sudo/ai-governance-case-studies)*
