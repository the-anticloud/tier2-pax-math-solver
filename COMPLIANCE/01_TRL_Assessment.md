# PAX Math Solver — TRL 8.0 & OWASP Assessment

**TRL: 8.0 — System complete and qualified**

## Benchmark Evidence

| Benchmark | Score | Verification |
|-----------|-------|-------------|
| GSM8K (5-shot, k=4 SC) | **100%** (50/50 sample) | PAX_RESULTS.md — H200 session |
| MATH-500 (k=4 SC) | **84%** (42/50) | PAX_RESULTS.md — H200 session |
| GSM8K (5-shot flexible) | **90.75%** | PAX_RESULTS_2.md — 2× RTX 3090 |
| GPQA-Diamond (n=25) | **64%** | PAX_RESULTS_2.md |

## OWASP / Security

**Code execution security:** SymPy executor runs in subprocess with:
- 10-second timeout (DoS prevention)
- No file I/O allowed (sandboxed env)
- No network access (no API keys passable from generated code)
- Allowlist imports: `sympy`, `math`, `fractions` only
- `exec()` with restricted `__builtins__` as secondary guard

**PII:** Math problems do not typically contain PII. PII scanner runs on input — if detected, problem is flagged before solving.

## No Frontier APIs

All inference via PAX_INFERENCE_CORE (local vLLM/llama.cpp). SymPy execution is pure CPU Python. No OpenAI/Anthropic/Gemini calls anywhere.
