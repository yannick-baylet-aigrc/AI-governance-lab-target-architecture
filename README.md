# Lab2: AI Governance Lab — Current vs. Target Architecture
Purpose
This lab explores the governance and security of locally deployed AI agents, with a focus on least privilege, human oversight, access control, network isolation, auditability, and controlled agent autonomy.
The current implementation is deliberately simple and cost-efficient. The target architecture represents a theoretical, more mature governance model rather than a planned home infrastructure.

Current Architecture
The real setup is intentionally compact: a laptop acts as the management console, while a dedicated local compute node runs the AI workloads without a permanent local display or keyboard.
```
+--------------------------+         +-------------------------------+         +-----------------------------+
|  Laptop / Management     | ------->|   Local AI Compute Node       | ------->|  Local Models / LLM Runtime |
|         Console          |  (Ether |          (Headless)           |         |          & Agents           |
+--------------------------+   net)  +-------------------------------+         +-----------------------------+

```
The direct Ethernet link creates a small, private management path without requiring the AI node to be directly exposed to the wider network.

Target Architecture
A more mature design introduces explicit governance and security control layers between the agent and the underlying system.
```

[ Human Operator ]
       (Approval / Oversight)
             │
             v
      [ Management Plane ] ──────────────┐
                                         v
      [ Hardware-backed Auth ] ────> [ Policy Engine ]
      (e.g., FIDO2 Security Key)     (Access Control / Risk / Workflow)
                                         │
                                         v
                                   [ Agent Layer ]
                               (Identity / Least Privilege)
                                         │
                                         v
                               [ Controlled Execution ]
                               (Sandbox / OS Security Controls)
                                         │
                                         v
                                   [ AI Compute ]
                                   (Local LLMs)

      [ Audit & Monitoring ] ──────> (Tracks Logs, Events & Traceability across 
       (Logs / Events)                Policy, Agent & Execution layers)

```
## Governance Principles

* **Least Privilege:** Agents receive only the permissions required to execute their specific tasks.
* **Separation of Duties:** Requesting an action and authorizing a high-risk operation are handled as distinct, isolated functions.
* **Human-in-the-Loop:** Sensitive, destructive, or irreversible actions require explicit human approval before execution.
* **Policy Enforcement:** Agents must interact through controlled tools and APIs rather than directly manipulating the operating system.
* **Defense in Depth:** Operating-system controls, sandboxing, network isolation, and identity verification provide independent layers of protection.
* **Auditability:** All agent requests, tool calls, authorizations, and execution outcomes are logged for full traceability.
* **Hardware-Backed Authorization:** Highly sensitive or critical operations mandate physical confirmation via a cryptographic hardware key.

The objective is not to eliminate agent autonomy, but to make autonomy **bounded, observable, attributable, and reversible where possible**.
