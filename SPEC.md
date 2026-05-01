# Repository Harness Specification

Status: Draft v2
Audience: humans and coding agents initializing or maintaining a repository
Scope: language-agnostic repository harness for an empty Git repository

## 1. Purpose

This file explains how to turn a brand-new empty Git repository into a self-sustaining engineering harness.

A repository harness is the repo-local support system that makes future work safe for humans and coding agents: clear starting points, indexed context, known commands, policies, templates, quality checks, review guidance, and rules for keeping all of that up to date.

When a human or coding agent applies this specification to an empty repository, the result should be a repository that already contains:

- a concise root harness charter, `HARNESS.md`;
- a short agent map, `AGENTS.md`;
- indexed durable docs under `docs/`;
- a machine-readable manifest, `harness.yaml`;
- a command registry;
- security, configuration, testing, documentation, dependency, generated-file, runtime, and audit policies;
- planning and decision templates;
- review guidance;
- quality scorecards and audit prompts;
- starter scripts that check whether the harness still matches its own rules.

This file is a setup blueprint. It is not meant to be the large instruction file that future agents read during routine work. After the repository is initialized, the repository must carry its own operating guidance in smaller, easier-to-discover files. Future agents should be able to understand and improve the harness by reading `AGENTS.md`, `HARNESS.md`, `harness.yaml`, `docs/index.md`, and the relevant linked docs, without reading this full specification.

A harness exists because coding agents can only reliably use context that is discoverable from the repository or from explicitly available tools. Relevant information that lives only in chat history, private memory, meeting notes, external docs, ad hoc prompts, or unstated human expectations effectively does not exist to the next agent working in the repository.

Most repositories have always had pieces of a harness: README files, tests, CI, style guides, architecture notes, scripts, templates, and review norms. Historically, those pieces were often treated as helpful project hygiene that could be added after the repository started to grow. Human developers carried much of the missing context in memory, noticed when something felt wrong, and corrected drift during design discussions, code review, or production support.

Agentic workflows change that tradeoff. A coding agent does not automatically share the team’s memory, taste, accountability, or sense of which conventions are load-bearing. It needs useful guidance before it acts and reliable feedback after it acts. As agents take on more of the software lifecycle, repository hygiene becomes operating infrastructure: the repo itself must become the onboarding packet, rulebook, command surface, feedback loop, and durable memory.

The harness turns that constraint into an engineering practice. It converts an empty Git repository into a self-propagating working environment: root instructions, indexed durable docs, discoverable commands, policies, templates, checks, quality rubrics, and knowledge-ingestion paths that preserve what future agents need to know.

This work is REQUIRED, not polish. In agentic development, repository hygiene, command consistency, guardrails, tests, architecture notes, durable context, review doctrine, runtime posture, quality audits, and feedback loops are the infrastructure that lets agents move quickly without constant human correction. The harness is the layer that makes later application work safe to delegate. Future agents must understand this history so they do not mistake the harness for bureaucracy or optional documentation.

A coding agent SHOULD be able to receive this file in an empty Git repository and create the initial harness commit without needing any prior chat history, external prompt, meeting notes, or unstored context. A future human or agent SHOULD then be able to clone the resulting repository, read only the committed files, and understand how to maintain and evolve the harness.

This file therefore has three roles:

1. It is a reusable setup contract for creating the first commit of a new repository.
2. It defines which harness files must exist in the initialized repository and what each file must preserve.
3. It explains the background rationale that must be carried forward into the initialized repository, especially in `HARNESS.md` and `docs/harness/source-rationale.md`.

The initialized repository MUST NOT require future agents to read this full setup blueprint during routine work. The agent applying this specification MUST move the important rules and rationale into the durable harness files listed here.

The harness is not application code, a framework choice, a project-management system, or an agent orchestrator. It is the repository-local contract around future application code: the substrate that makes all later work discoverable, scoped, reviewable, testable, and recoverable.

In this document, “the agent applying this specification” means the human or coding agent creating or updating the repository files according to this document.

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

- **A root harness charter**: `HARNESS.md` defines the root harness contract, explains why the harness exists, and tells future agents how to evolve it.
- **A source-backed rationale layer**: `HARNESS.md` and `docs/harness/source-rationale.md` preserve enough of the setup blueprint's harness theory and source-backed justification for future agents to improve the harness without reading this full setup blueprint.
- **An agent map**: `AGENTS.md` stays short and points agents to the right durable docs, commands, plans, and policies.
- **Durable memory**: `docs/` stores project context, decisions, command definitions, policies, quality rules, plans, and known risks.
- **A command surface**: `docs/commands.md` tells agents how to set up, validate, test, build, inspect, repair, and maintain the repository.
- **A checkable manifest**: `harness.yaml` records the required files, metadata fields, command schema, indexed docs, and enabled checks so the repository can validate its own harness instead of relying on prose alone.
- **A review doctrine**: `code_review.md` records how humans and agents review diffs, verify behavior, handle risky changes, and decide whether work is complete.
- **An agent-runtime posture**: `docs/policies/agent-runtime.md` records the expected sandbox, approval, network, writable-root, and external-tool posture before agents rely on those capabilities.
- **Generated and reference context**: `docs/generated/` and `docs/references/` preserve concise machine-readable or generated context that agents can inspect without depending on external systems.
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

Rationale: agents need a map, not a giant manual. Documentation alone does not keep a large repository coherent; durable instructions must be indexed, scoped, fresh, and mechanically checked where possible. Human taste and recurring feedback should be captured once, then enforced or surfaced continuously.

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
| Root harness charter | Concrete Now | `HARNESS.md` |
| Setup blueprint (`SPEC.md`) | External setup blueprint; optional archived reference | not required in target repos; optional `docs/harness/initializer-spec.md` |
| Source-backed harness rationale | Concrete Now | `docs/harness/source-rationale.md`, `HARNESS.md` |
| Short agent map | Concrete Now | `AGENTS.md` |
| Human entrypoint | Concrete Now | `README.md` |
| Documentation index | Concrete Now | `docs/index.md` |
| Status summary | Concrete Now | `docs/status.md` |
| Glossary | Concrete Now | `docs/glossary.md` |
| Initial architecture map | Concrete Now | `docs/architecture.md` |
| Command registry | Concrete Now | `docs/commands.md` |
| Harness manifest | Concrete Now | `harness.yaml` |
| Review doctrine | Concrete Now | `code_review.md` |
| Agent runtime posture | Concrete Now | `docs/policies/agent-runtime.md` |
| Documentation metadata schema | Concrete Now | `harness.yaml`, `docs/policies/documentation.md` |
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
| Generated context directory | Concrete Now | `docs/generated/README.md` |
| External reference directory | Concrete Now | `docs/references/README.md` |
| Quality checklist | Concrete Now | `docs/quality/checklist.md` |
| Drift cleanup | Concrete Now | `docs/quality/drift.md` |
| Harness health check | Concrete Now | `docs/quality/harness-health.md`, `scripts/check-harness.sh` |
| Quality scorecard | Concrete Now | `docs/quality/scorecard.md` |
| Golden principles | Concrete Now | `docs/quality/golden-principles.md` |
| Deterministic harness checks | Concrete Now | `scripts/check-harness.sh`, `harness.yaml` |
| Agentic audit specification | Concrete Now as specification; Conditional Module for executable CI integration | `docs/policies/agentic-audits.md`, `docs/quality/audits/*` |
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
| Executable agentic audit runner | Conditional Module | future `scripts/run-agentic-audit`, CI workflow, secret-management docs |
| External task assignment, scheduling, retries | Control-Plane Boundary | not stored as harness implementation |
| Agent runtime protocol | Control-Plane Boundary | not stored as harness implementation |
| Hosted dashboard | Out of Scope | none |

## 5. Persistence / Self-Propagation Model

Status: Concrete Now

### 5.1 Setup blueprint versus repository-owned harness

This file is the setup blueprint. The initialized repository is the product.

The initialized repository MUST be self-sustaining without requiring future agents to read this full file. It MUST preserve every future-critical rule in durable repository artifacts, but it SHOULD do so through progressive disclosure: a short map first, then linked docs, policies, templates, checks, prompts, and manifests.

The initialized repository MUST include:

- `HARNESS.md` as the root harness charter that explains what the harness is, why it exists, and how to evolve it;
- `AGENTS.md` as the short agent map;
- `harness.yaml` as the machine-readable harness manifest;
- `docs/index.md` as the documentation map;
- `docs/harness/` as deeper harness-maintenance context;
- `docs/context/` as durable product, business, operations, support, and external-source memory;
- `docs/policies/` as delegated policy;
- `docs/quality/` as scorecards, golden principles, drift cleanup, audit definitions, and failure feedback;
- `docs/commands.md` as the command surface;
- `scripts/check-harness.sh` as the starter deterministic enforcement path.

The full setup blueprint MAY be retained only as archived reference, preferably at `docs/harness/initializer-spec.md`, when the initializing human explicitly asks for it. If retained, it MUST be marked `Status: Archived setup reference` and `Routine agent read: No`. It MUST NOT be the only place any operational rule lives.

The initialized harness is incomplete if a future agent cannot understand why the harness exists and how to improve it without reading this setup blueprint.

### 5.2 Source of truth after initialization

After initialization, `HARNESS.md` is the root harness charter. It is normative for harness maintenance, but it MUST remain a charter, not a giant manual.

`HARNESS.md` MUST explain:

- what a repository harness is;
- what it is not;
- why agent-discoverable context is the core constraint;
- why root agent guidance must be a map rather than a large manual;
- why durable docs, command registries, templates, checks, scorecards, review doctrine, and policies exist;
- why external or tacit knowledge must be promoted into repository-local artifacts;
- why repeated agent failures are harness defects when caused by missing context, missing tools, unclear boundaries, missing checks, or unverifiable assumptions;
- how future changes improve the harness rather than bypass it.

`HARNESS.md` SHOULD be substantial enough to teach the operating model, not a terse placeholder. It SHOULD preserve the important rationale from this setup blueprint in repository-owned form. It SHOULD link to deeper docs instead of restating every procedure.

The repository MUST NOT rely on chat history, unstored prompts, private notebooks, external docs, meeting notes, or human memory for any rule REQUIRED to maintain the harness.

When a future-critical rule is discovered, it MUST be promoted into one of:

- `HARNESS.md` for root harness philosophy, operating model, and self-propagation rules;
- `AGENTS.md` for short navigational agent guidance;
- `harness.yaml` for machine-readable harness structure;
- `docs/` for durable project knowledge;
- templates for repeated work;
- checklists for reviewable processes;
- command registries for validation routines;
- scripts, tests, linters, CI jobs, or structural checks for mechanical enforcement;
- agentic audit prompts and rubrics when semantic review is needed and cannot yet be reduced to deterministic checks.

### 5.3 Using this file to initialize a repository

This file MAY be given to a coding agent inside an empty Git repository with the instruction: “Initialize this repository according to `SPEC.md`.”

The agent MUST then create the initial commit shape listed in Section 18. The agent MUST create the durable harness files directly. It MUST NOT simply copy this full file into the repository root as the day-to-day harness contract.

If the initializing human asks to keep this file for traceability, the agent MAY archive it under `docs/harness/initializer-spec.md`. Archived retention does not remove the requirement to move every essential rule into `HARNESS.md`, `AGENTS.md`, `harness.yaml`, `docs/`, templates, checklists, commands, scripts, or audit definitions.

### 5.4 What belongs in each artifact

`HARNESS.md` contains:

- the repository-owned harness definition;
- the source-backed rationale for why the harness exists;
- the operating premise that agents can only reliably use discoverable context;
- the root self-propagation rule;
- the relationship between `AGENTS.md`, `harness.yaml`, `docs/`, policies, checks, scorecards, and audits;
- the rule that repeated missing-context or missing-tool failures must improve the harness.

`docs/harness/source-rationale.md` contains:

- source-backed rationale and links that justify the harness design;
- short quotations or close paraphrases from official sources when useful;
- a short history of why harness work became more important as agentic workflows became common;
- the reason each preserved quote matters for future harness maintenance;
- boundaries showing which parts are source-derived and which are repository-local choices.

`harness.yaml` contains:

- the required initial file set;
- required durable-doc metadata fields;
- command-entry schema requirements;
- required indexed docs;
- allowed root instruction files;
- enabled deterministic harness checks;
- agentic audit prompt locations and modes;
- known generated/reference directories;
- placeholder slots for future specialized checks.

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
- harness maintenance context;
- decisions;
- plans;
- policies;
- quality, audits, scorecards, drift records, and failure feedback.

`code_review.md` contains stable review instructions: what evidence belongs in a handoff, how agents self-review, how risky diffs are escalated, and how review feedback becomes durable context or checks.

Templates contain repeated formats. Checklists contain repeatable review procedures. Scripts and future CI checks contain deterministic enforcement. Agentic audit prompts contain semantic review procedures.

### 5.5 Source-backed rationale preservation

The agent applying this specification MUST preserve enough source-backed rationale inside the initialized repository for future agents to understand and improve the harness itself. This context MUST NOT be treated as disposable background used only during setup.

At minimum, `HARNESS.md` and `docs/harness/source-rationale.md` MUST preserve these principles, with links to their public source pages.

Source pages to cite in the repository-owned rationale include:

- `https://openai.com/index/harness-engineering/`
- `https://developers.openai.com/codex/learn/best-practices`
- `https://developers.openai.com/codex/concepts/sandboxing`
- `https://developers.openai.com/blog/run-long-horizon-tasks-with-codex`
- `https://developers.openai.com/codex/guides/agents-md`
- `https://martinfowler.com/articles/harness-engineering.html`
- `https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html`

Required preserved principles:

- “Humans steer. Agents execute.” Preserve this as the role separation: humans define priorities, constraints, and acceptance criteria; agents execute inside the harness.
- “Early progress was slower than we expected, not because Codex was incapable, but because the environment was underspecified.” Preserve this as the historical reason a harness is not optional setup polish: agent capability is gated by the environment around the agent.
- “What capability is missing, and how do we make it both legible and enforceable for the agent?” Preserve this as the default response when an agent struggles.
- “Building software still demands discipline, but the discipline shows up more in the scaffolding rather than the code.” Preserve this as the reason repository hygiene, checks, docs, and feedback loops must be treated as core engineering work.
- “Give Codex a map, not a 1,000-page instruction manual.” Preserve this as the reason `AGENTS.md` stays short and maps to indexed docs.
- “Context is a scarce resource.” Preserve this as the reason routine context surfaces must be small.
- “So instead of treating `AGENTS.md` as the encyclopedia, we treat it as the table of contents.” Preserve this as the reason durable knowledge lives in indexed docs.
- “From the agent’s point of view, anything it can’t access in-context while running effectively doesn’t exist.” Preserve this as the reason external and tacit knowledge must become repository-local context.
- “Documentation alone doesn’t keep a fully agent-generated codebase coherent.” Preserve this as the reason policies need checks, linters, structural tests, scorecards, and audits.
- “When documentation falls short, we promote the rule into code.” Preserve this as the rule that repeated audit findings should become deterministic checks when possible.
- “Human taste is captured once, then enforced continuously.” Preserve this as the reason review feedback, golden principles, scorecards, and recurring audits exist.
- “A well-built outer harness serves two goals: it increases the probability that the agent gets it right in the first place, and it provides a feedback loop that self-corrects as many issues as possible before they even reach human eyes.” Preserve this as the guide/sensor model for harness design.
- “Guides” and “sensors” are useful terms for future harness work. Preserve the distinction: guides steer the agent before it acts; sensors observe results after it acts and feed correction back into the work loop.
- “Computational” and “inferential” controls are useful terms for future harness work. Preserve the distinction: deterministic checks such as tests, linters, type checkers, and structural analysis should catch what they can reliably catch; model-assisted reviews and audits should handle semantic judgment where deterministic checks are not enough.

The initialized repository MAY quote these phrases directly with attribution and links. It SHOULD avoid long copied passages. It SHOULD explain why each phrase matters operationally for this repository.

### 5.6 Update triggers

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
- agent runtime, sandbox, approval, network, or external-tool posture;
- agentic audit prompts, rubrics, modes, or provider configuration;
- data/artifact storage;
- recurring failure patterns.

A change is incomplete if it changes behavior but leaves the durable docs, manifest, commands, templates, checks, prompts, or scorecards stale.

### 5.7 Decomposition without losing normative force

The initialized harness MUST prefer progressive disclosure over giant instruction files. Decomposition is valid only if:

1. `HARNESS.md` still states the root harness contract;
2. every delegated document is listed in `docs/index.md`;
3. each delegated document states its status, owner, and scope;
4. `harness.yaml` records required files, metadata fields, command schemas, audit prompts, and enabled checks;
5. `AGENTS.md` points agents to the root contract and index, not to an uncatalogued pile of files;
6. mechanical checks verify required delegated files exist and are indexed.

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

Durable context MUST be summarized into the appropriate repository file. The target file MUST have the standard durable-doc metadata header. Each captured entry MUST include:

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

### 6.5 Durable-document metadata

Every durable markdown file under `docs/`, plus root `code_review.md`, MUST begin with a small metadata header. The minimum required fields are:

- `Status`: `Active`, `Draft`, `Placeholder`, `Stale`, `Superseded`, or `Archived`;
- `Owner`: a person, team, or `Unassigned`;
- `Last reviewed`: ISO date or `Never`;
- `Freshness`: review trigger or cadence;
- `Verification`: `Verified`, `Partially verified`, `Unverified`, `Scaffold`, `Stale`, or `Superseded`;
- `When to read`: one sentence explaining when an agent should open the file.

`harness.yaml` MUST declare these required fields, and `harness:check` MUST fail when a durable doc omits them. The goal is not bureaucracy; it is to let agents distinguish current source-of-truth context from placeholders and stale attractive nuisances.

### 6.6 Sensitive knowledge

The repository MUST NOT commit secrets, credentials, private keys, access tokens, regulated personal data, customer-private data, or proprietary third-party material unless the organization explicitly permits it and the repository is authorized to store it.

When sensitive external knowledge is relevant, commit a safe summary and a pointer to the authorized access path. Never commit the sensitive value itself.

### 6.7 Failure as harness feedback

If an agent fails because it lacked context, guessed an interface, crossed a boundary, ran the wrong command, missed an invariant, or could not verify its work, the failure is a harness defect. The fix MUST include at least one of:

- update a context doc;
- add or update a command in `docs/commands.md`;
- add a policy;
- add a template;
- add a mechanical check;
- update `AGENTS.md` if the missing guidance is small and navigational;
- update `HARNESS.md` if the root harness charter was incomplete, or update this setup blueprint only when improving the template itself.

Correcting only the current run is not enough when the same missing context could affect future runs.

## 7. Initial Repository Shape

Status: Concrete Now

A compliant empty-repository initialization MUST create this minimum tree:

```text
.
├── AGENTS.md
├── HARNESS.md
├── README.md
├── code_review.md
├── harness.yaml
├── docs/
│   ├── index.md
│   ├── status.md
│   ├── glossary.md
│   ├── architecture.md
│   ├── commands.md
│   ├── harness/
│   │   ├── README.md
│   │   ├── source-rationale.md
│   │   ├── coverage-model.md
│   │   ├── self-propagation.md
│   │   └── specialization.md
│   ├── generated/
│   │   └── README.md
│   ├── references/
│   │   └── README.md
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
│   │   ├── agent-runtime.md
│   │   ├── agentic-audits.md
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
│       ├── scorecard.md
│       ├── golden-principles.md
│       ├── failure-feedback.md
│       └── audits/
│           ├── README.md
│           ├── doc-quality.prompt.md
│           ├── architecture-drift.prompt.md
│           ├── golden-principles.prompt.md
│           ├── scorecard-update.prompt.md
│           └── output-schema.md
├── scripts/
│   ├── README.md
│   └── check-harness.sh
├── .editorconfig
├── .gitattributes
└── .gitignore
```

The initial repository is not empty after applying the harness. It is a documentation, policy, context, quality, audit, and verification scaffold whose job is to preserve future instructions.

The setup `SPEC.md` MAY exist outside the repository as the template used to create this tree. It MUST NOT be required in the initialized target repository unless the initializing human explicitly asks to archive it under `docs/harness/initializer-spec.md`.

## 8. Root Files

Status: Concrete Now

### 8.1 `README.md`

Requirement: provide a human-facing repository entrypoint.

Empty-repo output: a short README that explains the repository is currently a harness scaffold, points to `HARNESS.md`, `AGENTS.md`, `docs/index.md`, and `docs/commands.md`, and explains how to run the harness health check.

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
- harness update expectations;
- a pointer to `HARNESS.md` for the harness operating model;
- a pointer to `code_review.md` for review expectations.

Persistent artifact: `AGENTS.md`.

Mechanical enforcement: harness health check MUST warn or fail if `AGENTS.md` is missing, too large, or missing required pointers.

Agent failure mode addressed: agents ignoring durable docs or relying on unstored prompt context.

### 8.3 `HARNESS.md`

Requirement: preserve the root harness charter.

Empty-repo output: a substantive root document that teaches future agents what the harness is, why it exists, how it is organized, and how to improve it. It MUST preserve the source-backed rationale required by Section 5.5 and link to `docs/harness/source-rationale.md` for deeper evidence. It MUST briefly explain the history: ordinary repositories often had README files, tests, CI, scripts, style rules, and review norms, but agentic workflows make those pieces a first-class operating substrate because agents cannot rely on unstored human memory or tacit team judgment.

Persistent artifact: `HARNESS.md`.

Update trigger: changes to harness philosophy, operating model, source-backed rationale, self-propagation rules, or the relationship among `AGENTS.md`, `harness.yaml`, `docs/`, checks, scorecards, and audits.

Routine read: relevant for harness maintenance and useful for onboarding; not required for every small code change.

### 8.4 `code_review.md`

Requirement: preserve stable review doctrine.

Empty-repo output: a review guide covering self-review, risky diffs, validation evidence, generated files, dependency changes, docs updates, and how review feedback becomes durable context or checks.

Persistent artifact: `code_review.md`.

Update trigger: review expectations, risk categories, handoff evidence, or agent review workflow changes.

### 8.5 `harness.yaml`

Requirement: provide a machine-readable manifest for deterministic harness checks.

Empty-repo output: a manifest declaring required files, doc metadata fields, command-entry fields, index coverage, enabled checks, generated/reference directories, and agentic audit prompt locations.

Persistent artifact: `harness.yaml`.

Update trigger: required files, metadata schema, command schema, enabled checks, audit prompt locations, generated/reference directories, or specialization hooks change.

Important: `harness.yaml` is not a replacement for human-readable docs. It exists to let checks verify that the docs and commands still match the harness contract.

### 8.6 Optional archived setup blueprint

The full setup `SPEC.md` SHOULD NOT be committed to every target repository as root operational context. If retained for traceability, it MUST live under `docs/harness/initializer-spec.md`, be marked archived, and be excluded from routine agent reading. No essential rule may live only there.

### 8.7 `.editorconfig`, `.gitattributes`, `.gitignore`

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

### 9.5 `docs/harness/`

Requirement: preserve deeper harness-maintenance guidance without forcing agents to read the setup blueprint.

Empty-repo output:

- `docs/harness/README.md`: map for harness-maintenance docs;
- `docs/harness/source-rationale.md`: source-backed rationale, key quotations, links, and operational implications;
- `docs/harness/coverage-model.md`: the coverage classifications used by the harness;
- `docs/harness/self-propagation.md`: how future changes update durable artifacts;
- `docs/harness/specialization.md`: how language, product, deployment, API, data, release, and monorepo modules are added later.

Persistent artifact: `docs/harness/`.

Update trigger: harness maintenance rules, source-backed rationale, coverage classifications, or specialization procedures change.

### 9.5 Durable-doc metadata headers

Requirement: every durable documentation file MUST advertise status, owner, freshness, verification, and when to read it.

Empty-repo output: all starter docs under `docs/`, plus root `code_review.md`, include the standard metadata header from Section 6.5.

Persistent artifact: `harness.yaml`, `docs/policies/documentation.md`, and each durable doc.

Mechanical enforcement: `harness:check` MUST fail when required durable docs omit required metadata fields.

Agent failure mode addressed: agents treating stale placeholders as current source-of-truth guidance.

### 9.6 `docs/generated/` and `docs/references/`

Requirement: reserve explicit locations for machine-readable, generated, or external-reference context intended for agent use.

Empty-repo output:

- `docs/generated/README.md` explains where generated repository references belong, such as schema summaries, API maps, dependency graphs, route indexes, model cards, or architectural inventories.
- `docs/references/README.md` explains where concise external tool/library references belong, such as curated `llms.txt`-style docs, design-system references, CLI references, or vendor API notes.

Specialization rule: once generated or reference docs exist, their source, generation command, freshness expectation, and manual-editing policy MUST be documented. Generated references MUST be registered in `docs/commands.md` if they are regenerated by command.

Mechanical enforcement: mature repositories SHOULD check that generated references are up to date and that manually edited generated files are either forbidden or explicitly allowed.

Agent failure mode addressed: agents depending on invisible external APIs, stale schemas, or guessed interfaces instead of concise repo-local references.

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

`harness.yaml` MUST declare the required command-entry fields, and `harness:check` MUST validate that registered commands include them.

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

Rationale: long-running agent work succeeds when open-ended work is converted into checkpointed milestones with acceptance criteria, validation commands, a stop-and-fix rule, and a live status/audit log.

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

### 13.8 Agent runtime

Persistent artifact: `docs/policies/agent-runtime.md`.

Requirements:

- the repository MUST document the default sandbox posture expected for agents;
- the repository MUST document the default approval posture expected for agents;
- the repository MUST document whether network access is allowed by default, forbidden by default, or command-specific;
- the repository MUST document writable roots and any paths agents MUST NOT read or write;
- external tools, connectors, MCP servers, skills, browser/computer-use tools, or cloud agents MUST be listed only when they unlock a real repeated workflow;
- tool-specific configuration files MAY exist, but this policy remains the human- and agent-readable source of intent;
- broad full-access modes MUST be treated as exceptional and justified by the surrounding environment or workflow.

Empty-repo output: state the safe default: local workspace writes only, no broad network by default, no secrets access, destructive commands require approval, and external tools are added only after a repeated workflow needs them.

Mechanical enforcement: mature repositories SHOULD check command registry metadata against the agent-runtime policy, especially network, credential, destructive, and filesystem escalation flags.

Agent failure mode addressed: agents assuming permissions, network, credentials, or external tools that the repository has not made safe or repeatable.

### 13.9 Agentic audits

Persistent artifact: `docs/policies/agentic-audits.md`.

Requirements:

- agentic audits MUST use versioned prompts and rubrics stored under `docs/quality/audits/`;
- audit output MUST follow `docs/quality/audits/output-schema.md`;
- audit findings MUST include scope, evidence, severity, suggested remediation, and whether the finding should become a deterministic check;
- executable audit integration is a Conditional Module that MUST NOT be required until the repository has a selected CI provider, safe secret-management path, and enough code/docs for semantic auditing to be useful;
- agentic audits MUST NOT silently rewrite normative rules or commit broad changes without review;
- repeated audit findings SHOULD be promoted into deterministic checks, linters, schemas, templates, or command validations when possible.

Rationale: deterministic checks are best for structure, metadata, links, command schemas, and known invariants. Agentic audits are useful for semantic drift, stale assumptions, weak docs, architecture drift, and violations of golden principles that are not yet easy to express as deterministic rules.

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

The harness assumes agents MAY run in sandboxes with approval controls, but it does not mandate a specific agent runtime. `docs/policies/agent-runtime.md` is the durable policy for repository-local runtime expectations.

The specific agent runtime, sandbox, approval, and operator-confirmation posture are `Implementation-defined`; repository guidance MUST document the selected behavior before agents rely on it.

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

The initial harness MUST include `scripts/check-harness.sh`. It MUST verify at least:

- REQUIRED files and directories from `harness.yaml` exist;
- `HARNESS.md` is present;
- `AGENTS.md` is present and not excessively large;
- `code_review.md` is present and referenced from `AGENTS.md`;
- `docs/index.md` and `docs/commands.md` are present;
- every durable doc under `docs/`, plus `code_review.md`, has the required metadata fields;
- every markdown file under `docs/` is listed in `docs/index.md` unless explicitly exempted in `harness.yaml`;
- every command entry in `docs/commands.md` includes the required fields declared in `harness.yaml`;
- every executable script in `scripts/` is either registered in `docs/commands.md` or explicitly exempted;
- obvious secret patterns are not present in tracked text files;
- starter templates exist.

As the repository matures, mechanical checks SHOULD be added for:

- broken links;
- stale context metadata;
- undocumented commands beyond `scripts/`;
- missing test/build commands after a language is selected;
- architecture boundary violations;
- generated files out of date;
- stronger secret scanning;
- dependency policy violations;
- unowned docs;
- missing decision records for major changes;
- unchecked drift from golden principles.

### 16.1 Deterministic checks and agentic audits

The harness MUST distinguish deterministic checks from agentic audits.

Deterministic checks are suitable for CI gates when they can evaluate objective repository structure: required files, metadata, index coverage, command schemas, script registration, secret patterns, generated-reference freshness, dependency rules, and architecture boundaries.

Agentic audits are model-assisted semantic reviews. They MAY evaluate documentation quality, stale assumptions, architecture drift, product-domain quality, scorecard changes, and adherence to golden principles using versioned prompts and rubrics. They SHOULD be advisory by default. They MAY become ratcheted or blocking only for narrow, high-confidence findings with low false-positive risk.

Agentic audit modes:

- `advisory`: reports findings and may propose plans or PRs, but does not block merge;
- `ratchet`: blocks only new severe regressions or missing required audit output;
- `gate`: blocks merge on configured findings and should be used only after the prompt/rubric has proven stable.

Executable agentic audits are a Conditional Module. When enabled, the repository MUST document provider credentials, secret names, scopes, rotation owner, local fallback, CI schedule, audit prompts, output schema, and how findings update `docs/quality/scorecard.md`.

Mechanical checks SHOULD emit remediation instructions that tell future agents exactly which file to update. Checks should not merely fail; they should teach the next agent how to repair the harness.

## 17. Harness Health and Drift Cleanup

Status: Concrete Now

The repository MUST treat drift as normal and cleanup as recurring work.

`docs/quality/harness-health.md` tracks the current harness maturity, missing mechanical checks, stale docs, and next improvements.

`docs/quality/scorecard.md` grades the harness and, once code exists, each major product domain or architecture layer. It tracks gaps over time so agents can choose small cleanup tasks rather than letting debt compound.

`docs/quality/golden-principles.md` stores opinionated, durable principles that should remain true across the repository. Principles SHOULD be promoted into mechanical checks when documentation alone stops preventing drift.

`docs/quality/drift.md` defines the cleanup process. Drift includes:

- docs disagreeing with commands;
- commands missing from registry;
- repeated agent mistakes;
- unindexed docs;
- stale status;
- unchecked generated files;
- inconsistent architecture patterns;
- obsolete plans;
- policies with no enforcement path;
- quality scorecard entries with no owner or next action;
- golden principles that are repeatedly violated but not mechanically enforced.

When drift is found, agents SHOULD fix the smallest durable source of the drift, not just the symptom.

## 18. Initial Commit Blueprint

Status: Concrete Now

The first commit MUST create the following files. “Routine read” means agents SHOULD read it often at task start. “Relevant read” means agents SHOULD read it only when the task touches that area.

| Path | Role | Kind | Update when | Agent read | Stores |
|---|---|---|---|---|---|
| `HARNESS.md` | Root harness charter | Normative / rationale | harness philosophy, operating model, source-backed rationale, self-propagation rules, or major harness artifact relationships change | Relevant read for harness work and onboarding; not routine for every small change | initialized harness rules and rationale |
| `AGENTS.md` | Short agent map | Navigational | important entrypoints, forbidden actions, command discovery, done criteria, or plan rules change | Routine read | navigational guidance |
| `README.md` | Human entrypoint | Navigational | repository purpose, setup, status, or primary commands change | Routine read for humans; relevant for agents | durable overview |
| `code_review.md` | Stable review doctrine | Policy / checklist | review expectations, evidence requirements, risky-change categories, or handoff expectations change | Routine before completion and during reviews | normative review guidance |
| `harness.yaml` | Machine-readable harness manifest | Mechanical check input | required files, metadata fields, command schema, check rules, generated/reference dirs, or specialization hooks change | Not routine; read when editing checks or harness structure | checkable harness structure |
| `docs/index.md` | Documentation map | Navigational | docs are added, removed, renamed, split, deprecated, or status changes | Routine read | durable doc index |
| `docs/status.md` | Current repository state | Context | maturity, active plans, risks, languages, services, or deployment state changes | Routine read | durable project status |
| `docs/glossary.md` | Shared vocabulary | Context | terms, acronyms, product concepts, or architecture names appear | Relevant read | durable vocabulary |
| `docs/architecture.md` | Architecture map and boundary procedure | Context / policy | source code, services, packages, APIs, dependency boundaries, or generated code are added | Relevant read | durable architecture context |
| `docs/commands.md` | Command registry | Normative / checklist | any setup, validation, build, test, lint, run, release, migration, data, or harness command changes | Routine read | command truth |
| `docs/harness/README.md` | Harness-maintenance map | Navigational | harness-maintenance docs are added, removed, renamed, or reclassified | Relevant read for harness work | harness context map |
| `docs/harness/source-rationale.md` | Source-backed harness rationale | Context / rationale | source-backed rationale, public links, or preserved quotes change | Relevant read for harness work | durable why behind harness design |
| `docs/harness/coverage-model.md` | Coverage classifications | Normative context | classification scheme changes | Relevant read for harness work | durable concern taxonomy |
| `docs/harness/self-propagation.md` | Self-propagation procedure | Policy | update triggers or initialized-harness rules change | Relevant read for harness work | durable maintenance rules |
| `docs/harness/specialization.md` | Specialization procedure | Policy | language, framework, API, data, deployment, release, or monorepo specialization rules change | Relevant read for specialization work | durable specialization rules |
| `docs/generated/README.md` | Generated context directory guide | Policy / placeholder | generated schemas, API maps, route indexes, dependency graphs, or other generated references are added | Relevant read | generated reference rules |
| `docs/references/README.md` | External/reference context guide | Policy / placeholder | curated vendor/tool/API/design references are added or refreshed | Relevant read | reference context rules |
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
| `docs/policies/agent-runtime.md` | Agent runtime posture | Policy | sandbox, approval, network, writable-root, external-tool, or connector posture changes | Relevant read | normative runtime rules |
| `docs/policies/configuration.md` | Config/env policy | Policy | env vars, config loader, secret configuration, runtime config changes | Relevant read | normative config rules |
| `docs/policies/dependencies.md` | Dependency policy | Policy | package managers, dependencies, lockfiles, licenses, or supply chain assumptions change | Relevant read | normative dependency rules |
| `docs/policies/testing.md` | Testing policy | Policy | test strategy, validation gates, fixtures, snapshots, or parse-boundary rules change | Relevant read | normative quality rules |
| `docs/policies/documentation.md` | Documentation lifecycle | Policy | docs structure or maintenance process changes | Relevant read | normative docs rules |
| `docs/policies/knowledge-ingestion.md` | External knowledge promotion | Policy | external source handling, context metadata, or failure feedback rules change | Relevant read | normative knowledge rules |
| `docs/policies/generated-files.md` | Generated file rules | Policy | codegen, generated references, build artifacts, or regeneration commands change | Relevant read | normative generated-file rules |
| `docs/policies/agentic-audits.md` | Agentic audit policy | Policy | audit modes, provider posture, secret handling, prompt/rubric rules, or output handling changes | Relevant read when enabling or interpreting audits | normative semantic-audit rules |
| `docs/quality/checklist.md` | Change completion checklist | Checklist | review criteria or done definition changes | Routine before completion | quality checklist |
| `docs/quality/drift.md` | Drift cleanup process | Policy / checklist | cleanup process or drift categories change | Relevant read | durable cleanup rules |
| `docs/quality/harness-health.md` | Harness maturity and gaps | Context / checklist | checks, maturity, gaps, or harness risks change | Relevant read | durable harness status |
| `docs/quality/scorecard.md` | Quality scorecard | Context / checklist | grades, gaps, owners, or next cleanup actions change | Relevant read | quality trajectory |
| `docs/quality/golden-principles.md` | Durable taste and architecture principles | Policy / checklist | recurring review feedback or important invariants should become permanent guidance | Relevant read | durable principles |
| `docs/quality/failure-feedback.md` | Agent failure loop | Policy / log | repeated agent failure patterns or remediations occur | Relevant read after failures | durable feedback memory |
| `docs/quality/audits/README.md` | Audit prompt map | Navigational | audit prompts, rubrics, schemas, or modes change | Relevant read when running audits | audit map |
| `docs/quality/audits/*.prompt.md` | Agentic audit prompts | Template / audit rubric | audit scope, scoring rubric, or expected findings change | Relevant read when running audits | versioned semantic-audit prompts |
| `docs/quality/audits/output-schema.md` | Audit output schema | Schema / checklist | audit output fields or downstream consumers change | Relevant read when running audits | structured audit result contract |
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
8. The diff was reviewed against `code_review.md`, including risky-change and documentation checks where relevant.
9. No secrets or local-only artifacts were committed.
10. The harness health check passes or its failure is explained with a follow-up plan.

## 21. Acceptance Checklist for the Initial Harness

Status: Concrete Now

The initial harness is acceptable when:

- `HARNESS.md` exists at the root and is the root harness charter.
- `AGENTS.md` is short, navigational, and points to durable docs.
- `README.md` explains the scaffold and points to `HARNESS.md`, `AGENTS.md`, `docs/index.md`, and `docs/commands.md`.
- `docs/index.md` catalogs the docs.
- `harness.yaml` declares required files, docs metadata, command schema, enabled deterministic checks, and agentic audit prompt locations.
- `code_review.md` defines stable review doctrine and is referenced from `AGENTS.md`.
- `docs/commands.md` registers `harness:check`.
- all durable docs include required metadata headers.
- `docs/harness/` contains harness-maintenance docs including source-backed rationale, coverage model, self-propagation, and specialization guidance.
- `docs/generated/` and `docs/references/` contain README files explaining generated and reference context.
- `docs/context/` contains safe placeholders for durable business, product, operations, support, and external-source context.
- `docs/decisions/` contains a process and template.
- `docs/plans/` contains a process, template, and active/completed directories.
- `docs/policies/` contains starter policies for security, agent runtime, agentic audits, configuration, dependencies, testing, documentation, knowledge ingestion, and generated files.
- `docs/quality/` contains checklist, drift, harness-health, scorecard, golden-principles, failure-feedback, and audit prompt/schema docs.
- `scripts/check-harness.sh` exists and passes.
- `.editorconfig`, `.gitattributes`, and `.gitignore` exist.
- No essential rule exists only outside the repository.

## 22. Agent Initialization Procedure

Status: Concrete Now

When an agent initializes an empty repository from this specification, it MUST:

1. Create the tree in Section 7.
2. Populate every REQUIRED file with useful starter content, not empty placeholders, except `.gitkeep` files.
3. Create `HARNESS.md` as the root harness charter, preserving the source-backed rationale and linking to `docs/harness/source-rationale.md`.
4. Ensure `AGENTS.md` remains short and references `HARNESS.md`, `code_review.md`, `docs/index.md`, `docs/commands.md`, and `docs/status.md`.
5. Create `harness.yaml` with required file, metadata, command, deterministic-check, and agentic-audit declarations.
6. Add required metadata headers to all durable docs.
7. Register `harness:check` in `docs/commands.md`.
8. Make `scripts/check-harness.sh` executable when the filesystem supports it.
9. Run `sh scripts/check-harness.sh`.
10. Do not commit the full setup `SPEC.md` unless explicitly asked; if retained, archive it under `docs/harness/initializer-spec.md` and mark it non-routine.
11. Report created files, validation status, and any limitations.
