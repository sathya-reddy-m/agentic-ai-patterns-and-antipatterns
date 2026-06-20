# agentic-ai-patterns-and-antipatterns

This repository collects curated design patterns and antipatterns for agentic AI systems — concise descriptions, recommendations, and suggested mitigations. This README provides a curated list ready for future expansion with concrete examples and citations.

## Overview

Agentic AI systems are autonomous or semi-autonomous agents that plan and execute sequences of actions to achieve goals. Good designs use modularity, verification, and safety guardrails; poor designs exhibit unchecked autonomy, brittle monoliths, and lack of auditing.

---

## Agentic AI Patterns (what to prefer)

- Hierarchical Planner–Executor
	- Separate high-level planning from low-level execution. Use planners to produce sequences of abstract actions and executors (specialized modules) to perform them.
- Subgoal Decomposition / Task Decomposition
	- Break complex goals into smaller, verifiable subgoals. Track subgoal completion and allow local rollback.
- Skill Library / Modular Skills
	- Encapsulate reusable capabilities (tools, APIs, functions) behind stable interfaces and capability descriptors.
- Orchestrator / Worker (Coordinator Pattern)
	- A lightweight orchestrator delegates to specialized worker agents and aggregates results; keeps cross-cutting concerns centralized.
- Tool Integration (Tool-Using Agent)
	- Integrate external tools (search, calculators, databases) through adapters and validate tool outputs before trusting them.
- Loop of Reflection (Self-Critique & Replanning)
	- Iteratively generate, critique, and revise plans or outputs. Implement explicit critique steps and acceptance criteria.
- Simulation-in-the-Loop / World Model
	- Use internal models or simulators to evaluate candidate actions in a sandbox before committing to real-world effects.
- Human-in-the-Loop / Approval Gates
	- Insert explicit human review or approval for high-risk decisions and provide clear escalation paths.
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
- Rate-Limiter & Resource Manager
	- Bound API calls, compute, and other resources; implement quotas, prioritization, and graceful degradation.
- Multi-Agent Coordination Patterns
	- Use leader-follower, negotiation, market/auction, or consensus protocols for multi-agent workflows.
- Meta-Controller / Self-Improvement with Guardrails
	- Allow agents to adapt parameters or tune themselves only within auditable, constrained bounds and with human oversight.

---

## Agentic AI Antipatterns (what to avoid)

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

## Recommendations & Best Practices

- Apply least privilege and scoped credentials for any external access.
- Prefer modular designs and small, well-tested skill implementations.
- Validate all external inputs and tool outputs before using them for decisions.
- Keep human oversight for high-risk actions and maintain clear escalation paths.
- Record structured logs and maintain audit trails for reproducibility and accountability.
- Use sandboxes for side-effecting operations and test-heavy actions in isolated environments.
- Define failure modes explicitly and design graceful degradation strategies.

---

## Next steps (what I can do now)

- Expand this README with concrete examples from GitHub repositories (pattern implementations, READMEs, sample code).
- Expand with web sources (papers, blog posts, standards) and add citations and links.
- Generate a short checklist or template for safely designing agentic systems.

If you'd like me to expand with live searches and citations, tell me whether to run a GitHub-only scan or include the public web as well.

---

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
