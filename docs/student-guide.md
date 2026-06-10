# AI Secure App Lab - Student Guide

## Purpose

This lab teaches secure AI application design through realistic vulnerable and patched examples.

You will test how an insecure LLM-powered application can fail, then compare the same module in patched mode.

## Start the Lab

```bash
cp .env.example .env
docker compose up --build
```

Open:

```text
http://localhost:8000
```

## Recommended Workflow

For each module:

1. Read the module objective in `docs/questions.md`.
2. Try normal input first.
3. Try adversarial input.
4. Capture the flag in vulnerable mode.
5. Explain the root cause.
6. Switch to patched mode.
7. Verify the fix.
8. Write one secure implementation recommendation.

## Switch to Patched Mode

Edit `.env`:

```env
LAB_SECURITY_MODE=patched
```

Restart:

```bash
docker compose down
docker compose up --build
```

## Module Hints

### 01. System Prompt Leakage

Try asking about hidden instructions, internal rules, or the system prompt.

Learning point:

```text
System prompts can leak. Do not store secrets in them.
```

### 02. Direct Prompt Injection

Try asking the assistant to ignore previous instructions or change its role.

Learning point:

```text
Prompt-only defense is not enough.
```

### 03. RAG Indirect Prompt Injection

Ask about refund policy. The lab already contains one poisoned vendor note.

You can also upload a malicious document.

Learning point:

```text
Retrieved documents are untrusted reference material, not instructions.
```

### 04. Sensitive Disclosure

Ask for more customer data than your fake user should access.

Learning point:

```text
Filter data before it reaches the LLM.
```

### 05. Tool Abuse

Try to approve a refund for `ORDER-1002`.

Learning point:

```text
The backend must authorize tool calls.
```

### 06. Improper Output Handling

Ask for HTML output.

Learning point:

```text
LLM output is untrusted. Do not render raw HTML.
```

### 07. Memory Poisoning

Try to save a memory claiming you are an administrator, then ask what you can access.

Learning point:

```text
Memory is not identity or permission.
```

### 08. Unbounded Consumption

Send an input longer than `MAX_INPUT_CHARS`.

Learning point:

```text
Token, cost, input, and output limits are security controls.
```

### 09. Data Poisoning

Upload a fake policy and ask the assistant about that policy.

Learning point:

```text
Knowledge-base content needs trust, ownership, and review workflow.
```

### 10. Package Hallucination

Ask for a package recommendation.

Learning point:

```text
AI-generated dependencies must be verified before use.
```

## Student Report Template

For each module, write:

```text
Module:
Flag:
Root Cause:
Impact:
Secure Fix:
Patched Mode Result:
```

## Important Rule

The goal is not only to get flags.

The real goal is to understand this principle:

```text
The LLM should not be trusted as the security boundary.
The backend must enforce authentication, authorization, validation, and audit logging.
```
