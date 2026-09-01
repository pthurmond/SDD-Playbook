# 03 - Programmatic Orchestration

Programmatic orchestration is just code that passes bounded context between model calls and records what happened. Use it when the control you need is clear. A short script is often easier to inspect than a platform, but neither one supplies product judgment, security review, or operational ownership.

The examples are illustrative, not copy-paste production loops. Verify the current SDK, model, pricing, permission model, data handling, and failure behavior with the provider before using one.

## Using the Google GenAI SDK (Native API Loops)

If you need a small loop without a framework, call a provider API directly. Model names, limits, and prices change, so treat `Gemini 1.5 Flash` below as an example identifier, not a recommendation or a stable default.

```python
from google import genai
from google.genai import types

client = genai.Client() # Uses GEMINI_API_KEY

def evaluator_optimizer_loop(spec: str, max_iterations=3):
    """
    Implements the Evaluator-Optimizer pattern for code generation.
    """
    code_draft = ""
    for i in range(max_iterations):
        print(f"\n--- Iteration {i+1} ---")
        
        # 1. Optimizer step
        implementer_resp = client.models.generate_content(
            model='gemini-1.5-flash',
            contents=f"Spec:\n{spec}\n\nCurrent Draft:\n{code_draft}\n\nImprove the code to meet the spec.",
            config=types.GenerateContentConfig(temperature=0.2)
        )
        code_draft = implementer_resp.text
        print("Generated Code Draft.")

        # 2. Evaluator step
        evaluator_resp = client.models.generate_content(
            model='gemini-1.5-flash',
            contents=f"Spec:\n{spec}\n\nCode:\n{code_draft}\n\nCompare the code with the stated requirements and constraints. Reply exactly 'PASS' only when no violation is found. Otherwise, list the failures.",
            config=types.GenerateContentConfig(temperature=0.0)
        )
        evaluation = evaluator_resp.text
        
        if evaluation.strip() == "PASS":
            print("Evaluation PASSED.")
            return code_draft
        else:
            print(f"Evaluation FAILED. Feedback: {evaluation}")
    
    print("Max iterations reached. Requires human review.")
    return code_draft
```

## OpenAI Swarm (a handoff teaching example)

[OpenAI Swarm](https://github.com/openai/swarm) is a lightweight educational project for demonstrating a multi-agent handoff pattern. It is useful for learning the shape of routing. Check the project's current status and use a maintained, appropriate tool before building a production workflow.

The SDD point is smaller than the library: route only a task that is already classified and bounded.

```python
from swarm import Swarm, Agent

client = Swarm()

def escalate_to_architect():
    """Handoff to the Architect when structural changes are needed."""
    return architect_agent

def assign_to_implementer():
    """Handoff to Implementer when tasks are isolated."""
    return implementer_agent

router_agent = Agent(
    name="Router",
    instructions="Review the ticket. If it requires API changes, route to Architect. If it is a localized bug fix, route to Implementer.",
    functions=[escalate_to_architect, assign_to_implementer]
)

architect_agent = Agent(
    name="Architect",
    instructions="You draft `02-technical-plan.md` based on the requirements."
)

implementer_agent = Agent(
    name="Implementer",
    instructions="You write code based on `agent-task-prompt.md`."
)
```

## LangGraph (Stateful Graphs)

For stateful loops with pause points and explicit execution graphs, LangGraph is one option. Evaluate it against the operational needs, team skills, security model, and maintenance cost of your particular system rather than treating any framework as the default.

LangGraph treats the SDD workflow as a state machine:
1. `Node: Draft Spec` -> `Node: Review Spec` -> `Conditional Edge: Approved?`
2. If No -> Back to `Draft Spec`.
3. If Yes -> Proceed to `Node: Implement`.

*Reference:* [LangGraph Multi-Agent Workflows](https://langchain-ai.github.io/langgraph/tutorials/multi_agent/multi-agent-collaboration/)

## The Takeaway

Do not start with a large orchestration framework. Start with the smallest controlled loop that can demonstrate value. Add state, routing, or a framework only when a specific failure or coordination problem requires it. The SDD artifacts should stay readable and reviewable regardless of the tool.
