---
type: source
status: immutable
ingested: 2026-04-22
tags: [type/source, brand/37m]
---

# The Agentic SaaS Model: The Zero Human Company Approach to Product Development

> **Source document — do not modify.** This is the raw ingested source. Synthesized wiki pages derived from this document are in `30-37Metrics/Admin/operating-model.md`, `40-Forge42/Architecture/agent-org-chart.md`, and `40-Forge42/Architecture/handoff-package-spec.md`.

---

The classical SaaS model assumes a cross-functional team of humans. Product managers write PRDs, architects write HLDs, developers write code, QA writes tests, ops writes runbooks. Each document serves both as a specification and as a coordination artifact between humans who need to stay aligned.

The Zero Human Company model inverts that assumption. A single human director orchestrates a suite of AI agents that produce the same documentation and the same software, operating in two distinct phases connected by a structured handoff. The documents do not go away. If anything, they matter more, because they are now the primary interface between agents rather than between people. But the economics, the cadence, and the role of each document change substantially.

This post describes the full agentic documentation stack used by a Zero Human Company: what gets produced, which suite produces it, where the human director weighs in, and how the business-planning side hands off to the software-development side.

## The Two Suites

The Zero Human Company runs on two deliberately separated tool suites.

**The Business Planning Suite** runs on ChatGPT-backed agents orchestrated through Paperclip Hermes, with Claude available for refinement passes. This suite covers everything from market research to brand design to product requirements to high-level architectural planning. The deliverable of this suite is not software. It is a complete, structured documentation package that specifies what is to be built.

**The Software Development Suite** runs primarily on Claude Code. Parallel work streams are handled either by Claude Code sub-agents or by multiple Claude Code terminals running in coordination. For cost-sensitive or routine tasks this suite extends to OpenCode with OpenRouter models, and to locally hosted models where appropriate. The deliverable of this suite is running, tested, deployed software.

The two suites are separated for three reasons. First, the tooling optimized for product thinking, branding, and business analysis is different from the tooling optimized for writing and shipping code. Second, Claude Code operates within specific constraints set by Anthropic, and the agentic planning happens outside of those constraints so that the coding agent stays well within them. Third, keeping the two phases distinct enforces discipline. The handoff forces the business side to finish its thinking before the dev side starts spending tokens on implementation.

The interface between the two suites is a structured documentation package, described later in this post.

## The Business Planning Suite

The business suite is where the thinking happens. It is staffed by specialist agents that correspond roughly to the roles a classical SaaS company would hire into: a strategist, a market researcher, a business analyst, a product manager, a brand designer, a graphic designer, a head of software or CTO-equivalent for architectural planning, and a technical writer. Paperclip Hermes is the orchestration layer that routes work between them and maintains the shared context.

These agents do not code. They produce documents, assets, and specifications. Their output feeds into the handoff package.

Because the business suite is where the expensive strategic thinking happens, this is also where the human director spends most of their attention. The director reviews outputs at defined checkpoints, corrects direction, approves or rejects deliverables, and ultimately signs off on the handoff package before it is released to the development suite.

## The Software Development Suite

The development suite is where code gets written. Claude Code is the primary operator, consuming the handoff package and executing against it. For a given feature or service, work is parallelized either by spinning up Claude Code sub-agents or by running multiple Claude Code terminals against different parts of the codebase. A well-structured handoff package makes this parallelization possible, because each sub-agent gets a clear, bounded scope.

Not every task needs to run on Claude. Routine refactors, boilerplate generation, documentation extraction, and similar low-judgment work can be routed through OpenCode against cheaper OpenRouter models, or through locally hosted models where latency and cost matter more than raw capability. The model routing policy, defined in the handoff package, tells the dev suite which tasks go to which model.

The development suite reports back to the human director through commits, test results, deployment status, and evaluation reports. The director reviews at defined checkpoints but does not generally intervene mid-task.

## The Handoff Package

The handoff package is the single most important artifact in the Zero Human Company model. It is the structured set of documents that the business suite hands to the development suite, defining everything the dev agents need to know to build the product without further input from the business side.

A well-formed handoff package contains a build brief, the PRD, the system architecture brief, the API specification, the data model, a prioritized backlog of scoped tasks, the NFRs and acceptance criteria, the design tokens and component specs, the ADRs that constrain technical choices, the model routing policy, the cost budget, and the human checkpoint schedule. These are described in detail in the phase-by-phase breakdown below.

The package is version-controlled, and every subsequent change to it is logged. When the dev suite needs clarification, the question is routed back to the business suite through the orchestrator rather than answered by guesswork. When the business suite changes direction mid-build, the change is reflected as an updated handoff package with an explicit diff.

## Strategy and Discovery

The strategy and discovery phase runs entirely in the business suite.

**Vision Brief.** The founder's intent, translated by the strategist agent into a structured one-pager covering long-term vision, target market, differentiation, and business model. The human director writes or approves the initial seed.

**Market Brief.** The agentic replacement for the MRD. A market researcher agent runs structured web research, competitor analysis, pricing surveys, and customer problem synthesis, and returns a Market Brief documenting the opportunity. Because agents can do this research cheaply and quickly, the Market Brief is refreshed at every major product milestone rather than once at the beginning.

**Business Brief.** The agentic replacement for the BRD. Captures business objectives, success metrics, scope, and constraints in a format structured enough for downstream agents to parse. The business analyst agent produces this; the human director signs off.

## Product Definition

**Product Requirements Document.** The PRD remains the central product artifact, but it is restructured for agent consumption. Each user story is explicit and atomic, each acceptance criterion is measurable, and each feature has an explicit scope boundary so that downstream dev sub-agents know exactly where their responsibility ends. The product manager agent drafts it; the human director edits and approves it.

**Backlog.** User stories and epics are generated from the PRD by the product manager agent and written in a format that the dev suite can ingest directly. Each item carries estimated complexity, dependencies, and acceptance criteria.

**Persona Cards.** Generated by the market researcher agent from the Market Brief. Enterprise SaaS products get separate personas for buyer, admin, and end user.

**Jobs to Be Done Statements.** Supplement the personas with outcome-focused framing.

## Design

**Functional Specification.** Where the classical model has a separate FRD, the agentic model typically folds functional requirements into an expanded PRD, because a single agent-readable document is easier to maintain consistently than two linked ones. When a separate FRD does make sense — usually for complex systems with heavy business logic — it is generated by the business analyst agent.

**Software Requirements Specification.** The SRS is authored by the head-of-software agent and covers functional requirements, non-functional requirements, and use cases at the system level. In the agentic model it is written in a form that Claude Code can parse section by section and route to different sub-agents.

**Design Tokens and Component Specs.** The brand designer and graphic designer agents produce the visual language: color tokens, typography, spacing, component specs, iconography. Where the classical model leans heavily on Figma as the source of truth, the agentic model leans toward code-ready tokens and component specifications so that the dev suite can implement directly from the spec.

**Information Architecture.** Generated as a structured sitemap with permission overlays for multi-tenant products.

## Architecture and Technical Design

**System Architecture Brief.** The agentic equivalent of the HLD. Covers services, data stores, integrations, deployment topology, and trust boundaries. The head-of-software agent produces this; the human director reviews and approves major architectural choices.

**Component Specs.** The agentic equivalent of the LLD. Rather than one monolithic LLD, the agentic model produces one component spec per service or module, sized so that a single Claude Code sub-agent can own it end to end. This decomposition is what makes parallel dev-suite execution possible.

**Architecture Decision Records.** ADRs remain in markdown in the source repository at `doc/adr`, with the standard lifecycle of proposed, accepted, and superseded. In the agentic model the head-of-software agent drafts ADRs during planning, and Claude Code consults them during implementation to avoid re-litigating settled decisions. New decisions that surface during implementation generate new ADRs, which the human director reviews.

**API Specification.** OpenAPI remains the contract. In the agentic model, the spec is produced during planning rather than discovered during implementation, because the dev suite uses it to generate server stubs, client SDKs, contract tests, and mock servers in parallel.

**Data Model.** Entity-relationship diagrams and schema-as-code, including the explicit tenant isolation strategy for multi-tenant products.

**Non-Functional Requirements.** Performance, scalability, availability, security, maintainability, and observability targets, each expressed as a measurable metric so that the dev suite can verify compliance automatically.

**Threat Model.** Generated by a security-focused agent using STRIDE or a similar framework. The human director reviews and approves, because threat modeling is a domain where agent hallucination has real consequences.

## Orchestration and Task Routing

**Agent Org Chart.** Names every agent role in both suites, what each one is responsible for, and which tool or model backs it.

**Orchestration Plan.** Defines the flow of work: which agent produces which artifact, which artifacts feed which downstream agents, and which tasks run in parallel versus in sequence.

**Model Routing Policy.** Specifies which model handles which type of task. Strategic reasoning and architectural decisions route to frontier models. Code generation routes to Claude Code. Boilerplate, refactors, and high-volume simple tasks route to cheaper OpenRouter models or local models.

**Cost Budget.** Per-project and per-feature spending caps, tracked against actual usage.

**Human Checkpoint Schedule.** Defines the points at which the human director must review and approve before downstream work proceeds.

## Implementation

**CLAUDE.md and AGENTS.md.** Live at the root of the repository. Give Claude Code and other coding agents the context they need on every invocation: project conventions, folder structure, how to run tests, how to deploy, and which ADRs and specs to consult before making changes.

**Coding Standards as Config.** Enforced configuration files: linters, formatters, pre-commit hooks, CI checks.

**Service Runbooks.** Generated by the head-of-software agent during planning and maintained by the dev suite as the service evolves.

**Commit and PR Discipline.** Every commit references the backlog item it implements, every PR links back to the relevant spec section.

## Quality Assurance

**Test Strategy.** Specifies required testing levels, tools, environments, and pass criteria.

**Test Cases as Code.** Written by a testing sub-agent directly from the acceptance criteria in the PRD.

**Automated Traceability.** Every backlog item, commit, test, and PR references its upstream spec.

**Evaluation Harness.** A structured set of scenarios the human director runs to confirm the product meets intent before a release is cut.

## Deployment and Operations

**Deployment Runbook.** Pre-deployment checks, migrations, feature flag states, rollback procedures, customer communications.

**Operations Manual.** Day-to-day operational procedures including monitoring, alerting, and routine customer management.

**Incident Response Plan.** Triage agent assesses severity, escalates to human director above a defined threshold.

**SLA, SLO, and SLI Document.** External commitments, internal targets, and the metrics that measure both.

**Disaster Recovery and Business Continuity Plan.** RPO, RTO, backup strategy, failover procedures.

## Security and Compliance

Security and compliance documentation is produced by specialized agents and signed off by the human director. Legal consequences of error are high.

**Information Security Policy, Data Classification Policy, Access Control Policy.** Customized to the specific system architecture.

**Vendor and Third-Party Risk Assessment.** Updated whenever the dev suite adds a new dependency.

**DPA, Privacy Policy, Terms of Service.** Drafted by compliance agent from templates; reviewed by human attorney before publication. Hard line.

**SOC 2 and ISO 27001 Evidence.** Assembled from machine-readable logs, configurations, and audit trails.

**Security Questionnaire Responses.** Pre-filled SIG and CAIQ responses maintained as a structured knowledge base.

## User-Facing Documentation

**Help Center and End User Docs.** Continuously updated as the product evolves.

**Admin Guide.** Tenant-level configuration focus.

**API Reference.** Generated from the OpenAPI spec.

**Release Notes.** Authored from the commit log and backlog, framed for customers.

## Post-Launch and Lifecycle

**Post-Mortems.** Human-led with agent assistance.

**Retrospectives.** Synthesized by orchestration agent from actual work patterns.

**Metrics and KPI Reports.** Automated dashboards reviewed on a defined cadence.

## Human Checkpoints

The standard checkpoints: approval of Vision Brief and Business Brief before product definition, approval of PRD before design and architecture, approval of System Architecture Brief and ADRs before implementation, approval of full handoff package before dev suite starts, evaluation of each shippable increment before release, approval of security or legal documents before publication, review of post-mortems and quarterly retrospectives.

Everything between those checkpoints runs autonomously. When an agent gets stuck or hits a decision it is not authorized to make, it surfaces the question to the director through the orchestrator rather than guessing.

## How It All Flows Together

The flow starts in the business suite. The director seeds a Vision Brief. The market researcher agent produces a Market Brief. The business analyst agent produces a Business Brief. The director reviews and approves.

The product manager agent expands the approved briefs into a PRD and prioritized backlog. The brand and graphic designer agents produce design tokens and component specs. The head-of-software agent produces the System Architecture Brief, SRS, component specs, ADRs, OpenAPI spec, data model, and NFRs. The compliance agent drafts security and legal artifacts. The director reviews at defined checkpoints.

When the full set is complete, the orchestrator assembles the handoff package: every document, version-locked, with a model routing policy and cost budget attached. The director reviews the package as a whole and signs off. Control transfers to the development suite.

Claude Code ingests the handoff package. Sub-agents or parallel terminals pick up component specs and backlog items and begin building. Tests are written alongside code against PRD acceptance criteria. The OpenAPI spec drives SDK generation and contract testing. Cheaper models handle boilerplate under the model routing policy.

At each shippable increment, the director runs the evaluation harness and either approves release or returns items to the backlog. Questions that arise mid-build route back to the orchestrator, which either resolves them automatically or surfaces them to the director.

Post-launch: technical writer updates customer-facing documentation, incident response agent handles monitoring and triage, compliance agent maintains evidence package, orchestration agent synthesizes retrospective.

The cycle repeats. The next feature starts with a new Vision Brief or PRD amendment, flows through the business suite, produces a new handoff package, and gets built by the development suite. The documentation is the interface. New agents can be swapped into either suite without breaking the flow. The human director's role stays constant: seed the intent, approve at the checkpoints, evaluate the output.
