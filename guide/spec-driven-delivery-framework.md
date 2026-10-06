# Spec-Driven Delivery — Working Framework

**Author:** Patrick Thurmond  
**Status:** Working draft; agreed framework captured October 5, 2026. Stage minimums, optional artifacts, and client templates remain to be developed.

## Purpose

Spec-Driven Delivery is an AI-enabled method for turning human intent into verifiable outcomes through collaborative definition, bounded construction, and accountable human review.

It applies to software, design, content, comics, analysis, infrastructure, automation, and other work whose outcomes can be described and evaluated. The existing software-development playbook provides engineering practices within this broader model.

This is a personal methodology in development. The repository's [position and scope](00-position-and-scope.md) continue to apply.

## Principles

### Specification begins as a conversation

The human brings goals, domain knowledge, references, and judgment. AI helps explore the problem, identify missing considerations, compare options, and capture a usable definition. People own the decisions expressed in that definition; they need not manually author every sentence or prescribe every parameter.

### Define the decisions that matter

Establish enough shared understanding to prevent unacceptable outcomes and make verification credible. Detail should scale with uncertainty and consequence. The framework does not require exhaustive documentation or a fixed collection of files.

### Let references carry information

The specification is the maintained body of intent, context, constraints, decisions, references, and acceptance conditions that governs construction. It can include prose, diagrams, source code, schemas, design systems, reference images, examples, and tests.

Three complementary ways to specify work:

| Method | Purpose | Example |
| --- | --- | --- |
| Explicit definition | Describe required behavior or outcome | Valid submissions remain possible during a downstream outage. |
| Constraint definition | Bound acceptable solutions | Use services permitted by the client's architecture and data-handling rules. |
| Reference definition | Inherit detail from an authoritative source | Follow this design system and existing interaction pattern. |

Identify which references govern which decisions and how conflicts are resolved. An unaccepted generated output does not become authoritative merely because it exists.

### Give agents broad autonomy within reviewed boundaries

Humans review intent, constraints, interactions, architectural boundaries, and acceptance conditions. Agents can choose ordinary implementation details within those boundaries. They surface conflicting requirements, consequential unresolved decisions, or changes outside their authority.

A platform allowance is a boundary, not blanket approval of every service or configuration on that platform.

### Define evidence before substantial construction

Decide how correctness will be demonstrated before Build. Detailed tests may develop alongside design and implementation, but acceptance must already have meaning.

Evaluate outputs against the definition and authoritative references. Agent confidence is not evidence. Automated verification supplies evidence; accountable human review establishes acceptance.

## Primary process: the Delivery Loop

**Explore → Define → Bound → Review → Build → Verify → Experience → Refine**

These stages describe responsibilities and decisions, not a mandatory sequence of documents. Explore, Define, and Bound often overlap in conversation. Verification and feedback may occur throughout construction.

| Stage | Purpose |
| --- | --- |
| **Explore** | Understand the problem, affected people, context, and desired outcome through human/AI collaboration. Challenge assumptions and discover what is still unknown. |
| **Define** | Capture expected behavior, interactions, experience, success conditions, and relevant failure or recovery states. Establish how the result will be evaluated. |
| **Bound** | Establish constraints, invariants, negative requirements, authoritative references, standards, and the permissible solution space. |
| **Review** | A human reviews the definition, experience, architectural boundaries, constraints, references, and acceptance conditions before substantial construction. |
| **Build** | Humans and one or more AI agents produce the result with autonomy inside the reviewed boundaries. Divide work into coherent units when that improves control or review. |
| **Verify** | Gather evidence that the output satisfies requirements, constraints, references, and acceptance conditions. Use suitable automated checks and specialist or manual review. |
| **Experience** | A human uses or consumes the result as its intended audience would, including important interactions and failure states. An accountable practitioner also reviews the resulting implementation or artifact. |
| **Refine** | Correct the result or its governing definition based on discovered problems. Route work back to the stage where the problem originates. |

### Human review before Build

The readiness question is:

> Is there enough shared understanding that I am comfortable letting an agent build inside these boundaries?

Review whether the constraints cover what would make an otherwise reasonable solution unacceptable. Review user interactions explicitly, along with architectural permissions and limits.

This review does not require the human to select every library, function, or configuration value.

### Human acceptance after construction

Use the result. For an application, walk through it as a normal user: complete the flow, encounter errors, inspect relevant device layouts, and experience degraded behavior. For a comic, read and inspect the page as a reader. For a report, consume it as the intended decision-maker.

For software, a developer must also review architecture, maintainability, security implications, correctness, and surprising agent decisions. Other domains require an appropriately accountable reviewer.

Acceptance considers both verification evidence and the actual experience. Passing checks alone do not establish that the result is suitable.

### Refinement routes

Refine usually returns to Define, and may return to Explore when the problem itself was misunderstood. It can also return directly to Bound, Build, or Verify when the issue is localized.

| Discovery | Return to |
| --- | --- |
| The underlying problem or audience was misunderstood | Explore |
| The intended behavior or experience needs to change | Define |
| A constraint or authoritative reference was missing or incorrect | Bound |
| The definition is sound but construction is defective | Build |
| The evidence is insufficient or a check is missing | Verify |

Changes to reviewed intent, experience, or boundaries require renewed Review before further substantial construction. Corrected outputs are verified and experienced again as appropriate.

Refinement is the feedback loop for achieving the current goal. It is not merely end-of-project polish.

## Secondary process: the Improvement Loop

**Observe → Generalize → Codify → Reuse**

This process improves future delivery. It is distinct from the primary work of getting the active result right and need not block completion.

| Stage | Purpose |
| --- | --- |
| **Observe** | Identify something useful from working experience: a failure, effective technique, missing constraint, or successful pattern. |
| **Generalize** | Determine whether it applies locally, to a client, to a domain, or across the methodology. |
| **Codify** | Capture it in the appropriate project notes, client standards, domain patterns, or framework guidance. |
| **Reuse** | Apply it when relevant and confirm that its assumptions hold in the new setting. |

Learning occurs throughout work, but not every lesson deserves a reusable rule. Switching clients or domains requires fresh discovery and validation of context.

Carry forward applicable methods; reassess assumptions.

For example, defining comic work at panel level can be a reusable production technique. Negative constraints at book, page, and panel levels may become a domain pattern. A restriction on one character's appearance remains specific to that book.

## Client customization

Keep the reusable methodology separate from the client's operating context and the current project's definition.

| Layer | Contains |
| --- | --- |
| Universal framework | Delivery and improvement processes, collaborative specification, autonomy principles, and human review responsibilities |
| Domain guidance | Engineering, visual production, analysis, or other domain practices |
| Client operating context | Business goals, terminology, systems, approved platforms, data rules, design standards, operational expectations, and ownership |
| Project definition | Current problem, outcome, behavior, constraints, references, acceptance conditions, and work units |

A small engagement may keep client context in one document. A larger organization may maintain several referenced sources. The format is optional; relevance and authority must be clear.

Client context carries organizational knowledge. A project constitution captures applicable non-negotiable principles. They can be represented together when useful, but their purposes remain distinguishable.

At the start of a new engagement, establish the client's context afresh. Reuse a prior client's assumptions only when independently confirmed. Improvements are codified at the narrowest useful scope.

## Patterns from visual production

Patrick's Corbin & Milo comic work provides a non-software illustration of this method. These mappings capture the pattern discussed during framework development; they are not a claim that the framework depends on software artifacts.

Reference project: Corbin-The-Scientist comic script (private repo).

| Comic specification element | Delivery pattern |
| --- | --- |
| Story purpose and intended audience | Outcome and audience context |
| Standing references and precedence | Persistent context and authority rules |
| Character consistency rules | Invariants |
| Production and print requirements | Output constraints |
| Page state | Local construction context |
| Panel descriptions | Bounded units of work |
| Negative constraints per book, page, and panel | Constraints at multiple scopes |
| Visual acceptance checklist | Verification conditions |
| Human reading and inspection | Experience and acceptance |
| Correcting the page or its definition | Delivery refinement |

## Illustrative software application

A generic public form must continue accepting valid submissions while its downstream connection is unavailable.

- **Explore:** Identify what fails, who is affected, and the desired continuity.
- **Define:** Describe validation, submission, confirmation, failure, and recovery behavior.
- **Bound:** Establish permitted platforms, trust boundaries, data handling, retention, and compatible downstream contracts.
- **Review:** A human checks the experience, boundaries, and evidence plan.
- **Build:** Separate functional implementation and styling when useful; construct resilience and integration behavior within scope.
- **Verify:** Check normal delivery, invalid input, timeouts, temporary retention failure, retries, duplicate handling, recovery, and relevant experience standards.
- **Experience:** A human completes the flow and encounters representative errors and degraded behavior; a developer reviews the implementation.
- **Refine:** Correct whichever layer caused the observed problem.

This example illustrates the methodology. It does not assert a particular vendor's compliance, prove a production implementation, or document a client's systems.

## Executive explanation

> I work with AI to define the problem, desired experience, constraints, and evidence of success. Humans review the decisions and boundaries that matter; agents then build with room to solve implementation details. We verify the result against that definition, use it as the intended audience would, and refine what needs to change. The same process applies beyond software. Separately, we capture methods worth reusing while keeping client and project assumptions in their proper scope.

## Next exercise

The next collaborative exercise will define:

- What each delivery stage must produce at minimum.
- Which outputs and artifacts are optional.
- How the minimum scales with risk, uncertainty, and domain.
- How to capture client context without creating unnecessary ceremony.

These points remain open. This draft preserves the agreed framework without prematurely completing that exercise.

## Related playbook material

- [Position and scope](00-position-and-scope.md)
- [Software workflow](02-workflow.md)
- [AI-agent workflow](04-ai-agent-workflow.md)
- [Human-in-the-loop guide](10-human-in-the-loop.md)
- [Decision ladder](../decision-ladder.md)
- [Task brief](../templates/task-brief.md)
