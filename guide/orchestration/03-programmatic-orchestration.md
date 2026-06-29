# 03 - Programmatic Orchestration

To bring SDD documents to life, you need programmatic wrappers that feed specs to models and manage the handoffs. While you can use fully-featured UI tools, writing lightweight Python scripts is often the most reliable way to enforce strict agent boundaries.

Below is an expanded reference of how to build programmatic agent loops.

## Using the Google GenAI SDK (Native API Loops)

If you want absolute control without framework overhead, you can build a raw API loop. This allows you to leverage free-tier models like **Gemini 1.5 Flash** for rapid iterations.

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
            contents=f"Spec:\n{spec}\n\nCode:\n{code_draft}\n\nCheck if the code perfectly meets the spec. If it does, reply exactly 'PASS'. Otherwise, list the failures.",
            config=types.GenerateContentConfig(temperature=0.0)
        )
        evaluation = evaluator_resp.text
        
        if "PASS" in evaluation:
            print("Evaluation PASSED.")
            return code_draft
        else:
            print(f"Evaluation FAILED. Feedback: {evaluation}")
    
    print("Max iterations reached. Requires human review.")
    return code_draft
```

## OpenAI Swarm (Delegation Framework)

[OpenAI Swarm](https://github.com/openai/swarm) is a lightweight library specifically designed to teach the "multi-agent handoff" pattern. It uses functions to pass execution from one agent persona to another.

In SDD, you can use Swarm to route tasks:

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

For production-grade agent loops that require memory, pausing for human-in-the-loop approval, and strict execution graphs, **LangGraph** (by LangChain) is the industry standard.

LangGraph treats the SDD workflow as a state machine:
1. `Node: Draft Spec` -> `Node: Review Spec` -> `Conditional Edge: Approved?`
2. If No -> Back to `Draft Spec`.
3. If Yes -> Proceed to `Node: Implement`.

*Reference:* [LangGraph Multi-Agent Workflows](https://langchain-ai.github.io/langgraph/tutorials/multi_agent/multi-agent-collaboration/)

## The Takeaway

Do not start by installing massive orchestration frameworks. Start with a native API loop (like the Gemini example). When the loop becomes brittle because you need memory or complex routing, upgrade to Swarm or LangGraph. The SDD artifacts remain the same regardless of the framework.
