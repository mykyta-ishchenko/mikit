---
name: ai-security
description: Use when reviewing code that builds prompts, calls a model API, handles model output, or gives a model tools.
---

# LLM Security Review

Review code changes for LLM-specific security vulnerabilities. Focus on real exploitability, not theoretical risks.

## When to Use

Code that:
- Constructs prompts from user input
- Passes user data to LLM APIs
- Processes or displays LLM responses
- Stores or logs LLM interactions
- Uses function calling / tool use with LLMs

## Categories

### Prompt Injection
- User input directly interpolated into system prompts without sanitization
- Missing input/output boundaries between system instructions and user content
- Concatenation of untrusted data into prompt templates
- Lack of role separation (system vs user messages)

### Data Exfiltration
- LLM responses rendered without sanitization (markdown injection, link injection)
- Tool/function calls that could be manipulated by prompt injection to access unauthorized data
- LLM output used in subsequent API calls without validation

### Sensitive Data Exposure
- PII or secrets passed to LLM APIs without redaction
- User data from other users leaking into prompts (multi-tenant isolation)
- Conversation history shared across users or sessions
- LLM API keys or tokens exposed in client-side code

### Unsafe Output Handling
- LLM output executed as code (eval, exec, subprocess)
- LLM output used in database queries without parameterization
- LLM output rendered as HTML without escaping
- LLM-generated file paths used in file system operations

### Authorization Bypass
- LLM tools/functions that bypass normal auth checks
- User able to instruct LLM to perform actions beyond their permissions
- Missing permission validation on LLM-triggered operations

## Methodology

1. **Trace data flow** — follow user input from entry point through prompt construction to LLM API call
2. **Check boundaries** — verify system/user message separation, input sanitization
3. **Review output handling** — how LLM responses are processed, stored, displayed
4. **Check tool use** — if LLM has tools/functions, verify they respect auth boundaries
5. **Multi-tenant isolation** — verify user A cannot access user B's data through LLM

## Output Format

Same as `/security-review` — markdown with file, line, severity, description, exploit scenario, recommendation.

## Exclusions

- Theoretical prompt injection without concrete exploit path
- LLM hallucination or accuracy issues (not a security concern)
- Cost/billing abuse via excessive API calls (DoS category)
- Model bias or fairness issues
