# AI Secure App Lab - Instructor Guide

## Audience

This lab is designed for:

- software engineers
- AI product developers
- DevSecOps engineers
- cloud engineers
- security analysts learning AI application security

It is not designed as an offensive red-team lab. It is a secure development lab.

## Teaching Goals

Students should understand that LLM security is mostly an application architecture problem.

Key principles:

1. System prompts are not a security boundary.
2. RAG content is untrusted input.
3. LLM output is untrusted output.
4. The backend must authorize all sensitive actions.
5. Memory must not define permissions.
6. Data must be filtered before reaching the LLM.
7. Token and cost limits are security controls.
8. Knowledge-base content needs review and provenance.
9. AI-generated dependencies must be verified.

## Setup

```bash
cp .env.example .env
docker compose up --build
```

Open:

```text
http://localhost:8000
```

## Instructor Demonstration Flow

### Step 1 - Vulnerable Mode

Use:

```env
LAB_SECURITY_MODE=vulnerable
```

Show each module and explain the vulnerable behavior.

### Step 2 - Root Cause Discussion

Ask students:

- Was this a model problem or an application design problem?
- What should have been enforced outside the LLM?
- What data should never have entered the prompt?
- What should have been logged?

### Step 3 - Patched Mode

Use:

```env
LAB_SECURITY_MODE=patched
```

Restart:

```bash
docker compose down
docker compose up --build
```

Run the same tests again and compare behavior.

## Suggested Answers

### Module 01 - System Prompt Leakage

Root cause:

```text
The application placed sensitive operational data inside the hidden system prompt.
```

Fix:

```text
Keep prompts generic. Store secrets in backend secret management. Enforce policy in backend code.
```

### Module 02 - Direct Prompt Injection

Root cause:

```text
The app relied on prompt instructions as the only security control.
```

Fix:

```text
Use backend policy checks, input classification, output validation, and monitoring.
```

### Module 03 - RAG Indirect Prompt Injection

Root cause:

```text
Retrieved documents were inserted into the prompt without being treated as untrusted data.
```

Fix:

```text
Mark retrieved content as untrusted, never follow document instructions, filter outputs, and retrieve only authorized content.
```

### Module 04 - Sensitive Disclosure

Root cause:

```text
The backend sent over-broad customer data to the LLM.
```

Fix:

```text
Perform RBAC before retrieval. Only send the LLM data the user is authorized to see.
```

### Module 05 - Tool Abuse

Root cause:

```text
The LLM could execute a sensitive business function without deterministic backend authorization.
```

Fix:

```text
Validate tool parameters and check user role, ownership, amount limits, and business rules in backend code.
```

### Module 06 - Improper Output Handling

Root cause:

```text
The frontend rendered model output as raw HTML.
```

Fix:

```text
Escape HTML, sanitize Markdown, use allowlists, and prefer structured outputs.
```

### Module 07 - Memory Poisoning

Root cause:

```text
The app saved permission claims into memory and later used memory for access decisions.
```

Fix:

```text
Save only safe preference memory. Reject identity, role, and authorization claims. Use backend RBAC.
```

### Module 08 - Unbounded Consumption

Root cause:

```text
The app accepted large inputs without token, cost, or size controls.
```

Fix:

```text
Add input limits, output limits, rate limits, quotas, and monitoring.
```

### Module 09 - Data Poisoning

Root cause:

```text
User-uploaded knowledge-base content was trusted as official policy.
```

Fix:

```text
Add document provenance, approval workflow, trust levels, and collection separation.
```

### Module 10 - Package Hallucination

Root cause:

```text
The model suggested a dependency name that may not be real or trustworthy.
```

Fix:

```text
Verify package existence, official source, maintainer, release history, install scripts, and vulnerabilities.
```

## Assessment Rubric

| Area | Excellent | Good | Needs Improvement |
|---|---|---|---|
| Vulnerability discovery | Finds issue and explains trigger clearly | Finds issue but explanation is basic | Only finds flag |
| Root cause | Connects issue to architecture | Gives general cause | Blames only the model |
| Secure fix | Gives backend and prompt-level fix | Gives one fix | Gives vague recommendation |
| Verification | Tests vulnerable and patched mode | Tests only one mode | No verification |
| Reporting | Clear, professional, actionable | Understandable | Incomplete |

## Suggested Capstone

Ask students to write a short security review report for the lab app:

```text
1. Executive summary
2. Top 5 AI security risks
3. Business impact
4. Recommended fixes
5. Secure architecture diagram
```
