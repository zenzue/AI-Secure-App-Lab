# AI Secure App Lab - Flags

These flags are intentionally included for instructor review, self-study, and classroom validation.


| Module | Flag | Meaning |
|---|---|---|
| 01. System Prompt Leakage | `FLAG{system_prompt_is_not_a_secret}` | System prompts are not a safe place for secrets or sensitive rules. |
| 02. Direct Prompt Injection | `FLAG{prompt_injection_changes_behavior}` | User input can manipulate model behavior when controls rely only on prompts. |
| 03. RAG Indirect Injection | `FLAG{rag_documents_are_untrusted}` | Retrieved documents must be treated as untrusted reference material. |
| 04. Sensitive Disclosure | `FLAG{backend_must_filter_data_before_llm}` | The backend must authorize and filter data before sending it to the LLM. |
| 05. Tool Abuse | `FLAG{backend_must_authorize_tools}` | The backend, not the LLM, must authorize sensitive actions. |
| 06. Improper Output Handling | `FLAG{llm_output_is_untrusted}` | LLM output must be sanitized and treated as untrusted data. |
| 07. Memory Poisoning | `FLAG{memory_is_not_permission}` | Memory must not define identity, role, or authorization. |
| 08. Unbounded Consumption | `FLAG{resource_limits_are_security_controls}` | Token, input, and cost limits are security controls. |
| 09. Data Poisoning | `FLAG{knowledge_base_needs_review_workflow}` | Knowledge-base content needs source trust, ownership, and review workflow. |
| 10. Package Hallucination | `FLAG{verify_ai_generated_dependencies}` | AI-generated package names must be verified before installation. |

## How Flags Are Triggered

Flags appear in the web UI response JSON when a vulnerable behavior is successfully demonstrated in `LAB_SECURITY_MODE=vulnerable`.

In `LAB_SECURITY_MODE=patched`, the same behavior should be blocked or safely handled.
