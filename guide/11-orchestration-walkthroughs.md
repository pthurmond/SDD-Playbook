# 11 — Multi-Agent Orchestration Walkthroughs

This guide provides practical, step-by-step walkthroughs for running multi-agent Spec-Driven Development (SDD) using known orchestration frameworks and developer platforms. We cover Google, OpenAI, Anthropic, and the open-source agent platform OpenHands.

---

## 1. Google Gemini API (Python GenAI SDK)
Google’s developer tools allow you to build lightweight, fast multi-agent loops leveraging the **free tier** of Gemini 1.5 Flash (in Google AI Studio).

### The Pattern: Clarifier-Implementer Pipeline
This script runs a Clarifier Agent to scan a spec for ambiguities. If it finds none, it passes the context to the Implementer Agent to generate the code.

### Step-by-Step Setup
1. Install the official Google GenAI SDK:
   ```bash
   pip install google-genai
   ```
2. Set your API key:
   ```bash
   export GEMINI_API_KEY="your-api-key-here"
   ```
3. Run the following Python script:

```python
import os
from google import genai
from google.genai import types

# Initialize client (uses GEMINI_API_KEY environment variable)
client = genai.Client()

def run_sdd_pipeline(spec_content: str, task_brief: str):
    # Step 1: Run Clarifier
    print("--- Running Clarifier Agent ---")
    clarifier_instruction = (
        "You are a Spec Clarifier. Scan the provided spec for ambiguity, contradictions, "
        "and missing edge cases. List any blocking questions. If the spec is ready and "
        "has no ambiguities, reply with exactly: 'READY FOR IMPLEMENTATION'."
    )
    
    clarifier_response = client.models.generate_content(
        model='gemini-1.5-flash',
        contents=f"Spec:\n{spec_content}",
        config=types.GenerateContentConfig(
            system_instruction=clarifier_instruction,
            temperature=0.1
        )
    )
    
    print(clarifier_response.text)
    
    if "READY FOR IMPLEMENTATION" not in clarifier_response.text:
        print("\nPipeline stopped: Spec requires human clarification.")
        return
        
    # Step 2: Run Implementer
    print("\n--- Running Implementer Agent ---")
    implementer_instruction = (
        "You are an Implementer Agent. Generate clean, documented code and unit tests "
        "that satisfy the task brief and specification constraints. Do not modify files "
        "out of scope."
    )
    
    implementer_response = client.models.generate_content(
        model='gemini-1.5-flash',
        contents=f"Spec:\n{spec_content}\n\nTask Brief:\n{task_brief}",
        config=types.GenerateContentConfig(
            system_instruction=implementer_instruction,
            temperature=0.2
        )
    )
    print(implementer_response.text)

# Example execution
spec = "FR-001: Accept leads. Must normalize phone number. Must strip country codes."
brief = "TASK-004: Implement duplicate phone checking. Only touch src/duplicate.js."
run_sdd_pipeline(spec, brief)
```

---

## 2. OpenAI Swarm (Lightweight Orchestration)
OpenAI Swarm is an experimental, educational multi-agent framework. It uses two primitives—**Agents** and **Handoffs**—making it ideal for learning how agents delegate tasks to one another.

### The Pattern: Handing Off from Clarifier to Implementer
If the Clarifier Agent approves the specification, it calls a function to "hand off" the conversation to the Implementer Agent.

### Step-by-Step Setup
1. Install Swarm and OpenAI:
   ```bash
   pip install git+https://github.com/openai/swarm.git openai
   ```
2. Set your API key:
   ```bash
   export OPENAI_API_KEY="your-api-key"
   ```
3. Run this Python script:

```python
from swarm import Swarm, Agent

client = Swarm()

# Define handoff function
def transfer_to_implementer():
    print("[Handoff] Clarifier approved the spec. Transferring to Implementer...")
    return implementer_agent

# Define agents
clarifier_agent = Agent(
    name="Clarifier Agent",
    instructions=(
        "You are a Spec Clarifier. Review the requirements. If there is any ambiguity, "
        "ask the user for details. If the spec has clear acceptance criteria and no "
        "ambiguities, call the transfer_to_implementer function immediately."
    ),
    functions=[transfer_to_implementer],
)

implementer_agent = Agent(
    name="Implementer Agent",
    instructions=(
        "You are a developer agent. Generate code that satisfies the spec. Focus "
        "strictly on code within allowed boundaries."
    ),
)

# Start conversation loop
print("--- Starting Agent Swarm ---")
response = client.run(
    agent=clarifier_agent,
    messages=[{"role": "user", "content": "Spec: Add email uniqueness check. Only touch user.js. Let's go."}]
)

print("\nFinal Swarm Output:")
print(response.messages[-1]["content"])
```

---

## 3. Anthropic API (Architect-Implementer Pattern)
Anthropic's models (like Claude 3.5 Sonnet) excel at reasoning and complex coding. A common multi-agent design with Anthropic is the **Architect-Implementer** pattern, run using separate sessions to prevent context drift.

```text
Session A (Architect)                 Session B (Implementer)
Reads whole repo / spec draft          Reads task brief + allowed files only
Outputs detailed Technical Plan ---->  Writes code & executes local tests
Reviews final implementation   <----  Returns diff & verification results
```

### Scripted Dual-Session Loop
You can script this pattern using the `anthropic` SDK:

```python
# pip install anthropic
import os
from anthropic import Anthropic

client = Anthropic(api_key=os.environ.get("ANTHROPIC_API_KEY"))

def run_dual_session(spec_draft: str, task_id: str):
    # Session A: Architect designs technical plan
    print("--- Session A: Architect ---")
    architect_prompt = (
        f"Design a technical implementation plan for task {task_id} based on this spec:\n"
        f"{spec_draft}\nLink each decision to requirement IDs. Output plan in markdown."
    )
    
    plan_message = client.messages.create(
        model="claude-3-5-sonnet-latest",
        max_tokens=2000,
        messages=[{"role": "user", "content": architect_prompt}]
    )
    tech_plan = plan_message.content[0].text
    print(tech_plan)
    
    # Session B: Implementer receives the plan and acts under tight constraints
    print("\n--- Session B: Implementer ---")
    implementer_prompt = (
        f"Implement the following plan. Allowed scope: src/service.js only.\n"
        f"Technical Plan:\n{tech_plan}"
    )
    
    code_message = client.messages.create(
        model="claude-3-5-sonnet-latest",
        max_tokens=2000,
        system="You are an Implementer. Do not write code outside the allowed scope.",
        messages=[{"role": "user", "content": implementer_prompt}]
    )
    print(code_message.content[0].text)

run_dual_session("FR-101: Track user signups.", "TASK-01")
```

---

## 4. OpenHands (Autonomous Software Engineer)
[OpenHands](https://github.com/All-In-A-Day-Work/OpenHands) is a powerful, open-source agent workspace that runs in a local Docker container. It has access to a terminal, web browser, and workspace files, enabling it to act as an autonomous **Implementer** or **Reviewer**.

### Step-by-Step Setup & SDD Workflow

#### Step 1: Run OpenHands locally
Open a terminal in your project workspace directory and run the following command to spin up the OpenHands container, mounting your current workspace as the target directory:

```bash
docker run -it \
  --pull always \
  -e SANDBOX_USER_ID=$UID \
  -e WORKSPACE_BASE=$PWD \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v $PWD:/workspace \
  -p 3000:3000 \
  --name openhands-app \
  ghcr.io/all-in-a-day-work/openhands:latest
```

#### Step 2: Configure OpenHands
1. Open your browser and navigate to `http://localhost:3000`.
2. Set your LLM Provider (e.g., Anthropic, OpenAI, or Google Gemini via OpenRouter/Google AI Studio).
3. Set your Model (recommend: `claude-3-5-sonnet` or `gemini-1.5-pro` for coding).
4. Provide your API Key.

#### Step 3: Execute the SDD Task Brief
To run an SDD cycle inside OpenHands:
1. Paste the **Task Brief** (e.g., `agent-task-prompt.md` containing allowed files, constraints, and test commands) directly into the chat interface.
2. Type:
   > *"Read `agent-task-prompt.md` and the linked specifications under `docs/specs/`. Implement the requested task. Do not touch files outside the allowed scope. Run the validation tests before indicating completion."*
3. Watch the agent write code, create tests, and execute the shell commands inside the sandbox.
4. Review the final diff generated in the git tree within OpenHands or in your local IDE before merging.
