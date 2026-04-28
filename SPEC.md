# Repository Harness Specification

Status: Draft v1  
Audience: humans and coding agents initializing or maintaining a repository  
Scope: language-agnostic repository harness for an empty Git repository

## 1. Purpose

This document is the root contract for a repository harness. It defines the durable repository-local operating substrate that makes future software work legible, bounded, verifiable, and recoverable for humans and coding agents.

A harness exists because coding agents can only reliably use context that is discoverable from the repository or from explicitly available tools. Relevant information that lives only in chat history, private memory, meeting notes, external docs, ad hoc prompts, or unstated human expectations effectively does not exist to the next agent working in the repository.

The harness turns that constraint into an engineering practice. It converts an empty Git repository into a self-propagating working environment: a small set of root instructions, indexed durable docs, discoverable commands, policies, templates, checks, and knowledge-ingestion paths that preserve what future agents need to know.

This work is REQUIRED, not polish. In agentic development, repository hygiene, command consistency, guardrails, tests, architecture notes, and durable context are the infrastructure that lets agents move quickly without constant human correction. The harness is the layer that makes later application work safe to delegate.

A coding agent SHOULD be able to receive this file in an empty Git repository and create the initial harness commit without needing any prior chat history, external prompt, meeting notes, or unstored context. A future human or agent SHOULD then be able to clone the resulting repository, read only the committed files, and understand how to maintain and evolve the harness.

This file is both:

1. A normative repository artifact that MUST be committed to the repository root as `spec.md`.
2. A prompt-like initialization contract that can be reused to create the first commit of future repositories.

The harness is not application code, a framework choice, a project-management system, or an agent orchestrator. It is the repository-local contract around future application code: the substrate that makes all later work discoverable, scoped, reviewable, testable, and recoverable.

## 2. Harness Definition

Status: Concrete Now

A repository harness is the durable, versioned, repository-local operating substrate that tells humans and coding agents how work happens in the repository. It is the set of committed files, directories, commands, policies, templates, checks, and knowledge artifacts that makes the repository understandable without relying on unstored context.

At minimum, the harness tells humans and agents:

- where to start;
- what rules govern the repository;
- what commands prove a change is correct;
- what boundaries MUST NOT be crossed;
- what business, product, architecture, operational, support, design, security, and domain context matters;
- where that context lives;
- how to discover context progressively instead of reading a giant manual;
- how to record plans and decisions;
- how to distinguish durable context from temporary task notes;
- how to promote external knowledge into repository-local memory;
- how to update the repository when assumptions change;
- how to determine whether a change is complete;
- how to recover from failed or partial work;
- how to preserve and improve the harness itself as the project evolves.

### 2.1 What the harness is

The harness is:

- **A root contract**: `spec.md` defines the normative rules and REQUIRED starter shape.
- **An agent map**: `AGENTS.md` stays short and points agents to the right durable docs, commands, plans, and policies.
- **Durable memory**: `docs/` stores project context, decisions, command definitions, policies, quality rules, plans, and known risks.
- **A command surface**: `docs/commands.md` tells agents how to set up, validate, test, build, inspect, repair, and maintain the repository.
- **A knowledge-ingestion loop**: external or tacit knowledge that future work depends on is summarized, cited when safe, indexed, and committed in the right place.
- **A feedback loop**: when agents fail because context, tools, checks, boundaries, or instructions were missing, the repository is updated so the next agent naturally discovers the fix.
- **A specialization scaffold**: the harness does not choose a language, framework, package manager, CI vendor, cloud provider, product domain, or deployment model up front, but it defines how those choices MUST be introduced and documented later.

### 2.2 What the harness is not

The harness is not:

- application code;
- a framework choice;
- a package manager choice;
- a CI vendor choice;
- a cloud provider choice;
- a project-management system;
- an issue tracker;
- an agent scheduler;
- a hosted agent runtime;
- a multi-agent orchestration protocol;
- a dashboard;
- a replacement for human review or judgment.

Those systems MAY use the harness, but they are not the harness. The harness remains repository-local and MUST still make sense when read from a fresh clone.

### 2.3 Core operating premise

The core premise is: to an agent, anything not discoverable from the repository or from explicitly available tools effectively does not exist.

Therefore:

- relevant business, product, architecture, operational, support, design, and domain knowledge MUST be converted into durable repository-local artifacts when future work depends on it;
- those artifacts MUST be structured, indexed, cross-linked, scoped, and easy to discover;
- `AGENTS.md` MUST remain a short map, not a giant manual;
- deeper context MUST live in `docs/` and be reachable through progressive disclosure;
- repeated agent failures MUST be treated as harness defects when caused by missing context, unclear boundaries, missing tools, missing checks, or unverifiable assumptions;
- the durable fix for such failures is to add the missing context, command, policy, template, invariant, or mechanical check where a future agent will naturally find it.

Correcting one agent run without updating the repository is not enough when the same missing context could affect later work.

### 2.4 Boundary with external automation

The harness MAY be ready for external automation by exposing stable repository-local interfaces: docs, command names, checklists, templates, status files, and validation commands. It MUST NOT depend on any external orchestrator for its own meaning.

The harness is in scope for this specification. The following are out of scope for the harness itself:

- agent scheduling;
- issue-tracker state machines;
- background task watchers;
- hosted agent runtimes;
- multi-agent dispatch policies;
- dashboard UX;
- pull-request landing automation beyond repository-local review expectations;
- product-specific application code;
- choosing a programming language, framework, package manager, CI vendor, cloud provider, or deployment target.

## 3. Normative Language

The key words `MUST`, `MUST NOT`, `REQUIRED`, `SHOULD`, `SHOULD NOT`, `RECOMMENDED`, `MAY`, and
`OPTIONAL` in this document are to be interpreted as described in RFC 2119.

`Implementation-defined` means the behavior is part of the implementation contract, but this
specification does not prescribe one universal policy. Implementations MUST document the selected
behavior.

## 4. Harness Coverage Model

Status: Concrete Now

Every harness concern is classified so agents can distinguish what MUST exist on day one from what MUST be specialized later.

### 4.1 Classifications

- **Concrete Now**: MUST exist in the initial empty-repository harness as actual files, directories, commands, policies, templates, or checks.
- **Persisted Guidance Now**: MUST be saved in durable repository files so future agents can maintain the harness without access to the original prompt or chat.
- **Meta-Specified Now**: MUST be specified immediately as a decision process, template, or invariant, but cannot be fully instantiated until the repository chooses a language, product domain, deployment model, business domain, or team structure.
- **Conditional Module**: REQUIRED only when the repository gains a capability such as deployable services, public APIs, database migrations, generated code, data pipelines, regulated secrets, multiple packages, release artifacts, support workflows, or operational runbooks.
- **Knowledge-Ingestion Requirement**: governs how external or tacit knowledge is converted into versioned, discoverable, repository-local context.
- **Control-Plane Boundary**: relevant to agentic workflows, but belongs above the repository harness and MUST NOT be implemented as part of the harness.
- **Out of Scope**: not appropriate for this harness.

### 4.2 Concern Classification Table

| Concern | Classification | Durable artifact |
|---|---:|---|
| Root harness contract | Concrete Now | `spec.md` |
| Short agent map | Concrete Now | `AGENTS.md` |
| Human entrypoint | Concrete Now | `README.md` |
| Documentation index | Concrete Now | `docs/index.md` |
| Status summary | Concrete Now | `docs/status.md` |
| Glossary | Concrete Now | `docs/glossary.md` |
| Initial architecture map | Concrete Now | `docs/architecture.md` |
| Command registry | Concrete Now | `docs/commands.md` |
| Knowledge system of record | Concrete Now | `docs/context/README.md`, `docs/policies/knowledge-ingestion.md` |
| Business, product, operations, support context slots | Concrete Now as placeholders; Knowledge-Ingestion Requirement for contents | `docs/context/*.md` |
| External source register | Concrete Now | `docs/context/external-sources.md` |
| Decision log process | Concrete Now | `docs/decisions/README.md`, `docs/decisions/template.md` |
| Plan process | Concrete Now | `docs/plans/README.md`, `docs/plans/template.md` |
| Active and completed plan storage | Concrete Now | `docs/plans/active/`, `docs/plans/completed/` |
| Security policy | Concrete Now | `docs/policies/security.md` |
| Configuration and environment policy | Concrete Now | `docs/policies/configuration.md` |
| Dependency policy | Concrete Now | `docs/policies/dependencies.md` |
| Testing policy | Concrete Now | `docs/policies/testing.md` |
| Documentation lifecycle | Concrete Now | `docs/policies/documentation.md` |
| Generated files policy | Concrete Now | `docs/policies/generated-files.md` |
| Quality checklist | Concrete Now | `docs/quality/checklist.md` |
| Drift cleanup | Concrete Now | `docs/quality/drift.md` |
| Harness health check | Concrete Now | `docs/quality/harness-health.md`, `scripts/check-harness.sh` |
| Agent failure feedback loop | Concrete Now | `docs/quality/failure-feedback.md` |
| Source tree layout | Meta-Specified Now | `docs/architecture.md` |
| Package/module boundaries | Meta-Specified Now | `docs/architecture.md` plus future package docs |
| Language-specific setup | Meta-Specified Now | future language-specific docs and command entries |
| CI implementation | Meta-Specified Now | `docs/commands.md`, future CI config |
| Type/schema/contract strategy | Meta-Specified Now | `docs/policies/testing.md`, future schemas |
| Parse-don't-validate boundary policy | Persisted Guidance Now | `docs/policies/testing.md`, future architecture docs |
| Error handling, logging, observability | Meta-Specified Now | `docs/architecture.md`, future service docs |
| Release/versioning policy | Conditional Module | future `docs/policies/release.md` |
| Database migrations | Conditional Module | future `docs/policies/migrations.md` |
| Data/artifact storage | Conditional Module | future `docs/policies/data-artifacts.md` |
| Public APIs | Conditional Module | future `docs/api/` and generated references |
| Deployable services | Conditional Module | future `docs/operations/` or `docs/runbooks/` |
| Customer support workflows | Conditional Module | future `docs/context/support.md` and runbooks |
| Ownership/CODEOWNERS | Conditional Module | future `CODEOWNERS` and ownership docs |
| External task assignment, scheduling, retries | Control-Plane Boundary | not stored as harness implementation |
| Agent runtime protocol | Control-Plane Boundary | not stored as harness implementation |
| Hosted dashboard | Out of Scope | none |

## 5. Persistence / Self-Propagation Model

Status: Concrete Now

### 5.1 Source of truth

`spec.md` is the root normative harness contract. It MUST be committed in the repository root in the initial commit.

The repository MUST NOT rely on chat history, unstored prompts, private notebooks, external docs, meeting notes, or human memory for any rule REQUIRED to maintain the harness.

When a future-critical rule is discovered, it MUST be promoted into one of:

- `spec.md` for root normative harness rules;
- `AGENTS.md` for short navigational agent guidance;
- `docs/` for durable project knowledge;
- templates for repeated work;
- checklists for reviewable processes;
- command registries for validation routines;
- scripts, tests, linters, CI jobs, or structural checks for mechanical enforcement.

### 5.2 `spec.md` as initializer prompt

This file MAY be given to a coding agent inside an empty Git repository with the instruction: “Initialize this repository according to `spec.md`.”

The agent MUST then create the initial commit shape listed in Section 18. It MUST preserve this file unchanged except for repository-specific metadata explicitly allowed by the initializing human.

### 5.3 What belongs in each artifact

`spec.md` contains:

- the harness definition;
- normative rules;
- coverage classification;
- persistence rules;
- repository knowledge rules;
- REQUIRED initial file tree;
- rules for future specialization;
- acceptance criteria for the harness.

`AGENTS.md` contains:

- a short map for agents;
- the first files to read;
- forbidden actions;
- how to find commands;
- how to decide when work is done;
- where to put plans, decisions, and new context.

`AGENTS.md` MUST remain short. It MUST point to deeper docs rather than duplicate them. If it grows beyond roughly 150 lines or becomes hard to scan, it MUST be reduced and supporting detail moved into `docs/`.

`docs/` contains durable memory:

- command registry;
- status;
- architecture map;
- glossary;
- product, business, operations, support, and external-source context;
- decisions;
- plans;
- policies;
- quality and drift records.

Templates contain repeated formats. Checklists contain repeatable review procedures. Scripts and future CI checks contain mechanical enforcement.

### 5.4 Update triggers

Any future change that adds or changes one of the following MUST update the relevant durable harness artifact in the same change:

- language, framework, package manager, runtime, or build tool;
- command names or validation workflow;
- architecture boundaries;
- package/module layout;
- configuration or environment variables;
- secrets handling;
- dependencies;
- public APIs or schemas;
- generated files;
- database migrations;
- release/versioning policy;
- deployment or operations model;
- product requirements;
- business rules;
- support workflows;
- domain vocabulary;
- ownership or review policy;
- security posture;
- data/artifact storage;
- agent permissions;
- recurring failure patterns.

A change is incomplete if it changes behavior but leaves the durable docs, commands, templates, or checks stale.

### 5.5 Decomposition without losing normative force

If `spec.md` becomes too large for routine agent context, it MAY be decomposed into a short root contract plus linked detailed documents. Decomposition is allowed only if:

1. the root `spec.md` still states that it is normative;
2. every delegated document is listed in `docs/index.md`;
3. each delegated document states its status, owner, and scope;
4. the root contract says which delegated document governs each topic;
5. `AGENTS.md` points agents to the root contract and index, not to an uncatalogued pile of files;
6. mechanical checks verify REQUIRED delegated files exist.

## 6. Repository Knowledge Model

Status: Knowledge-Ingestion Requirement

Repository-local, versioned knowledge is the system of record for agentic development. If future work depends on business context, product context, architecture decisions, operational constraints, design principles, support learnings, domain vocabulary, schemas, generated references, quality scores, known risks, or technical debt, that context MUST be discoverable from the repository or from an explicitly documented tool path.

### 6.1 Knowledge that belongs in the repository

The repository SHOULD contain durable summaries of:

- product requirements and non-goals;
- business rules and commercial assumptions;
- architecture decisions and constraints;
- operational constraints and runbooks;
- support learnings and common user failures;
- design principles and UX rules;
- security assumptions;
- domain vocabulary;
- schemas, APIs, and generated references;
- quality scores and known gaps;
- technical debt and cleanup plans;
- external sources that were used to make repository decisions.

### 6.2 Knowledge ingestion path

When relevant knowledge originates outside the repository, the agent or human MUST decide whether it is:

- durable context that future work will need;
- temporary task context that expires after the current change;
- sensitive information that MUST NOT be committed;
- irrelevant information that SHOULD NOT enter the harness.

Durable context MUST be summarized into the appropriate repository file. The summary MUST include:

- source description;
- source link or citation when safe;
- date captured;
- owner or responsible role when known;
- verification status;
- freshness expectation;
- scope of relevance;
- sensitive material omitted or redacted.

External knowledge MUST NOT be pasted wholesale when a concise, source-linked summary is enough. The goal is durable legibility, not dumping raw context into the repo.

### 6.3 Indexing and progressive disclosure

The repository MUST avoid giant manuals. Use maps and indexes:

- `AGENTS.md` maps agents to root docs.
- `docs/index.md` maps all durable docs.
- `docs/context/README.md` maps business, product, operations, support, and external sources.
- Each directory with durable process meaning SHOULD include a `README.md`.
- Long context docs SHOULD begin with a short “When to read this” section.

Agents SHOULD start from the smallest relevant map, then follow links only as needed.

### 6.4 Verification status

Durable context docs SHOULD mark entries with one of:

- `Verified`: confirmed by source of truth;
- `Partially verified`: plausible but incomplete;
- `Unverified`: captured from discussion or assumption;
- `Stale`: likely outdated;
- `Superseded`: replaced by a newer entry.

Unverified or stale context MUST NOT be treated as final architecture or product truth without human confirmation or a newer source.

### 6.5 Sensitive knowledge

The repository MUST NOT commit secrets, credentials, private keys, access tokens, regulated personal data, customer-private data, or proprietary third-party material unless the organization explicitly permits it and the repository is authorized to store it.

When sensitive external knowledge is relevant, commit a safe summary and a pointer to the authorized access path. Never commit the sensitive value itself.

### 6.6 Failure as harness feedback

If an agent fails because it lacked context, guessed an interface, crossed a boundary, ran the wrong command, missed an invariant, or could not verify its work, the failure is a harness defect. The fix MUST include at least one of:

- update a context doc;
- add or update a command in `docs/commands.md`;
- add a policy;
- add a template;
- add a mechanical check;
- update `AGENTS.md` if the missing guidance is small and navigational;
- update `spec.md` if the root contract was incomplete.

Correcting only the current run is not enough when the same missing context could affect future runs.

## 7. Initial Repository Shape

Status: Concrete Now

A compliant empty-repository initialization MUST create this minimum tree:

```text
.
├── AGENTS.md
├── README.md
├── spec.md
├── docs/
│   ├── index.md
│   ├── status.md
│   ├── glossary.md
│   ├── architecture.md
│   ├── commands.md
│   ├── context/
│   │   ├── README.md
│   │   ├── business.md
│   │   ├── product.md
│   │   ├── operations.md
│   │   ├── support.md
│   │   └── external-sources.md
│   ├── decisions/
│   │   ├── README.md
│   │   └── template.md
│   ├── plans/
│   │   ├── README.md
│   │   ├── template.md
│   │   ├── active/
│   │   │   └── .gitkeep
│   │   └── completed/
│   │       └── .gitkeep
│   ├── policies/
│   │   ├── security.md
│   │   ├── configuration.md
│   │   ├── dependencies.md
│   │   ├── testing.md
│   │   ├── documentation.md
│   │   ├── knowledge-ingestion.md
│   │   └── generated-files.md
│   └── quality/
│       ├── checklist.md
│       ├── drift.md
│       ├── harness-health.md
│       └── failure-feedback.md
├── scripts/
│   ├── README.md
│   └── check-harness.sh
├── .editorconfig
├── .gitattributes
└── .gitignore
```

The initial repository is not empty after applying the harness. It is a documentation, policy, context, and verification scaffold whose job is to preserve future instructions.

## 8. Root Files

Status: Concrete Now

### 8.1 `README.md`

Requirement: provide a human-facing repository entrypoint.

Empty-repo output: a short README that explains the repository is currently a harness scaffold, points to `spec.md`, `AGENTS.md`, and `docs/index.md`, and explains how to run the harness health check.

Persistent artifact: `README.md`.

Update trigger: any change to the repository's purpose, setup flow, primary commands, or maturity stage.

### 8.2 `AGENTS.md`

Requirement: provide a short agent-facing map.

Empty-repo output: a concise file with:

- first files to read;
- command discovery path;
- planning rules;
- verification rules;
- forbidden actions;
- harness update expectations.

Persistent artifact: `AGENTS.md`.

Mechanical enforcement: harness health check SHOULD warn if `AGENTS.md` is missing or too large.

Agent failure mode addressed: agents ignoring durable docs or relying on unstored prompt context.

### 8.3 `spec.md`

Requirement: preserve the root normative harness contract.

Empty-repo output: this file.

Persistent artifact: `spec.md`.

Update trigger: changes to harness philosophy, REQUIRED structure, classifications, persistence model, knowledge model, acceptance checklist, or initial commit blueprint.

### 8.4 `.editorconfig`, `.gitattributes`, `.gitignore`

Requirement: establish minimal cross-platform repository hygiene without choosing a language.

Empty-repo output:

- `.editorconfig` for indentation, final newlines, and charset;
- `.gitattributes` for normalized text handling;
- `.gitignore` for OS/editor noise, environment files, secrets, logs, build output, caches, and local artifacts.

Persistent artifact: root configuration files.

Update trigger: adding a language, package manager, build system, generated artifacts, or local tool cache.

## 9. Documentation System

Status: Concrete Now

`docs/` is durable repository memory. It MUST be indexed and maintained.

### 9.1 `docs/index.md`

Requirement: catalog durable docs.

Empty-repo output: an index with all starter docs, when to read them, and their status.

Update trigger: adding, deleting, renaming, splitting, or deprecating docs.

### 9.2 `docs/status.md`

Requirement: summarize the current repository state.

Empty-repo output: initial status showing the repository is in harness scaffold stage.

Update trigger: major milestones, new language/framework adoption, new deployable services, major risk changes, completed plans, or maturity changes.

### 9.3 `docs/glossary.md`

Requirement: preserve domain and repository terms.

Empty-repo output: starter definitions for harness terms.

Update trigger: new domain vocabulary, product terms, architecture terms, acronyms, or ambiguous names.

### 9.4 `docs/architecture.md`

Requirement: provide a top-level architecture map even before application code exists.

Empty-repo output: state that no application architecture exists yet, then define the process for adding architecture docs when code is introduced.

Specialization rule: when source code is added, this file MUST document:

- top-level packages or services;
- dependency direction;
- ownership boundaries;
- external interfaces;
- generated-code locations;
- where local `AGENTS.md` files are allowed;
- what structural checks enforce boundaries.

## 10. Command Discovery and Validation

Status: Concrete Now for registry; Meta-Specified Now for language-specific commands

`docs/commands.md` is the command registry. Agents MUST consult it before running setup, validation, test, build, lint, format, release, migration, data, deployment, or harness maintenance commands.

Each command entry MUST include:

- name;
- purpose;
- command line;
- working directory;
- prerequisites;
- expected runtime;
- whether network is REQUIRED;
- whether secrets are REQUIRED;
- safe sandbox/approval expectation;
- when to run it;
- what success means;
- common failures and fixes.

The initial harness MUST define at least:

- `harness:check` → `sh scripts/check-harness.sh`

When a language or toolchain is chosen, the repository MUST add commands for:

- setup/bootstrap;
- format;
- lint;
- typecheck or static analysis when available;
- test;
- build/package;
- local run or smoke test;
- CI-equivalent validation.

A change is incomplete if it introduces a command humans or agents need but does not update `docs/commands.md`.

## 11. Planning and Execution Plans

Status: Concrete Now

Small changes MAY use ephemeral planning in the agent thread. Multi-step, risky, ambiguous, long-running, cross-cutting, or architecture-changing work MUST create a durable plan under `docs/plans/active/` using `docs/plans/template.md`.

A durable plan MUST include:

- goal;
- non-goals;
- relevant context links;
- assumptions;
- milestones;
- acceptance criteria;
- validation commands per milestone;
- decision log;
- current status;
- rollback or recovery notes;
- open questions.

Plans move from `docs/plans/active/` to `docs/plans/completed/` when finished or abandoned. Completed plans MUST record outcome, validation performed, unresolved follow-ups, and links to related decisions.

Agent failure mode addressed: long-running work drifting, losing status, or expanding scope without a durable audit trail.

## 12. Decisions

Status: Concrete Now

Architectural, product, security, dependency, deployment, data, or process decisions with future impact MUST be recorded under `docs/decisions/` using `docs/decisions/template.md`.

A decision record MUST include:

- title;
- status;
- date;
- context;
- decision;
- consequences;
- alternatives considered;
- verification or rollback plan;
- related docs and plans.

Decision records SHOULD be short. They are not essays; they are future-agent memory.

## 13. Policies

Status: Concrete Now for starter policies; Meta-Specified Now for implementation details

### 13.1 Security

Persistent artifact: `docs/policies/security.md`.

Requirements:

- secrets MUST NOT be committed;
- local environment files MUST be ignored unless they are safe examples;
- agents MUST NOT exfiltrate secrets or inspect local private material unless explicitly authorized;
- destructive commands MUST have explicit human approval;
- network access MUST be denied by default unless a documented command needs it;
- security assumptions MUST be written down and updated when capabilities change.

### 13.2 Configuration

Persistent artifact: `docs/policies/configuration.md`.

Requirements:

- application code MUST treat process environment as read-only input;
- configuration values SHOULD be parsed once at a boundary into typed or structured configuration objects when the chosen language supports it;
- `.env.example` or equivalent MUST be added when environment variables become REQUIRED;
- defaults, REQUIRED variables, and secret status MUST be documented;
- library code SHOULD NOT mutate global process environment at import/load time.

### 13.3 Dependencies

Persistent artifact: `docs/policies/dependencies.md`.

Requirements:

- production dependencies MUST have a stated reason;
- dependency managers and lockfiles MUST be documented once chosen;
- new dependencies SHOULD be evaluated for maintenance status, license, security risk, transitive footprint, and agent legibility;
- opaque dependencies SHOULD be wrapped behind clear repository-local interfaces.

### 13.4 Testing

Persistent artifact: `docs/policies/testing.md`.

Requirements:

- every behavior change SHOULD include or update tests unless a documented reason explains why not;
- every validation command MUST be registered in `docs/commands.md`;
- tests SHOULD be arranged by scope once code exists: unit, integration, contract, end-to-end, smoke, regression;
- fixtures MUST be documented when non-obvious;
- golden/snapshot tests MUST have an update procedure;
- external input boundaries SHOULD parse untrusted data into precise internal representations before business logic acts on it.

### 13.5 Documentation

Persistent artifact: `docs/policies/documentation.md`.

Requirements:

- docs MUST be discoverable from `docs/index.md`;
- docs MUST distinguish durable context from temporary notes;
- stale docs MUST be marked stale or updated;
- removed behavior MUST remove or update its docs;
- large docs SHOULD include a summary and “when to read this” section.

### 13.6 Knowledge ingestion

Persistent artifact: `docs/policies/knowledge-ingestion.md`.

Requirements:

- durable external context MUST become safe repository-local summaries;
- sensitive information MUST be redacted or referenced by authorized access path;
- source, date, owner, freshness, and verification status MUST be recorded;
- repeated missing-context failures MUST trigger a doc, command, policy, template, or check update.

### 13.7 Generated files

Persistent artifact: `docs/policies/generated-files.md`.

Requirements:

- generated files MUST be clearly marked;
- their generator command MUST be registered;
- generated outputs SHOULD NOT be edited manually unless the policy explicitly permits it;
- generated references intended for agents SHOULD be kept concise and regenerated when sources change.

## 14. Architecture and Boundary Rules

Status: Meta-Specified Now

Before application code exists, the harness cannot honestly define package boundaries. It MUST define the procedure for creating them.

When code is introduced, the first code-bearing change MUST update `docs/architecture.md` to define:

- top-level source directories;
- layer or package boundaries;
- dependency direction;
- public interfaces;
- allowed cross-boundary calls;
- forbidden imports or dependencies;
- test locations;
- generated-code locations;
- local agent instruction files if needed.

Boundaries SHOULD be mechanically enforced where possible by language tooling, static analysis, tests, or repository scripts.

The repository SHOULD prefer explicit contracts at boundaries. External inputs SHOULD be parsed into precise internal forms before execution logic depends on them. Invalid states SHOULD be made unrepresentable where the language and domain make that practical.

## 15. Sandbox, Permissions, and Forbidden Actions

Status: Persisted Guidance Now

The harness assumes agents MAY run in sandboxes with approval controls, but it does not mandate a specific agent runtime.

The specific agent runtime, sandbox, approval, and operator-confirmation posture are
`Implementation-defined`; repository guidance MUST document the selected behavior before agents
rely on it.

Repository guidance MUST make these boundaries explicit:

- agents MAY read repository files needed for the task;
- agents MAY edit files inside the repository workspace unless a policy says otherwise;
- agents MUST NOT edit files outside the repository without explicit human approval;
- agents MUST NOT read or print secrets;
- agents MUST NOT run destructive commands unless explicitly authorized;
- agents MUST NOT enable broad network access merely for convenience;
- agents MUST NOT install dependencies or change toolchains without updating dependency and command docs;
- agents MUST NOT commit generated artifacts unless the generated-files policy permits it;
- agents MUST NOT leave essential instructions only in a chat response.

When a command needs network, credentials, filesystem access outside the repo, or destructive power, `docs/commands.md` MUST say so.

## 16. Mechanical Checks

Status: Concrete Now for starter check; Meta-Specified Now for future checks

The initial harness MUST include `scripts/check-harness.sh`. It SHOULD verify at least:

- REQUIRED files exist;
- REQUIRED directories exist;
- `AGENTS.md` is present and not excessively large;
- `docs/index.md` and `docs/commands.md` are present;
- `spec.md` is present;
- starter templates exist.

As the repository matures, mechanical checks SHOULD be added for:

- docs index coverage;
- broken links;
- stale context metadata;
- undocumented commands;
- missing test/build commands;
- architecture boundary violations;
- generated files out of date;
- forbidden secrets;
- dependency policy violations;
- unowned docs;
- missing decision records for major changes.

Mechanical checks SHOULD emit remediation instructions that tell future agents exactly which file to update.

## 17. Harness Health and Drift Cleanup

Status: Concrete Now

The repository MUST treat drift as normal and cleanup as recurring work.

`docs/quality/harness-health.md` tracks the current harness maturity, missing mechanical checks, stale docs, and next improvements.

`docs/quality/drift.md` defines the cleanup process. Drift includes:

- docs disagreeing with commands;
- commands missing from registry;
- repeated agent mistakes;
- unindexed docs;
- stale status;
- unchecked generated files;
- inconsistent architecture patterns;
- obsolete plans;
- policies with no enforcement path.

When drift is found, agents SHOULD fix the smallest durable source of the drift, not just the symptom.

## 18. Initial Commit Blueprint

Status: Concrete Now

The first commit MUST create the following files. “Routine read” means agents SHOULD read it often at task start. “Relevant read” means agents SHOULD read it only when the task touches that area.

| Path | Role | Kind | Update when | Agent read | Stores |
|---|---|---|---|---|---|
| `spec.md` | Root harness contract | Normative | harness rules, coverage model, persistence model, knowledge model, REQUIRED tree, acceptance criteria change | Relevant read; routine for harness work | normative harness rules |
| `AGENTS.md` | Short agent map | Navigational | important entrypoints, forbidden actions, command discovery, done criteria, or plan rules change | Routine read | navigational guidance |
| `README.md` | Human entrypoint | Navigational | repository purpose, setup, status, or primary commands change | Routine read for humans; relevant for agents | durable overview |
| `docs/index.md` | Documentation map | Navigational | docs are added, removed, renamed, split, deprecated, or status changes | Routine read | durable doc index |
| `docs/status.md` | Current repository state | Context | maturity, active plans, risks, languages, services, or deployment state changes | Routine read | durable project status |
| `docs/glossary.md` | Shared vocabulary | Context | terms, acronyms, product concepts, or architecture names appear | Relevant read | durable vocabulary |
| `docs/architecture.md` | Architecture map and boundary procedure | Context / policy | source code, services, packages, APIs, dependency boundaries, or generated code are added | Relevant read | durable architecture context |
| `docs/commands.md` | Command registry | Normative / checklist | any setup, validation, build, test, lint, run, release, migration, data, or harness command changes | Routine read | command truth |
| `docs/context/README.md` | Context index | Navigational | context files change | Relevant read | context map |
| `docs/context/business.md` | Business assumptions | Context | business model, pricing, customer, contract, compliance, or commercial assumptions change | Relevant read | durable business context |
| `docs/context/product.md` | Product requirements | Context | product requirements, personas, UX rules, non-goals, or acceptance criteria change | Relevant read | durable product context |
| `docs/context/operations.md` | Operational context | Context | deploy, reliability, incident, observability, or environment assumptions change | Relevant read | durable ops context |
| `docs/context/support.md` | Support learnings | Context | recurring user issues, support workflows, FAQs, or workaround knowledge changes | Relevant read | durable support context |
| `docs/context/external-sources.md` | External source register | Context / index | external docs, chats, meetings, designs, APIs, or sources influence repo decisions | Relevant read | source provenance |
| `docs/decisions/README.md` | Decision process | Policy | decision record rules change | Relevant read | normative process |
| `docs/decisions/template.md` | Decision template | Template | decision format changes | Relevant read | reusable template |
| `docs/plans/README.md` | Plan process | Policy | planning workflow changes | Relevant read | normative process |
| `docs/plans/template.md` | Plan template | Template | plan format changes | Relevant read | reusable template |
| `docs/plans/active/.gitkeep` | Preserve active plan directory | Placeholder | never unless directory has real files | Not read | none |
| `docs/plans/completed/.gitkeep` | Preserve completed plan directory | Placeholder | never unless directory has real files | Not read | none |
| `docs/policies/security.md` | Security policy | Policy | secrets, permissions, network, destructive commands, or threat model changes | Relevant read | normative security rules |
| `docs/policies/configuration.md` | Config/env policy | Policy | env vars, config loader, secret configuration, runtime config changes | Relevant read | normative config rules |
| `docs/policies/dependencies.md` | Dependency policy | Policy | package managers, dependencies, lockfiles, licenses, or supply chain assumptions change | Relevant read | normative dependency rules |
| `docs/policies/testing.md` | Testing policy | Policy | test strategy, validation gates, fixtures, snapshots, or parse-boundary rules change | Relevant read | normative quality rules |
| `docs/policies/documentation.md` | Documentation lifecycle | Policy | docs structure or maintenance process changes | Relevant read | normative docs rules |
| `docs/policies/knowledge-ingestion.md` | External knowledge promotion | Policy | external source handling, context metadata, or failure feedback rules change | Relevant read | normative knowledge rules |
| `docs/policies/generated-files.md` | Generated file rules | Policy | codegen, generated references, build artifacts, or regeneration commands change | Relevant read | normative generated-file rules |
| `docs/quality/checklist.md` | Change completion checklist | Checklist | review criteria or done definition changes | Routine before completion | quality checklist |
| `docs/quality/drift.md` | Drift cleanup process | Policy / checklist | cleanup process or drift categories change | Relevant read | durable cleanup rules |
| `docs/quality/harness-health.md` | Harness maturity and gaps | Context / checklist | checks, maturity, gaps, or harness risks change | Relevant read | durable harness status |
| `docs/quality/failure-feedback.md` | Agent failure loop | Policy / log | repeated agent failure patterns or remediations occur | Relevant read after failures | durable feedback memory |
| `scripts/README.md` | Script directory policy | Policy | scripts are added or script conventions change | Relevant read | script rules |
| `scripts/check-harness.sh` | Starter harness health check | Mechanical check | REQUIRED files or harness checks change | Executed, not usually read | executable verification |
| `.editorconfig` | Editor defaults | Policy/config | file formats or language-specific conventions change | Not routine | formatting defaults |
| `.gitattributes` | Git text normalization | Policy/config | binary/generated/text treatment changes | Not routine | repository hygiene |
| `.gitignore` | Ignore policy | Policy/config | toolchains, artifacts, caches, env files, or generated outputs change | Not routine | safety and hygiene |

## 19. Specialization Hooks

Status: Meta-Specified Now

The initial harness MUST stay language-agnostic. When the repository specializes, the first change introducing that specialization MUST add the relevant module:

Specialization choices are `Implementation-defined`. The selected behavior MUST be documented in
the durable artifact named by the relevant hook before downstream work depends on it.

### 19.1 Language or framework

Add:

- setup instructions;
- package manager and lockfile policy;
- format/lint/typecheck/test/build commands;
- source tree layout;
- test layout;
- dependency policy additions;
- local `AGENTS.md` files only if subdirectories need distinct rules.

### 19.2 Deployable service

Add:

- local run command;
- smoke test;
- configuration variables;
- secrets handling;
- operational context;
- logging/metrics/tracing expectations;
- runbook location;
- deployment safety policy.

### 19.3 Public API or schema

Add:

- source-of-truth schema location;
- generated reference location;
- compatibility policy;
- contract tests;
- example payloads;
- parse-boundary rules.

### 19.4 Database or migration system

Add:

- migration command;
- rollback policy;
- local development database setup;
- fixture/seed policy;
- data safety rules;
- schema reference generation.

### 19.5 Generated code or references

Add:

- generator command;
- source inputs;
- output location;
- review policy;
- regeneration check;
- manual-editing rule.

### 19.6 Release artifacts

Add:

- versioning policy;
- changelog policy;
- release validation commands;
- artifact storage policy;
- rollback process.

### 19.7 Monorepo scale

When the repository grows beyond one package/service, add:

- workspace/package map;
- ownership model;
- boundary checks;
- per-package commands or command selectors;
- local `AGENTS.md` files for package-specific rules;
- cross-package dependency policy;
- shared utility policy;
- generated reference index.

## 20. Done Criteria for Future Changes

Status: Concrete Now

A future change is complete only when:

1. The requested behavior or documentation change is implemented.
2. Relevant validation commands from `docs/commands.md` have run, or the reason they could not run is recorded.
3. Tests or checks were added/updated when behavior changed.
4. Durable docs were updated when commands, architecture, product assumptions, business rules, operations, dependencies, configuration, security, generated files, or workflows changed.
5. New external knowledge used by the change was either promoted into repo-local context or explicitly classified as temporary/sensitive/irrelevant.
6. Any major decision was recorded under `docs/decisions/`.
7. Any multi-step plan was updated and moved if complete.
8. No secrets or local-only artifacts were committed.
9. The harness health check passes or its failure is explained with a follow-up plan.

## 21. Acceptance Checklist for the Initial Harness

Status: Concrete Now

The initial harness is acceptable when:

- `spec.md` exists at the root and is the normative contract.
- `AGENTS.md` is short, navigational, and points to durable docs.
- `README.md` explains the scaffold and points to the right starting files.
- `docs/index.md` catalogs the docs.
- `docs/commands.md` registers `harness:check`.
- `docs/context/` contains safe placeholders for durable business, product, operations, support, and external-source context.
- `docs/decisions/` contains a process and template.
- `docs/plans/` contains a process, template, and active/completed directories.
- `docs/policies/` contains starter policies for security, configuration, dependencies, testing, documentation, knowledge ingestion, and generated files.
- `docs/quality/` contains checklist, drift, harness-health, and failure-feedback docs.
- `scripts/check-harness.sh` exists and passes.
- `.editorconfig`, `.gitattributes`, and `.gitignore` exist.
- No essential rule exists only outside the repository.

## 22. Agent Initialization Procedure

Status: Concrete Now

When an agent initializes an empty repository from this specification, it MUST:

1. Create the tree in Section 7.
2. Populate every REQUIRED file with useful starter content, not empty placeholders, except `.gitkeep` files.
3. Ensure `AGENTS.md` remains short.
4. Register `harness:check` in `docs/commands.md`.
5. Make `scripts/check-harness.sh` executable when the filesystem supports it.
6. Run `sh scripts/check-harness.sh`.
7. Report created files, validation status, and any limitations.
