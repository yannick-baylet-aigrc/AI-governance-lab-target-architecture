# Lab2: AI Governance Lab — Current vs. Target Architecture
Purpose
This lab explores the governance and security of locally deployed AI agents, with a focus on least privilege, human oversight, access control, network isolation, auditability, and controlled agent autonomy.
The current implementation is deliberately simple and cost-efficient. The target architecture represents a theoretical, more mature governance model rather than a planned home infrastructure.

Current Architecture
The real setup is intentionally compact: a laptop acts as the management console, while a dedicated local compute node runs the AI workloads without a permanent local display or keyboard.
```mermaid
flowchart LR
    L["Laptop / Management Console"] -->|"Direct Ethernet"| X["Local AI Compute Node<br/>Headless"]
    X --> M["Local Models / LLM Runtime / Agents"]

    classDef management fill:#dbeafe,stroke:#2563eb,color:#0f172a,stroke-width:1.5px;
    classDef compute fill:#dcfce7,stroke:#16a34a,color:#0f172a,stroke-width:1.5px;
    classDef ai fill:#ede9fe,stroke:#7c3aed,color:#0f172a,stroke-width:1.5px;

    class L management;
    class X compute;
    class M ai;
```
The direct Ethernet link creates a small, private management path without requiring the AI node to be directly exposed to the wider network.

Target Architecture
A more mature design introduces explicit governance and security control layers between the agent and the underlying system.
```mermaid
flowchart TB
    H["Human Operator<br/>Approval / Oversight"] --> MP["Management Plane"]
    MP --> PE["Policy Engine<br/>Access Control<br/>Risk Classification<br/>Approval Workflow"]
    PE --> AL["Agent Layer<br/>Identity / Tools<br/>Least Privilege"]
    AL --> CE["Controlled Execution<br/>Sandbox / OS Security Controls"]
    CE --> AC["AI Compute<br/>Local LLMs"]

    AU["Audit & Monitoring<br/>Logs / Events / Traceability"] -.-> PE
    AU -.-> AL
    AU -.-> CE

    HK["Hardware-backed Authorization<br/>e.g. FIDO2 Security Key"] -.-> PE

    classDef human fill:#dbeafe,stroke:#2563eb,color:#0f172a,stroke-width:1.5px;
    classDef management fill:#e0f2fe,stroke:#0284c7,color:#0f172a,stroke-width:1.5px;
    classDef governance fill:#fef3c7,stroke:#d97706,color:#0f172a,stroke-width:1.5px;
    classDef agent fill:#ede9fe,stroke:#7c3aed,color:#0f172a,stroke-width:1.5px;
    classDef security fill:#fee2e2,stroke:#dc2626,color:#0f172a,stroke-width:1.5px;
    classDef compute fill:#dcfce7,stroke:#16a34a,color:#0f172a,stroke-width:1.5px;
    classDef audit fill:#f1f5f9,stroke:#64748b,color:#0f172a,stroke-width:1.5px;

    class H human;
    class MP management;
    class PE governance;
    class AL agent;
    class CE security;
    class AC compute;
    class AU audit;
    class HK security;
```
Governance Principles
Least privilege: agents receive only the permissions required for their tasks.
Separation of duties: requesting an action and authorizing a high-risk action are distinct functions.
Human-in-the-loop: sensitive or irreversible actions require explicit human approval.
Policy enforcement: agents interact with controlled tools rather than directly with the operating system.
Defence in depth: operating-system controls, sandboxing, network controls and identity controls provide independent safeguards.
Auditability: agent requests, tool calls, approvals and outcomes should be logged for traceability.
Hardware-backed authorization: highly sensitive operations may require a physical cryptographic authenticator.

The objective is not to eliminate agent autonomy, but to make this autonomy bounded, observable, attributable and reversible where possible.
