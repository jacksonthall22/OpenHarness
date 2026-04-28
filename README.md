# OpenHarness

**A language-agnostic repo harness for agentic engineering workflows.**

OpenHarness is a single `SPEC.md` file that turns a repository into an agent-ready engineering harness: structured, discoverable, self-improving, and safe to modify.

Built for use with [Symphony](https://openai/Symphony) as our engineering harness at [Hyperchess](https://hyperchess.ai).

## What it does

OpenHarness defines the **lower layer structure** repos should follow:

- Clear structure and conventions
- Discoverable commands (build, test, lint, etc.)
- Durable documentation and context
- Guardrails and verification workflows
- Agent + human-friendly instructions

Anything important that isn’t in the repo doesn’t exist to an agent. OpenHarness makes sure it does.

## Quick start

1. Copy `SPEC.md` into your repo
2. Ask an agent to implement it
3. Review and iterate  

Example prompt:

```txt
Read SPEC.md and execute it completely to turn this repository into the robust engineering harness it describes. If the repo is empty, just execute the plan and ignore below. Otherwise, adapt the current project to adhere strictly to the spec's requirements without changing any functionality. You may sort existing docs files into the canonical layout described, but do not modify them (only mv whole files). Do not change other files. If you feel strongly that something about the existing project needs to change to conform to the spec (accurate conformation is blocked), complete the implementation with as few tradeoffs as possible: maintain the spirit of the spec where possible while staying flexible to the local project conventions, and briefly state in your final message what should change and why. When done, reread the spec and do a comprehensive review pass to verify the repo is fully aligned with its requirements (use sub-agents if able). If residual changes are required, iterate until a fresh review is green. Finally, summarize what you changed or created.
```

## Contributing

Feedback and PRs welcome!

## License

MIT