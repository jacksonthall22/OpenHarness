# OpenHarness

**A language-agnostic repo harness for agentic engineering workflows.**

OpenHarness is a single `SPEC.md` file that turns an empty repository into an agent-ready engineering harness: structured, discoverable, and safe to modify.

## What it does

OpenHarness defines the **lower layer** every serious repo needs:

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

Example:

```txt
Read SPEC.md and turn this repository into the harness it describes.

Create only durable repo files. No app code unless required.

Summarize what you created and what’s left to decide.
```

## When to use it

- Starting a new project  
- Preparing a repo for coding agents  
- Standardizing workflows  
- Converting many repos into a monorepo  

## Core idea

A good repo should be:

- **Self-explanatory** → context lives in version control  
- **Self-checking** → commands + tests are obvious  
- **Self-improving** → confusion leads to better repo structure  

## Contributing

Feedback and PRs welcome.

---

## License

MIT
