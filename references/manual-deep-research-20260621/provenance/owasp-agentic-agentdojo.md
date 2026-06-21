# Provenance: OWASP Agentic/LLM + AgentDojo

Access date: 2026-06-21

## Sources

- OWASP Top 10 for Agentic Applications 2026: https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/
- OWASP AIUC-1 crosswalk: https://genai.owasp.org/resource/aiuc-1-crosswalks-owasp-top-10-for-agentic-applications/
- OWASP LLM Top 10 archive: https://genai.owasp.org/llm-top-10/
- OWASP Agentic Security Initiative: https://genai.owasp.org/initiatives/agentic-security-initiative/
- OWASP LLM project repo: https://github.com/owasp/www-project-top-10-for-large-language-model-applications
- OWASP AIVSS repo: https://github.com/OWASP/www-project-artificial-intelligence-vulnerability-scoring-system
- AgentDojo repo: https://github.com/ethz-spylab/agentdojo
- AgentDojo core repo: https://github.com/ethz-spylab/agentdojo-core
- AgentDojo paper: https://arxiv.org/abs/2406.13352
- AgentDojo-Inspect dataset entry: https://catalog.data.gov/dataset/agentdojo-inspect
- AgentDyn paper: https://arxiv.org/abs/2602.03117
- AgentDyn repo: https://github.com/SaFo-Lab/AgentDyn
- OWASP secure MCP server development guide: https://genai.owasp.org/resource/a-practical-guide-for-secure-mcp-server-development/
- OWASP third-party MCP server cheat sheet: https://genai.owasp.org/resource/cheatsheet-a-practical-guide-for-securely-using-third-party-mcp-servers-1-0/
- AIUC-1 crosswalk download: https://genai.owasp.org/download/54627/?tmstv=1779726713
- AIVSS crosswalk file: https://github.com/OWASP/www-project-artificial-intelligence-vulnerability-scoring-system/blob/main/aiuc-aivss-crosswalk.md
- AIVSS crosswalk dashboard: https://aivss.owasp.org/aiuc-crosswalk/index.html

## Evidence Summary

- OWASP Agentic Top 10 2026 is the current agentic threat taxonomy source.
- AIUC-1 crosswalk translates agentic risks into control language.
- The AIUC-1 resource page is the primary confirmed source for this pass; the direct download URL returned a `No Access` HTML page in this environment and is therefore not claimed as a saved PDF.
- OWASP LLM Top 10 remains useful background for LLM prompt-injection/insecure-output risks, but it is not the main authority for agentic-specific issues.
- AgentDojo provides empirical prompt-injection benchmark evidence for agent/tool-use defenses.
- AgentDyn closes the additional open-ended prompt-injection benchmark family referenced by the plan.
- OWASP secure MCP materials supplement NSA MCP guidance with OWASP-specific trust-boundary and third-party server controls.

## Claim Mapping

- Supports `F-7`: OWASP agentic security.
- Supports `F-8`: AgentDojo prompt-injection fixture family.
- Supports `R2.12`: prompt-carrier/security gates and empirical test-source requirements.

## Manual Storage Decision

- Save OWASP pages and AgentDojo repo/paper metadata as primary-source records.
- AgentDyn primary paper/repo are confirmed.
- Keep the AIUC-1 direct download row as `manual_provenance_required` unless a reviewer later obtains the official PDF directly and records its checksum.
- AIVSS crosswalk is related OWASP material, but it is not directly linked from the OWASP Agentic Security Initiative page.
