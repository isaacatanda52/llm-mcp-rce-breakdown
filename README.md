# llm-mcp-rce-breakdown
Architectural analysis of indirect prompt injection and token confusion vulnerabilities within agentic tool-calling workflows.
## Vulnerability Mechanics: Why Content Filters Fail

Traditional Web Application Firewalls (WAFs) rely on signature-based filtering to block specific malicious keywords or payload syntax. In LLM-orchestrated environments, this approach fails due to **Token Confusion**.

### The Flattened Data Plane
An LLM processes input sequentially, blending developer instructions, system prompts, and untrusted external data into a single execution stream. 

* **The Illusion of Innocence:** Unlike a direct jailbreak where a user actively fights the system prompt, an indirect prompt injection embeds instructions inside a third-party source (e.g., a web page scrape or a repository markdown file).
* **Literal Execution:** The model interprets the entire sequence with the same intent: processing and execution. It reads the attacker's embedded payload, mistakes it for a legitimate instruction within the context of the workflow, and executes it literally.
* **Semantic Obfuscation:** Because the model evaluates the semantic meaning rather than strict strings, an attacker can completely avoid blocked words (like `execute` or `rm`) by using context-shifting language or multi-turn instructions to achieve the exact same malicious outcome.
