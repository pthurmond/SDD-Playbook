# The Peppers Project

The first such workflow is from an old collegue and friend, Chad Peppers. Chad is extremely smart. He has two decades of experience, much of which has been working with Drupal and Laravel. Over the last year he has built a practice of agent orchestration that leverages Spec-Driven Development and sets up guardrails (lots of checks and balances and rules) that keeps the agents inline and functioning correctly. His process has led to such a solid workflow and process (which still keeps the human in the loop) that he has gone the last FOUR MONTHS without a single QA failure.

## Basic Concept

The basic concept for the overarching system is "Pipeline <--> Skills <--> Brain".

The steps for AI are: Plan Code, Test, and Document.

1. With Plan building a spec file that follows the EARS format.
2. Code being the next step where the code is written.
3. Test where the agent tests the changes and builds automated tests when they do not exist, with an emphasis on Playwright tests (end-to-end testing) and adding unit tests as needed.
4. Then Document, somethign that we are all bad at but we should all be doing anyway. Having the agent document the feature or changes so that we humans have a reference and can follow the system.

## The Peppers Brain

The core concept of The Peppers Brain (my labeling, not his) is to have a knowledge graph and map and documentation of yours system that is kept up to date. His team uses an MCP server to make it easier to look into their codebases.

First, you have to establish this brain. The tools he pointed me to primarily are [Codegraph](https://github.com/colbymchenry/codegraph) (the MCP), [OpenViking](https://github.com/volcengine/OpenViking) (the knowledgebase), and [OpenWiki](https://github.com/langchain-ai/openwiki/) (the documentation). He also mentioned working with [Tree-sitter](https://tree-sitter.github.io/tree-sitter/). Once, you have setup those first three tools you then have to force the AI agents to use this Brain. This reduces context bloat and improves performance.

## The Peppers Plan

Chad's process includes following [Github's Spec-Kit](https://github.com/github/spec-kit/tree/main) to define the SDD structure and scope. He also adds his own project-specific standards which mostly entails focusing on the standards for the language, framework, and tools they are using. Example: Using the common PHP, Drupal, or Laravel industry standards. Same with CSS/SCSS or JavaScript/TypeScript. Whatever language or toolset that you are building upon you need to find and define the standards and best practices you need your project to follow.

The plan includes doing static analysis for security, for which he uses [Semgrep](https://github.com/semgrep/semgrep). Other forms of static analysis can also be implemented including Linters, type checkers, code quality tools (phpstan, phpcs, Drupal code, etc), other security/SAST tools, Bug finders/deep analysis, dependency/SCA scanners, complexity/maintainability, IaC/config scanners, container/image scanners, license compliance, and formal/memory safety. So the question becomes, how tight will your process be?

Beyond this, Chad's process includes mutation testing. His configuration also forces agents to fix the bugs it finds and/or creates. Though he did say you need to limit the blast radius of the scanning and searching to just the context of what you are testing or what was changed. This avoids slowdowns and irrelevant context and out of context testing.

One last thing Chad left me with is his own [workflow.md](exampes/peppers-workflow.md) document. You take the contents of the workflow.md file, modify it for your project, and provide it to Claude in a prompt after you setup the aforementioned brain. Then see what it comes up with.

He did say that the first setup always has bugs and needs tuning. Beyond that, he also runs a subsession that watches the new pipeline end-to-end to help with working out those bugs. This evaluation process to ensure agents are sticking to the process can be manual, however, what Chad found works best is to have one Claude instance spin up and observe another Claude instance running the pipeline and see where it fails and gets sneaky (avoiding tools or skirting around requirements).

He uses [tmux](https://github.com/tmux/tmux) for this. This would be done in the initial phases to make sure you have a clean end-to-end run. He would have it record and score each run until the system works then cut over to real use. For those unfamiliar, tmux is a headless shell that you can attach or detach from. The manager agent can spin up a tmux and view it just like it would be if you were running a new terminal session. This allows for the best testing experience.

Currently, Chad runs 6 agents + 1 manager for the work. It requires 2 to 3 Claude Max plans to run it. Which is about $600 per month for this collection that you can potentially see a lot of value out of.

Try it out and tell us if this worked out for you.

## Drupal and Droost

Chad is a long-time Drupal developer. As such he has been building tooling around Drupal that follows his process and provides a path to agentic orchestration for Drupal projects. He calls his tool [Droost](https://www.droost.org/). It is made up of multiple pieces including [Droost](https://www.drupal.org/project/droost) (the brain), [Droost Workflow](https://github.com/Ignibyte/droost-workflow) (the workflow), and soon a tool called Droost CMS.
