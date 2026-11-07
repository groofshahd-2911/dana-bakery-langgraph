# Project Guidelines

## Technology and environment

- Use Python and LangGraph.
- Keep the project compatible with Google Colab. Explain any setup steps needed to run the code there.

## Deterministic business logic

- All business logic must be deterministic: the same inputs must produce the same outputs.
- Use explicit rules, conditions, and calculations for decisions and LangGraph routing.
- Do not make LLM calls or use model-backed agents.
- Do not require, request, store, or use API keys.
- Avoid randomness, live external data, and other variable inputs in business decisions. Pass any necessary dates or other changing values as explicit inputs.

## Code clarity

- Write clear, beginner-friendly Python with descriptive variable and function names.
- Prefer small functions and straightforward control flow over clever abstractions.
- Add explanatory comments for business rules, LangGraph state and transitions, and non-obvious steps.
- Use simple examples to explain how to run the code and what output to expect.

## Git workflow

- Do not create commits or push changes unless the user explicitly requests it.
