# Agentic AI

> A complete guide to understanding Agentic AI — perfect for interview preparation.

---

## What is Agentic AI?

Agentic AI refers to AI systems that can **act on their own** to achieve goals. Unlike simple AI that just responds to inputs, Agentic AI can make decisions, use tools, plan ahead, and learn from feedback.

---

## Key Characteristics

### 1. Autonomous

The agent can make decisions and take actions on its own. It is **proactive** (doesn't wait for instructions).

**How autonomy is controlled:**
- Permission scope (what it is allowed to do)
- Human in the loop (HITL) — a human approves important actions
- Override controls (a human can stop or change its action)
- Guardrails (rules it cannot break)

### 2. Goal Oriented

The agent continuously works toward a specific goal.

**Key points:**
- Goals are stored in **core memory**
- Goals can be changed later if needed
- The agent works independently to achieve the goal within given limits

### 3. Planning

Breaking a big goal into smaller steps or subgoals.

**Three steps of planning:**
1. **Generate** multiple candidate plans
2. **Evaluate** each plan based on — efficiency, tool availability, cost, risk, and alignment with rules
3. **Select** the best plan (often with human in the loop)

### 4. Reasoning

The thinking process where the agent understands information, draws conclusions, and makes decisions.

**Reasoning during planning:**
- Breaking the goal into smaller pieces
- Choosing which tool to use
- Estimating resources needed

**Reasoning during execution:**
- Making real-time decisions
- Handling human in the loop requests
- Handling errors

### 5. Adaptability

The agent can change its plans or actions when something unexpected happens, while still staying aligned with the goal.

**When adaptability is needed:**
- When a step **fails**
- When the agent gets **external feedback**
- When the **goal changes**

### 6. Context Awareness

The agent makes better decisions by understanding the full situation across multiple steps.

**Types of context:**
- The original goal
- Progress made so far
- The current environment state
- Tool responses
- User preferences
- Policies or guardrails

**Context is managed through:**
- **Short term memory** — remembers recent steps
- **Long term memory** — remembers past experiences and learnings

---

## Components of Agentic AI

### 1. Brain

The core thinking part of the agent. It handles:
- Understanding the goal
- Planning
- Reasoning
- Choosing the right tool
- Communicating with users and other systems

### 2. Orchestrator

The manager that controls the flow of work. It handles:
- Deciding the order of tasks
- Routing to different paths based on conditions
- Retrying failed steps
- Looping and repeating steps when needed
- Delegating work to other agents or systems

### 3. Tools

Ways for the agent to interact with the outside world:
- **External actions** — like sending an email, calling an API, or updating a database
- **Knowledge base access** — searching documents or databases for information

### 4. Memory

How the agent stores and recalls information:
- **Short term memory** — current session or task info
- **Long term memory** — past learnings and experiences
- **State tracking** — keeping track of where it is in the current task

### 5. Supervisor

The safety and control layer. It handles:
- Asking for human approval (Human in the Loop)
- Enforcing guardrails and rules
- Escalating edge cases to a human when the agent is stuck

---

## Summary Table

| Component | What it does |
|-----------|-------------|
| **Brain** | Thinks, plans, reasons, selects tools |
| **Orchestrator** | Manages task flow, retries, routing |
| **Tools** | Connects to external systems and data |
| **Memory** | Stores short-term and long-term info |
| **Supervisor** | Enforces safety, asks humans for help |

---

## Interview Tips

- **Autonomy vs Safety** — Be ready to explain how autonomy is balanced with guardrails and human oversight.
- **Planning vs Execution** — Understand the difference: planning is before action, reasoning happens during both.
- **Memory types** — Short term = like a notebook for current task. Long term = like a diary of past experiences.
- **Real-world example** — Think of an AI customer support bot: it plans how to solve your issue (planning), checks your order history (tool), remembers what you said earlier (memory), and asks a human if it cannot help (supervisor).


---

# LangChain vs LangGraph

## What is LangChain?

LangChain is an open-source library for building LLM-based applications. It provides **modular building blocks** that make it easy to create LLM workflows.

**Main components of LangChain:**
| Component | What it does |
|-----------|-------------|
| **Model** | Gives a unified interface to interact with different LLM providers |
| **Prompts** | Helps you engineer and manage prompts |
| **Retrievers** | Fetches relevant documents from a vector store |

The biggest feature of LangChain is **Chains** — connecting steps in a sequence.

---

## Example: Automated Hiring Workflow

Imagine we are hiring a **Backend Engineer**. The workflow looks like this:

```
Start
  → Hiring Request
  → Create JD
  → JD Approved (Human)
  → Post JD on LinkedIn / other platforms
  → Wait 7 days
  → Monitor Applications (threshold = 20 applications)
      → If NO → Modify JD → Wait 48 hrs → loop back
      → If YES → Shortlist
          → Schedule Interview
          → Conduct Interview
              → If Selected → Send Offer Letter
                  → If Accepted → Onboarding
                  → If Rejected → Renegotiate
              → If Not Selected → Send Regret Email
```

---

## Challenges: LangChain vs LangGraph

If we build this workflow with **LangChain**, we face several problems. Here is how **LangGraph** solves each one.

### Challenge 1: Control Flow Complexity

| | LangChain | LangGraph |
|---|-----------|-----------|
| **Problem** | For big workflows, you need a lot of **glue code** for conditions, loops, and jumps | No glue code needed |
| **How it works** | You write custom Python to handle branching and looping | First create **nodes**, then draw **edges** between them. The graph handles the flow automatically |

### Challenge 2: State Handling

| | LangChain | LangGraph |
|---|-----------|-----------|
| **Problem** | You must manually create a document and **manually change key-value pairs** to track state | **Stateful by design** |
| **How it works** | Lots of manual work to keep track of progress | When creating the graph, you define a state object. Every node receives the state, processes it, and returns the updated state |

### Challenge 3: Event-Driven Execution

| | LangChain | LangGraph |
|---|-----------|-----------|
| **Execution model** | **Sequential** — steps run one after another in a fixed order | **Event-driven** — nodes run when their conditions are met |

### Challenge 4: Fault Tolerance

| | LangChain | LangGraph |
|---|-----------|-----------|
| **Problem** | If a long-running workflow breaks, it **starts from the beginning** | Uses a **checkpointer** to save progress. If a fault happens, it **resumes from where it failed** |
| **Recovery** | Not possible | Easy — just resume from the saved state |

### Challenge 5: Human in the Loop

| | LangChain | LangGraph |
|---|-----------|-----------|
| **Problem** | You need to **split into two chains** — one before human approval and one after | Progress is **saved in state**. After human approval, it **continues from the same point** |
| **Flow** | Chain 1 → break → human approves → Chain 2 | Node runs → waits for human → continues from saved state |

### Challenge 6: Nested Workflows (Sub-graphs)

| | LangChain | LangGraph |
|---|-----------|-----------|
| **Concept** | Hard to compose workflows inside workflows | A node in a graph can be **another graph** (sub-graph) |
| **Use case** | Difficult to build multi-agent systems | Great for **multi-agent systems**. Makes graphs **reusable** |

### Challenge 7: Observability

| | LangChain | LangGraph |
|---|-----------|-----------|
| **Tool** | Both use **LangSmith** | Both use **LangSmith** |
| **Problem** | LangSmith cannot understand glue code, so debugging is hard | No glue code → LangSmith understands everything. It gives a **complete timeline** of the workflow |
| **Debugging** | Difficult | Easy |

---

## Quick Comparison Table

| Feature | LangChain | LangGraph |
|---------|-----------|-----------|
| Control flow | Needs glue code | Graph-based (nodes + edges) |
| State management | Manual | Built-in (stateful) |
| Execution | Sequential | Event-driven |
| Fault tolerance | Restarts from beginning | Resumes from checkpoint |
| Human in the loop | Split into multiple chains | Pause and resume from same point |
| Nested workflows | Hard to compose | Sub-graphs supported |
| Observability | Hard to debug (glue code) | Easy (no glue code) |
| Best for | Simple linear workflows | Complex, branching, long-running workflows |

---

## Interview Tips (LangChain vs LangGraph)

- **The key difference** — LangChain is **chain-based** (linear steps). LangGraph is **graph-based** (nodes + edges, supports loops and branches).
- **When to use what** — Use LangChain for simple Q&A or basic RAG. Use LangGraph for complex multi-step workflows, multi-agent systems, and long-running processes with human approval.
- **State is the game changer** — In interviews, highlight that LangGraph's built-in state + checkpointing is what makes fault tolerance and HITL possible.
- **Sub-graphs** — Mention that sub-graphs make LangGraph great for building **reusable, modular** AI systems.


---

# LangGraph Core Concepts

LangGraph is an **orchestration framework** for building intelligent, stateful, multi-step LLM workflows. It provides advanced features like parallelism, loops, branching, memory, and resumability.

---

## LLM Workflows

A workflow is a **series of tasks** designed to achieve a goal or build a complex application. Each task can be — prompting, reasoning, tool calling, memory access, or decision making.

Workflows can be **linear**, **parallel**, or **looped**.

### Common Types of Workflows

#### 1. Prompt Chaining

Call the LLM **multiple times in a sequence**, where the output of one prompt becomes the input of the next.

#### 2. Routing

Call an LLM **router** to decide which specialist agent or tool can best handle a given task.

```
Input → Router LLM → Checks which agent is capable → Routes to that agent
```

#### 3. Parallelization

Break a task into **multiple sub-tasks**, run them all at the same time, then combine the results with an **aggregator**.

**Example:** Content moderation — multiple models check different things (text, image, video) → aggregator makes the final decision

#### 4. Orchestrator Workers

Similar to parallelization, but the sub-tasks are **not pre-defined**. The orchestrator decides which worker to assign based on the input.

```
Input → Orchestrator → Decides which workers to use → Workers execute → Aggregate results → Output
```

#### 5. Evaluator-Optimizer

A loop where the LLM generates a solution, then another LLM (or the same one) evaluates it.

```
Generate → Evaluate → If rejected → Generate again (loop) → If accepted → Output
```

---

## Graph, Nodes, and Edges

The core idea of LangGraph is to **convert an LLM workflow into a graph**.

| Term | Meaning |
|------|---------|
| **Graph** | The entire workflow |
| **Node** | A single step or task in the workflow (e.g., call LLM, call a tool, make a decision) |
| **Edge** | The connection between nodes — defines the flow (which node runs next) |

---

## State

**State** is the **shared memory** that flows through your entire workflow.

- State is **shared** between all nodes
- State is **mutable** (can be changed)
- State is a special type of dictionary called **TypedDict** (you define the structure)

Every node receives the current state, does its work, and returns an **updated state**.

---

## Reducers

A **reducer** tells the system **how to update the state** when a node returns new data.

- Each key in the state can have its **own reducer**
- Reducers define the update logic (e.g., add to a list, overwrite a value, merge dictionaries)

**Simple example:**
```
State = {"messages": []}
Reducer for "messages" = add new message to the list (append)
Every time a node runs, it adds its message → state keeps growing
```

---

## LangGraph Execution Model

LangGraph runs a workflow in **6 steps**:

```
1. Graph Definition
   ↓
2. Compilation
   ↓
3. Invocation
   ↓
4. Super Step Begins
   ↓
5. Message Passing & Node Activation
   ↓
6. Halting Condition
```

| Step | What happens |
|------|-------------|
| **1. Graph Definition** | Define the state, nodes, and edges |
| **2. Compilation** | Call the `compile()` function to prepare the graph |
| **3. Invocation** | Pass the initial state to start execution |
| **4. Super Step Begins** | Run multiple nodes in parallel where possible |
| **5. Message Passing** | Nodes send messages and activate the next nodes |
| **6. Halting Condition** | Stop when the goal is reached or a stop condition is met |

These 6 steps happen **automatically** — you just define the graph and LangGraph handles the execution.

---

## Interview Tips (LangGraph Core Concepts)

- **State is the heart of LangGraph** — Everything revolves around state. Nodes read state, modify it, and pass it forward.
- **Reducers = how state changes** — Be ready to explain that reducers control how each key in state gets updated (append, overwrite, etc.).
- **Graph vs Chain** — LangGraph = graph (nodes + edges, flexible). LangChain = chain (linear, rigid). This is the most common interview question.
- **Super step** — LangGraph is not purely sequential. It can run multiple nodes in parallel in a single super step.
