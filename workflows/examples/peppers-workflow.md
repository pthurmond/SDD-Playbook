You are establishing an enforceable, spec-driven engineering workflow for the PHP project at:

<TARGET_REPO>

The workflow has exactly four user-facing phases:

Plan → Code → Test → Document

Reference implementations are available read-only at:

- /srv/stacks/aic
- /srv/stacks/ucsosv2

Study their specifications, phase hooks, EARS requirements, quality gates, PHPUnit/Infection configuration, Playwright contracts, and UCSOSv2’s docs/wiki implementation. Do not copy their full complexity. Build the smallest reliable workflow that mechanically prevents phases from being skipped.

If <TARGET_REPO> is AIC or UCSOSv2, preserve the existing pipeline. Present the four phases as a façade over it:

- Plan = existing Plan + Design + Playwright Design
- Code = Implement + conditional Style
- Test = Validate + conditional Register/Revalidate
- Document = Complete

Do not delete or weaken existing gates, test thresholds, hooks, or documentation requirements.

Do not import Forge, RLM, ticket-management, or project-specific infrastructure into another repository unless that repository already depends on it.

## 1. Start with discovery

Before writing files:

1. Read AGENTS.md, CLAUDE.md, CONSTITUTION.md, composer.json, package.json, PHPUnit, PHPStan, Infection, PHPCS, Rector, Playwright, MCP, hook, and CI configuration.
2. Inventory existing:
    - specification and planning files;
    - unit, feature, integration, and browser tests;
    - Composer quality commands;
    - git hooks, AI-agent hooks, and CI checks;
    - architecture documentation and code wiki;
    - coverage and mutation-testing thresholds.
3. Report:
    - what already exists;
    - what can be reused;
    - missing enforcement;
    - conflicts between the requested workflow and current repository rules.
4. Preserve all stricter existing rules.
5. Then implement the workflow unless a real architectural or safety blocker requires a decision.

## 2. Required artifacts

Create or adapt:

- `docs/specs/active/`
- `docs/specs/completed/`
- `docs/specs/_templates/WORK.spec.md`
- `docs/specs/_templates/WORK.notes.md`
- `docs/specs/reference/ears.md`
- `docs/specs/reference/workflow.md`
- `docs/wiki/README.md`
- a PHP or Artisan workflow-gate command;
- Composer aliases for the gates;
- local hook integration;
- CI enforcement;
- automated tests for the gate implementation.

Keep the active specification compact. Put investigation logs, long rationale, mutation ledgers, and debugging notes in the companion `.notes.md`.

## 3. Specification contract

Each work item must have one active `WORK-<id>.spec.md` with machine-readable YAML frontmatter:

workflow_version: 1 id: WORK-000 title: … status: active current_phase: plan plan_status: pending code_status: locked test_status: locked document_status: locked browser_testable: yes|no mutation_testing: required|optional-run|skipped mutation_reason: … created_at: … updated_at: …

The body must contain:

1. Problem statement
2. Scope and explicit non-goals
3. Acceptance criteria
4. EARS Requirements
5. File Manifest
6. Test Plan
7. Browser Verification Contract, when applicable
8. Phase Gate Status
9. Risks and rollback
10. Documentation impact
11. Phase receipts

Use this EARS table:

| ID | Pattern | Requirement | Verification Type | Planned Verification | Verified By | Status |
|----|---------|-------------|-------------------|----------------------|-------------|--------|

Use stable requirement IDs such as `R1`, `R2`, and `R3`.

Every requirement must describe one independently verifiable behavior. Every behavioral requirement must map to exactly one automated test unless automation is genuinely impossible. Manual verification requires a reason and an artifact.

Use the five EARS forms:

- Ubiquitous: “The system SHALL …”
- Event-driven: “When <event>, the system SHALL …”
- State-driven: “While <state>, the system SHALL …”
- Optional: “Where <feature is enabled>, the system SHALL …”
- Unwanted behavior: “If <undesired condition>, then the system SHALL …”

Reject vague requirements such as “the system shall be fast,” “work correctly,” or “provide a good user experience.” Require a measurable result, named trigger, observable state, or error response.

The Plan phase leaves `Verified By` empty. The Test gate fills it with an exact test class/method, Playwright spec, or manual artifact.

## 4. Phase rules

### Plan

Allowed writes:

- the active spec and notes;
- architecture decision records needed to settle the plan.

Forbidden:

- production code;
- migrations;
- test code;
- product documentation changes.

The Plan gate must require:

- one and only one active spec;
- no unresolved `TBD` or `[NEEDS CLARIFICATION]`;
- measurable acceptance criteria;
- valid EARS rows with unique IDs;
- an exact file manifest;
- a test type for every requirement;
- browser-testable classification;
- mutation-testing decision;
- security, authorization, data migration, and rollback considerations where relevant;
- human approval before Code when the plan changes public APIs, schema, authorization, or architecture.

### Code

Entry requires Plan PASS.

Allowed writes:

- production PHP;
- configuration;
- migrations;
- frontend implementation;
- selector/test-hook attributes needed for browser testing.

Test files are written during Test, preserving the requested phase order.

The Code gate must require:

- every changed path appears in the planned File Manifest;
- no unrelated cleanup;
- static analysis and formatting are green;
- the implementation satisfies the declared scope;
- no requirement or acceptance criterion was silently removed;
- scope changes return the workflow to Plan.

Do not claim behavioral test success during Code.

### Test

Entry requires Plan PASS and Code PASS.

The Test phase writes and runs tests. It must:

- create or update the tests planned in the spec;
- map each EARS requirement to one exact verification;
- run targeted tests first;
- run the repository’s required regression suite;
- report actual test and assertion counts;
- record exit codes;
- run coverage when the environment supports it;
- run mutation testing when the Plan decision requires it;
- run Playwright MCP verification and checked-in Playwright tests for browser-testable work;
- loop back to Code on any implementation failure.

No test may be reported PASS if the command failed to start, selected zero tests, skipped the behavior, or was not executed.

### Document

Entry requires Test PASS.

This phase must:

- update architecture documentation when contracts or structure changed;
- update user/developer documentation as applicable;
- update the code wiki for affected subsystems;
- add a changelog entry when the project requires one;
- record operational or migration instructions;
- record an after-action note containing surprises, failures, and follow-up work;
- run documentation links, wiki freshness, and citation gates;
- move the spec from `active/` to `completed/` only after all four phases pass.

Documentation may not invent behavior that was not proven during Test.

## 5. Mechanical enforcement

Implement a command appropriate to the repository:

- Laravel: `php artisan workflow:gate <phase>`
- Plain PHP: `php bin/workflow.php gate <phase>`

Expose stable Composer commands:

- `composer spec:lint`
- `composer gate:plan`
- `composer gate:code`
- `composer gate:test`
- `composer gate:document`
- `composer gate:delivery`

The gate command, not a manual edit, is the authority that changes a phase from pending to PASS. Write a receipt containing:

- work ID;
- phase;
- timestamp;
- git commit or working-tree fingerprint;
- commands executed;
- exit codes;
- observed test counts;
- artifact paths;
- output digest.

A spec saying PASS without a matching valid receipt must fail.

Enforce the state machine in three places:

1. Agent PreToolUse hooks:
    
    - block production-code edits until Plan PASS;
    - block test edits until Code PASS;
    - block final documentation/wiki completion until Test PASS;
    - always allow the active spec and companion notes.
2. Stop/phase-completion hooks:
    
    - block completion until the appropriate gate command ran successfully;
    - require real receipts rather than prose claims.
3. CI:
    
    - changed production code requires a completed spec;
    - phases cannot be skipped or reordered;
    - all EARS rows must have verification;
    - all required commands must pass independently of local receipts;
    - stale wiki pages or unresolved citations block delivery;
    - browser contracts and mutation decisions must be satisfied.

Agent hooks are convenience and defense in depth. CI is the enforcement boundary for humans and other tools.

Add negative tests proving the gates fail for:

- no active spec;
- multiple active specs;
- Code attempted before Plan PASS;
- Test attempted before Code PASS;
- duplicate or malformed EARS IDs;
- an EARS row without verification;
- a fabricated PASS without a receipt;
- executable PHP changes with no mutation decision;
- `browser_testable: yes` without a browser contract;
- a browser contract ID missing from Playwright;
- a test command that selected zero tests;
- skipped or “did not run” tests;
- Document attempted before Test PASS;
- stale wiki sources;
- broken `file:line` wiki citations.

Begin hook rollout in report-only mode only long enough to prove the negative tests. Then switch to blocking mode. Do not leave a permanent silent “observe” default.

## 6. PHP coding standards

Apply the repository’s stricter existing rules. Otherwise use:

- PHP 8.5 where supported by the project;
- `declare(strict_types=1);` in every PHP source file;
- PSR-4 namespaces and one class/interface/trait/enum per file;
- typed parameters, properties, and return values;
- `final` by default;
- `readonly` for immutable state;
- constructor injection rather than service location;
- no global mutable state or request-specific static caches;
- no swallowed exceptions;
- no `mixed` when a specific type, generic PHPDoc, value object, or array shape can express the contract;
- generic PHPDoc for collections and arrays;
- enums or value objects for closed domains;
- small methods with one purpose;
- authorization and validation at system boundaries;
- parameterized queries or ORM query binding;
- no secrets, credentials, or environment-specific endpoints in source control;
- class and public/protected API documentation where required by repository policy;
- comments explaining invariants and non-obvious decisions, not restating code.

Required core quality tools:

1. Composer validation and audit
2. Rector dry-run
3. Pint or PHPCS
4. PHPStan at the existing level, targeting level 9 or higher
5. No new baseline or suppression merely to make a gate green

Use Deptrac, PHPat, PHPMD, Semgrep, composer-unused, and duplication checks when already present. Do not make a huge toolchain a prerequisite for the first usable workflow.

For long-lived workers such as FrankenPHP or Octane, explicitly prohibit request-specific state in static properties or persistent singletons.

## 7. PHPUnit standards

Use PHPUnit and the project’s framework test utilities.

- Arrange, Act, Assert.
- One behavior per test.
- Use descriptive behavioral test names.
- Unit-test domain logic without booting the framework where possible.
- Use feature/integration tests for HTTP, database, queues, events, policies, and framework wiring.
- Mock external boundaries, not the class under test.
- Prefer factories and named factory states.
- Keep tests deterministic. Control clocks, randomness, queues, and network access.
- Do not use sleeps as synchronization.
- Treat warnings, risky tests, incomplete tests, and deprecations as failures.
- Do not add `@doesNotPerformAssertions`, ignored tests, or skips to force green.
- Report actual runner counts.
- Require 100% line coverage for new or changed business logic when reliable diff coverage is available. A coverage waiver must be explicit, narrow, and human-approved.
- Remember that coverage only proves execution. It does not prove that assertions detect incorrect behavior.

For UCSOSv2 specifically, preserve its `DatabaseTransactions` rule and do not introduce `RefreshDatabase`.

## 8. Optional mutation testing

Mutation testing uses a tool such as Infection to make small changes to production code—for example reversing a condition, changing a comparison, or removing a return—and then runs the tests. A killed mutant means the tests detected the changed behavior. A surviving mutant identifies weak assertions, untested behavior, dead defense, or sometimes an equivalent mutation.

Mutation testing is optional at the workflow level, but the decision is mandatory during Plan.

Allowed values:

- `required` — must run and meet thresholds;
- `optional-run` — the team elected to run it for additional confidence;
- `skipped` — requires a specific reason.

Default mutation testing to `required` for:

- authorization and security rules;
- money, billing, scoring, or calculations;
- state transitions;
- parsers and validators;
- retry and error-handling logic;
- domain services with branching business rules.

It may be skipped for documentation-only changes, passive configuration, generated files, or thin framework wiring with a specific explanation.

Use existing repository thresholds when stricter. Otherwise begin with:

- MSI ≥ 70%
- Covered Code MSI ≥ 80%

Ratchet these upward after collecting stable results.

Enforcement must verify:

- Infection actually ran;
- the changed source files were included;
- at least one mutant was generated;
- a zero-mutant run is “not measured,” never “100%”;
- the threshold came from configuration or the declared command;
- no source exclusion or ignored-mutator regex was added only to force green;
- surviving mutants are either killed with better tests or entered in a mutation ledger with evidence explaining equivalent, uncovered, or unreachable status.

Run targeted Infection during Test. Put full mutation sweeps in scheduled CI if runtime makes them unsuitable for every commit.

## 9. Playwright MCP and browser testing

Treat Playwright MCP and checked-in Playwright tests as separate evidence:

- Playwright MCP provides interactive browser inspection and a live walkthrough.
- `@playwright/test` provides repeatable, reviewable regression tests.
- MCP verification does not replace a checked-in test.

When `browser_testable: yes`, Plan must include:

| ID | EARS Ref | Role | Route | Setup | Observable Result | Stable Selector |
|----|----------|------|-------|-------|-------------------|-----------------|

Use contract IDs `C1`, `C2`, etc.

During Test:

1. Use Playwright MCP to navigate to the configured application URL.
2. Inspect the rendered page and accessibility/DOM state.
3. Exercise the planned interaction.
4. Capture a screenshot for visual work.
5. Record the URL, role, result, and screenshot path.
6. Write or update a checked-in Playwright spec.
7. Add `// contract: C1` markers linking each test to its contract.
8. Run the targeted spec and record the real result.

The browser-contract gate must fail if:

- a planned contract ID has no matching Playwright marker;
- the targeted spec did not run;
- any contract test failed, skipped, or “did not run”;
- `test.only`, an unjustified `test.skip`, or `test.fixme` remains;
- the test navigated to a login/error page instead of its declared route;
- MCP verification was required but no live walkthrough receipt exists.

Use stable selectors such as accessible roles, labels, `data-test`, or documented framework IDs. Avoid structural CSS selectors, `nth-child`, and fragile class chains. Use the project’s authentication fixtures and helper functions. Make browser data deterministic.

For UCSOSv2, preserve:

- Playwright MCP from the existing `.mcp.json`;
- the browser-server configuration in `playwright.config.ts`;
- per-checkout `PLAYWRIGHT_BASE_URL`;
- the configured WebSocket endpoint;
- worker-scoped authentication fixtures;
- zero silent fallback when the browser server is unavailable.

Never overwrite an owner-managed `.mcp.json` or expose its headers or credentials.

## 10. Code wiki

Use `docs/wiki/` as the generated code-orientation wiki. Do not confuse it with a private `.codex/wiki/` or `.claude/` working-memory directory.

Authority order:

1. Constitution/project rules
2. Architecture decisions
3. Source code
4. Generated code wiki

The wiki orients. It does not define architecture.

Each wiki page should contain:

- Purpose
- Key classes with resolvable `file:line` citations
- Entry points
- Data flow
- External dependencies
- Binding architecture documents
- Provenance and regeneration instructions

Frontmatter should include:

generated: true authority: orients subsystem: … source_commit: … generated_at: … primary_anchors: - app/… wiki_sources: app/…: <content hash>

Implement:

- `wiki:affected` — identify pages whose recorded sources changed;
- `wiki:status` — classify pages as fresh, stale, orphaned, or invalid;
- `wiki:stamp` — update hashes only after prose is regenerated;
- `gate:wiki-freshness`;
- `gate:wiki-citations`.

The Document phase must regenerate affected prose before updating hashes. Stamping stale prose is prohibited. Where practical, fail if source hashes changed and only provenance metadata changed while the page body remained byte-identical.

Every cited file must exist. Every line must be in range. When possible, verify that the cited symbol appears near the cited line.

## 11. Delivery gate

`composer gate:delivery` is the final truth gate. It must verify:

- all four phases are PASS in order;
- all phase receipts are valid;
- formatting and static analysis pass;
- required PHPUnit suites pass;
- coverage policy is satisfied;
- required mutation testing passes;
- browser contracts pass;
- no Playwright tests were skipped or left unexecuted;
- documentation links resolve;
- affected wiki pages are fresh;
- wiki citations resolve;
- no unresolved acceptance criteria, EARS requirements, scope changes, or waivers remain.

Only then archive the spec to `docs/specs/completed/`.

## 12. Final handoff

Return:

1. Discovery and gap analysis
2. The four-phase design and transition rules
3. Files created or modified
4. Composer commands and hook/CI wiring
5. Gate test results, including negative/sabotage tests
6. One small pilot spec taken through all four phases
7. Remaining rollout risks
8. Exact commands developers and agents use to start and complete work

Do not report the workflow as enforced merely because templates exist. Enforcement requires tested blocking hooks plus a CI gate.