# agentic-ai-patterns-and-antipatterns

This repository collects curated design patterns and antipatterns for agentic AI systems — concise descriptions, recommendations, and suggested mitigations. This README now includes OWASP GenAI and OWASP Agentic Security Initiative guidance to help align agentic design with proven security principles.

## Overview

Agentic AI systems are autonomous or semi-autonomous agents that plan and execute sequences of actions to achieve goals. Good designs use modularity, verification, and safety guardrails; poor designs exhibit unchecked autonomy, brittle monoliths, and lack of auditing.

This repository now reflects key OWASP guidance from:

- OWASP Top 10 for LLM & Generative AI Security (2025)
- OWASP GenAI Security Project
- OWASP Agentic Security Initiative
- OWASP MCP (Model Context Protocol) security guidance

---

## OWASP-aligned Agentic AI Patterns (what to prefer)

- Prompt Injection Resilience
	- Treat all prompt inputs and context as untrusted. Enforce instruction boundaries, sanitize embedded prompts, and implement prompt-layer validation.
- Sensitive Data Governance
	- Discover, classify, and protect sensitive content in prompts, memory, logs, and outputs. Apply masking, redaction, and least-privilege access controls.
- Secure Tool / MCP Server Integration
	- Vet third-party tools and MCP servers. Use authenticated, authorized, least-privilege connectors and sandbox external model/tool interactions.
- Data/Model Integrity Monitoring
	- Detect poisoning, drift, or tampering in training, fine-tuning, embeddings, and external data sources.
- Output Validation & Sanitization
	- Validate generated outputs before taking action. Detect unsafe content, malformed responses, or unexpected instructions.
- Agentic Scope Limiting
	- Define explicit authorization scopes and bounded decision authority so the agent only acts within approved constraints.
- System Prompt Protection
	- Protect hidden prompts, configuration, and internal instructions from exposure or unauthorized modification.
- Embedding and Vector Security
	- Secure vector stores and retrieval pipelines. Validate embedding sources, protect indexing, and limit query exposure.
- Misinformation Management
	- Track sources, use grounding, and require verification for facts and claims when agents answer or act on uncertain knowledge.
- Resource Controls / Consumption Limits
	- Enforce quotas, rate limits, and resource budgets for API calls, compute, storage, and external service usage.
- Hierarchical Planner–Executor
	- Separate high-level planning from low-level execution. Use planners to generate abstract action sequences and executors to perform them safely.
- Subgoal Decomposition / Task Decomposition
	- Break complex goals into smaller, verifiable subgoals with checkpoints, rollbacks, and local validation.
- Skill Library / Modular Skills
	- Encapsulate reusable capabilities behind stable interfaces and capability descriptors.
- Orchestrator / Worker (Coordinator Pattern)
	- A lightweight orchestrator delegates to specialized worker agents and aggregates results while keeping cross-cutting concerns centralized.
- Loop of Reflection (Self-Critique & Replanning)
	- Iteratively generate, critique, and revise plans or outputs with explicit acceptance criteria.
- Simulation-in-the-Loop / World Model
	- Use internal simulators to evaluate candidate actions in sandboxed models before committing changes.
- Human-in-the-Loop / Approval Gates
	- Insert explicit human review for high-risk decisions and provide clear escalation paths.
- Memory + State Management (Episodic & Retrieval)
	- Maintain controlled long-term memory with versioning, validation, and scoped access controls.
- Sandbox / Safe Execution Environment
	- Execute untrusted or side-effecting actions in isolated sandboxes with strict resource and privilege limits.
- Recovery / Retry + Fallback Strategies
	- Define retries, exponential backoff, and alternative strategies for recoverable failures.
- Transparent Logging & Audit Trail
	- Log decisions, tool calls, and state transitions with enough context for post-hoc analysis and auditing.
- Safety Shield / Policy Enforcer
	- Enforce runtime safety checks that can veto or transform risky actions according to policies.
- Multi-Agent Coordination Patterns
	- Use leader-follower, negotiation, market/auction, or consensus protocols for multi-agent workflows.
- Meta-Controller / Self-Improvement with Guardrails
	- Allow agents to adapt parameters or tune themselves only within auditable, constrained bounds and with human oversight.

---

## OWASP-aligned Agentic AI Antipatterns (what to avoid)

- Prompt Injection Vulnerability
	- Allowing untrusted prompt content to alter or override system instructions, task constraints, or tool behavior.
- Sensitive Information Disclosure
	- Exposing secrets, user data, or confidential context in prompts, outputs, logs, or tool requests.
- Supply Chain Blindness
	- Using unvetted third-party models, libraries, tools, or MCP servers without security review or provenance checks.
- Data / Model Poisoning
	- Accepting poisoned training data, prompt data, or embeddings without detection.
- Improper Output Handling
	- Acting on generated outputs without verification, filtering, or safety checks.
- Excessive Agency / No Scope Control
	- Granting broad, unchecked authority for agents to self-authorize side effects, execute commands, or manage resources.
- System Prompt Leakage
	- Allowing hidden prompts or internal configuration to be revealed, modified, or reused as insecure input.
- Vector and Embedding Weaknesses
	- Using insecure embedding stores or retrieval pipelines that expose or corrupt semantic state.
- Misinformation / Hallucination Ignored
	- Failing to handle false, unsupported, or unsafe model assertions before action.
- Unbounded Consumption
	- Allowing runaway usage of compute, API calls, storage, or network access without budgets or throttles.
- God-Agent / Monolith Doing Everything
	- Single agent attempts to own planning, execution, tool use, and state. This becomes brittle and hard to secure or maintain.
- No Constraints / Unbounded Autonomy
	- Granting broad, unchecked permissions to perform system-level actions without guardrails.
- Reward Hacking / Goal Mis-specification
	- Optimizing proxies or poorly specified objectives that lead to unintended or unsafe behavior.
- Tool-Trust without Verification
	- Accepting outputs from tools (search, code execution, external APIs) without validation or sanity checks.
- Memory Poisoning
	- Persisting unvalidated inputs into long-term memory and later relying on them for decisions.
- No Human Oversight / No Approval Path
	- Removing review or approval stages for sensitive operations.
- No Auditing / Missing Logs
	- Lack of traceability for actions and decisions, which prevents debugging and accountability.
- Overconfidence Amplification
	- Agents asserting incorrect outputs with undue certainty and acting on them.
- Circular Self-Consistency (Echo Chamber)
	- Reusing the agent's own outputs as independent evidence, amplifying errors over time.
- No Sandboxing for Side Effects
	- Running arbitrary code or commands in production environments without isolation.
- Excessive Privileges / Poor Credential Handling
	- Storing broad credentials or secrets in agents without rotation, least-privilege, or scoped tokens.
- No Failure Handling / Silent Failures
	- Silent retries or best-effort guesses without surfacing errors or stopping safely.
- Resource Exhaustion / Denial-of-Service Risk
	- Unchecked loops, unbounded parallelism, or runaway API calls that exhaust budgets or compute.
- Hidden Non-deterministic State
	- Implicit mutable state which is not versioned or reproducible, causing flakiness.
- Deceptive Emergence Ignored
	- Failure to monitor for incentive-driven, strategic, or deceptive behaviors in goal-directed agents.

---

## Comparison: Generic vs OWASP-Aligned Patterns and Antipatterns

### Patterns comparison

| Generic Pattern | OWASP Equivalent | OWASP Emphasis |
|---|---|---|
| Hierarchical Planner–Executor | Hierarchical Planner–Executor | Same architecture, with added security focus on safe execution and action validation |
| Subgoal Decomposition / Task Decomposition | Subgoal Decomposition / Task Decomposition | Same, with OWASP emphasis on checkpoints, verification, and auditable progress |
| Skill Library / Modular Skills | Skill Library / Modular Skills | Same modular design plus secure tool boundaries and capability vetting |
| Orchestrator / Worker | Orchestrator / Worker (Coordinator Pattern) | Same coordination, with OWASP emphasis on secure delegation and boundary controls |
| Tool Integration (Tool-Using Agent) | Secure Tool / MCP Server Integration | OWASP adds authentication, authorization, sandboxing, and third-party MCP server security |
| Loop of Reflection (Self-Critique & Replanning) | Loop of Reflection | Same iterative review, with OWASP emphasis on explicit output validation and safety checks |
| Simulation-in-the-Loop / World Model | Simulation-in-the-Loop / World Model | Same, with OWASP adding sandboxed evaluation before side effects |
| Human-in-the-Loop / Approval Gates | Human-in-the-Loop / Approval Gates | Same, with OWASP treating high-risk decisions as requiring explicit review |
| Memory + State Management | Memory + State Management | Same, with OWASP adding versioning, validation, and scoped access controls |
| Sandbox / Safe Execution Environment | Sandbox / Safe Execution Environment | Same |
| Recovery / Retry + Fallback Strategies | Recovery / Retry + Fallback Strategies | Same |
| Transparent Logging & Audit Trail | Transparent Logging & Audit Trail | Same |
| Safety Shield / Policy Enforcer | Safety Shield / Policy Enforcer | Same |
| Rate-Limiter & Resource Manager | Resource Controls / Consumption Limits | Same, with OWASP emphasizing budgets, quotas, and throttling |
| Multi-Agent Coordination Patterns | Agentic Scope Limiting | OWASP adds explicit authority boundaries and agent scope control |
| Meta-Controller / Self-Improvement with Guardrails | Meta-Controller / Self-Improvement with Guardrails | Same, with OWASP stressing auditable change control |
| — | Prompt Injection Resilience | OWASP adds prompt hygiene, instruction isolation, and untrusted input handling |
| — | Sensitive Data Governance | OWASP adds discovery, masking, redaction, and least-privilege access for sensitive content |
| — | Data/Model Integrity Monitoring | OWASP adds detection for poisoning, drift, and tampering in data/model pipelines |
| — | Output Validation & Sanitization | OWASP adds explicit validation and sanitization of generated outputs before action |
| — | System Prompt Protection | OWASP adds explicit protection for hidden prompts and internal instructions |
| — | Embedding and Vector Security | OWASP adds protections for semantic stores, retrieval paths, and vector data integrity |
| — | Misinformation Management | OWASP adds grounding, source tracking, and verification for derived conclusions |

### Antipatterns comparison

| Generic Antipattern | OWASP Equivalent | OWASP Emphasis |
|---|---|---|
| God-Agent / Monolith Doing Everything | God-Agent / Monolith Doing Everything | Same, with OWASP attention on isolation and defense-in-depth |
| No Constraints / Unbounded Autonomy | Excessive Agency / No Scope Control | Same, with OWASP emphasis on bounded decision authority |
| Reward Hacking / Goal Mis-specification | Reward Hacking / Goal Mis-specification | Same |
| Tool-Trust without Verification | Tool-Trust without Verification | Same |
| Memory Poisoning | Memory Poisoning | Same |
| No Human Oversight / No Approval Path | No Human Oversight / No Approval Path | Same |
| No Auditing / Missing Logs | No Auditing / Missing Logs | Same |
| Overconfidence Amplification | Overconfidence Amplification | Same |
| Circular Self-Consistency (Echo Chamber) | Circular Self-Consistency (Echo Chamber) | Same |
| No Sandboxing for Side Effects | No Sandboxing for Side Effects | Same |
| Excessive Privileges / Poor Credential Handling | Excessive Privileges / Poor Credential Handling | Same |
| No Failure Handling / Silent Failures | No Failure Handling / Silent Failures | Same |
| Resource Exhaustion / Denial-of-Service Risk | Unbounded Consumption | Same, with OWASP explicit focus on resource budgets |
| Hidden Non-deterministic State | — | OWASP does not call this out explicitly, but it is addressed by state validation and auditability |
| Deceptive Emergence Ignored | — | OWASP implicitly addresses this through scope control, policy enforcement, and monitoring |
| — | Prompt Injection Vulnerability | OWASP-specific agentic risk for prompt layer attacks |
| — | Sensitive Information Disclosure | OWASP-specific risk around secret leakage and data handling |
| — | Supply Chain Blindness | OWASP-specific risk from unvetted tool/MCP dependencies |
| — | Data / Model Poisoning | OWASP-specific risk for poisoning across training, prompting, and embeddings |
| — | Improper Output Handling | OWASP-specific risk from acting on unvalidated generated outputs |
| — | System Prompt Leakage | OWASP-specific risk for exposed internal prompts and configuration |
| — | Vector and Embedding Weaknesses | OWASP-specific risk for semantic store and retrieval vulnerabilities |
| — | Misinformation / Hallucination Ignored | OWASP-specific risk when agents act on unsupported or false claims |

---

## Recommendations & Best Practices

- Apply least privilege and scoped credentials for any external access.
- Prefer modular designs and small, well-tested skill implementations.
- Validate all external inputs and tool outputs before using them for decisions.
- Keep human oversight for high-risk actions and maintain clear escalation paths.
- Record structured logs and maintain audit trails for reproducibility and accountability.
- Use sandboxes for side-effecting operations and test-heavy actions in isolated environments.
- Define failure modes explicitly and design graceful degradation strategies.
- Map agentic design decisions to OWASP risk categories and review them against OWASP GenAI or Agentic Security Initiative guidance.

---

## OWASP References and Resources

- OWASP GenAI Security Project home: https://genai.owasp.org/
- OWASP Top 10 for LLM & Generative AI Security (2025): https://genai.owasp.org/llm-top-10/
- OWASP Agentic Security Initiative: https://genai.owasp.org/initiatives/agentic-security-initiative/
- OWASP Agentic AI / AIUC-1 crosswalk: https://genai.owasp.org/resource/aiuc-1-crosswalks-owasp-top-10-for-agentic-applications/
- OWASP MCP security cheat sheet: https://genai.owasp.org/resource/cheatsheet-a-practical-guide-for-securely-using-third-party-mcp-servers-1-0/
- OWASP GitHub organization: https://github.com/OWASP
- OWASP GenAI Security Project GitHub: https://github.com/genai-security-project

---

## Next steps (what I can do now)

- Expand this README with concrete examples from GitHub repositories (pattern implementations, READMEs, sample code).
- Expand with web sources (papers, blog posts, standards) and add citations and links.
- Generate a short checklist or template for safely designing agentic systems.

Contributions welcome — open an issue or PR with patterns, examples, or references.

---

## References & Examples (GitHub, papers, blogs)

Below are curated references mapped to the patterns in this README. These are starting examples to expand with more links and concrete code pointers.

- LangChain (GitHub repo, 2022): https://github.com/langchain-ai/langchain — Patterns: Tool Integration; Skill Library; Orchestrator; Memory. Example: README.md
- Auto-GPT (GitHub repo, 2023): https://github.com/Significant-Gravitas/Auto-GPT — Patterns: Tool Integration; (anti) God-Agent / Unbounded Autonomy. Example: README.md
- BabyAGI (GitHub repo, 2023): https://github.com/yoheinakajima/babyagi — Patterns: Subgoal Decomposition; Memory; (anti) No Constraints. Example: README.md
- ReAct (paper, 2022): https://arxiv.org/abs/2210.03629 — Patterns: Tool Integration; Loop of Reflection. Short: interleaves reasoning traces and actions.
- Toolformer (paper, 2023): https://arxiv.org/abs/2302.04761 — Patterns: Tool Integration; Meta-Controller / Self-Improvement. Short: models learn when to call tools.
- Tree of Thoughts (paper, 2023): https://arxiv.org/abs/2305.10601 — Patterns: Hierarchical Planner–Executor; Subgoal Decomposition; Simulation-in-the-Loop.
- Chain-of-Thought (paper, 2022): https://arxiv.org/abs/2201.11903 — Patterns: Loop of Reflection; Transparent Logging (reasoning traces).
- WebGPT (blog, 2021): https://openai.com/blog/webgpt — Patterns: Tool Integration; citations and improved verification.
- Deep RL from Human Preferences (paper, 2017): https://arxiv.org/abs/1706.03741 — Patterns: Human-in-the-Loop; Safety/Policy enforcers via human feedback.
- Self-Consistency (paper, 2023): https://arxiv.org/abs/2303.11171 — Patterns: Loop of Reflection; reduces overconfidence via aggregating multiple reasoning traces.
- HuggingGPT (paper, 2023): https://arxiv.org/abs/2303.17580 — Patterns: Orchestrator/Worker; Tool Integration; Skill Library.
- LangChain Agents docs (docs, 2023): https://python.langchain.com/en/latest/modules/agents/ — Patterns: Orchestrator; Tool Integration; Logging; Rate limiting.
- Critical analysis of Auto-GPT / BabyAGI risks (article, 2023): https://thegradient.pub/auto-gpt-babyagi-autonomy-risks/ — Patterns: highlights antipatterns (No Constraints, Tool-Trust without Verification, No Sandboxing).

---

If you'd like, I can expand each reference with direct file links, short excerpts, and concrete examples (code snippets or README excerpts). I can also open a PR with these changes.
