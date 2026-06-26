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

---

# Practical LangGraph Workflows

This section covers hands-on notebooks that demonstrate LangGraph concepts with real code.

---

## 1️⃣ BMI Calculator Workflow (`1_bmi_workflow.ipynb`)

**Goal:** Build a simple two-step graph that computes BMI and categorizes it.

### Code Pattern

```python
from langgraph.graph import StateGraph, START, END
from typing import TypedDict

# 1. Define state
class BMIState(TypedDict):
    weight_kg: float
    height_m: float
    bmi: float
    category: str

# 2. Define nodes (Python functions)
def calculate_bmi(state: BMIState) -> BMIState:
    bmi = state['weight_kg'] / (state['height_m'] ** 2)
    state['bmi'] = round(bmi, 2)
    return state

def label_bmi(state: BMIState) -> BMIState:
    bmi = state['bmi']
    if bmi < 18.5:       state['category'] = 'underweight'
    elif bmi < 25:       state['category'] = 'Normal'
    elif bmi < 30:       state['category'] = 'Overweight'
    else:                state['category'] = 'obese'
    return state

# 3. Build graph
graph = StateGraph(BMIState)
graph.add_node('calculate_bmi', calculate_bmi)
graph.add_node('label_bmi', label_bmi)
graph.add_edge(START, 'calculate_bmi')
graph.add_edge('calculate_bmi', 'label_bmi')
graph.add_edge('label_bmi', END)

# 4. Compile
workflow = graph.compile()

# 5. Invoke
result = workflow.invoke({'weight_kg': 80, 'height_m': 1.73})
print(result)
# Output: {'weight_kg': 80, 'height_m': 1.73, 'bmi': 26.73, 'category': 'Overweight'}
```

### Key Takeaways

| Concept | What it teaches |
|---------|----------------|
| **State** | State is a `TypedDict` — all nodes share and update it |
| **Node** | Any Python function that receives state and returns state |
| **Edge** | Directs flow: `START → node1 → node2 → END` |
| **Invoke** | `workflow.invoke(initial_state)` runs the graph and returns final state |
| **Return** | A node **must return state** — otherwise updates are lost |

**⚠️ Common Bug:** Forgetting `return state` in a node — the state changes won't be applied.

---

## 2️⃣ Simple LLM Q&A Workflow (`2_simple_llm_workflow.ipynb`)

**Goal:** Integrate an LLM (ChatOpenAI) into a LangGraph workflow.

### Code Pattern

```python
from langgraph.graph import StateGraph, START, END
from langchain_openai import ChatOpenAI
from typing import TypedDict
from dotenv import load_dotenv

load_dotenv()
model = ChatOpenAI()

class LLMstate(TypedDict):
    question: str
    answer: str

def llm_qa(state: LLMstate) -> LLMstate:
    question = state['question']
    prompt = f'Answer to the following question {question}'
    answer = model.invoke(prompt).content
    state['answer'] = answer
    return state

graph = StateGraph(LLMstate)
graph.add_node('llm_qa', llm_qa)
graph.add_edge(START, 'llm_qa')
graph.add_edge('llm_qa', END)
workflow = graph.compile()

result = workflow.invoke({'question': 'what is the distance between earth and moon?'})
print(result)
# Output: {'question': '...', 'answer': 'The average distance ... 384,400 km ...'}
```

### Key Takeaways

| Concept | What it teaches |
|---------|----------------|
| **LLM in a node** | Call `model.invoke(prompt)` inside a node function |
| **State → LLM → State** | Extract data from state → call LLM → store result back in state |
| **Minimal graph** | Even a single-node graph is a valid LangGraph workflow |
| **dotenv** | Use `.env` files to manage API keys securely |

**Interview Tip:** This is the simplest LangGraph pattern — one LLM call inside one node. It shows how LangGraph wraps any Python logic (including LLM calls) into a graph node.

---

## 3️⃣ Prompt Chaining — Blog Generator (`3_prompt_chaining.ipynb`)

**Goal:** Chain two LLM calls — first generates an outline, then writes a full blog using that outline.

### Code Pattern

```python
class BlogState(TypedDict):
    title: str
    outline: str
    content: str

def create_outline(state: BlogState) -> BlogState:
    prompt = f"Generate a detailed outline for a blog on - {state['title']}"
    state['outline'] = model.invoke(prompt).content
    return state

def create_blog(state: BlogState) -> BlogState:
    prompt = f"Write a blog on '{state['title']}' using outline:\n{state['outline']}"
    state['content'] = model.invoke(prompt).content
    return state

# Graph: START → create_outline → create_blog → END
```

### Key Takeaways

| Concept | What it teaches |
|---------|----------------|
| **Prompt Chaining** | Output of one LLM call becomes input to the next |
| **Multi-node LLM** | Each node can make its own LLM call |
| **State as bridge** | `outline` is written by node 1, read by node 2 — state connects them |
| **Real use case** | Content generation pipelines (outline → draft → edit → final) |

### Prompt Chaining Pattern (General)

```
Input → LLM Call 1 → Intermediate Result → LLM Call 2 → Final Output
                     ↑                            ↓
                  State Key 1                State Key 2
```

Prompt chaining is ideal when:
- Each step needs a **different prompt**
- Later steps depend on earlier outputs
- You want to **separate concerns** (e.g., planning vs writing)

---

## 4️⃣ Batsman Stats Calculator (`4_batsman_workflow.ipynb`)

**Goal:** Compute cricket stats (strike rate, balls per boundary, boundary %) in parallel, then summarize.

### Code Pattern

```python
class BatsmanState(TypedDict):
    runs: int
    balls: int
    fours: int
    sixes: int
    sr: float
    bpb: float
    boundary_percent: float
    summary: str

def calculate_sr(state: BatsmanState):
    return {"sr": (state['runs'] / state['balls']) * 100}

def calculate_bpb(state: BatsmanState):
    return {"bpb": state['balls'] / (state['fours'] + state['sixes'])}

def calculate_boundary_percent(state: BatsmanState):
    bp = ((state['fours']*4) + (state['sixes']*6)) / state['runs'] * 100
    return {"boundary_percent": bp}

def summary(state: BatsmanState):
    s = f"SR: {state['sr']}, BPB: {state['bpb']}, Bound%: {state['boundary_percent']}"
    return {"summary": s}

graph = StateGraph(BatsmanState)
graph.add_node('calculate_sr', calculate_sr)
graph.add_node('calculate_bpb', calculate_bpb)
graph.add_node('calculate_boundary_percent', calculate_boundary_percent)
graph.add_node('summary', summary)

graph.add_edge(START, 'calculate_sr')
graph.add_edge(START, 'calculate_bpb')
graph.add_edge(START, 'calculate_boundary_percent')
graph.add_edge('calculate_sr', 'summary')
graph.add_edge('calculate_bpb', 'summary')
graph.add_edge('calculate_boundary_percent', 'summary')
graph.add_edge('summary', END)

workflow = graph.compile()
result = workflow.invoke({'runs': 100, 'balls': 50, 'fours': 6, 'sixes': 4})
```

### Key Takeaways

| Concept | What it teaches |
|---------|----------------|
| **Partial update** | Nodes return only the keys they changed — `return {"sr": sr}` |
| **Parallel execution** | Nodes from `START` run simultaneously (fan-out) |
| **Fan-in** | `summary` waits for ALL 3 calculators to finish |
| **No reducer needed** | Each parallel node writes a unique key — no merge conflicts |

**⚠️ Common Bug:** Returning a set `{"key", value}` instead of dict `{"key": value}` — LangGraph expects a dict.

---

## 5️⃣ Essay Evaluation Workflow (`5_essay_evaluation.ipynb`)

**Goal:** Evaluate an essay across 3 criteria (language, analysis, clarity) using parallel LLM calls, then aggregate scores.

### Code Pattern

```python
from pydantic import BaseModel, Field

class EvaluationSchema(BaseModel):
    feedback: str = Field(description="Detailed feedback")
    score: int = Field(description="Score out of 10", ge=0, le=10)

structure_model = model.with_structured_output(EvaluationSchema)

class EssayState(TypedDict):
    essay: str
    language_feedback: str
    analysis_feedback: str
    clarity_feedback: str
    language_score: float
    analysis_score: float
    clarity_score: float
    overall_feedback: str
    indivisual_scores: list[int]
    avg_score: float

def evaluate_language(state: EssayState):
    prompt = f"Evaluate language quality... {state['essay']}"
    output = structure_model.invoke(prompt)
    return {"language_feedback": output.feedback, "language_score": output.score}

# evaluate_analysis, evaluate_thought — same pattern

def final_evaluation(state: EssayState):
    scores = [state['language_score'], state['analysis_score'], state['clarity_score']]
    return {"indivisual_scores": scores, "avg_score": sum(scores) / len(scores)}

# Graph: START → (3 parallel evaluators) → final_evaluation → END
```

### Key Takeaways

| Concept | What it teaches |
|---------|----------------|
| **Structured output** | `model.with_structured_output(Schema)` returns typed objects |
| **Parallel LLM calls** | Each evaluator calls the LLM independently |
| **Per-node score keys** | Each parallel node writes its own key — avoids reducer race conditions |
| **Post-hoc aggregation** | Final node builds the list from individual scores |

**⚠️ Common Bug:** f-string quote clash — `f'...{state['key']}...'` breaks. Use `f"...{state['key']}..."` instead.

---

## 6️⃣ Conditional Workflow — Quadratic Solver (`6_conditional_workflow.ipynb`)

**Goal:** Solve a quadratic equation by computing the discriminant and routing to the correct result node based on its value.

### Code Pattern

```python
class QuadState(TypedDict):
    a: int; b: int; c: int
    equation: str; discriminant: str; result: str

def show_equation(state: QuadState):
    equation = f"{state['a']}x^2 + {state['b']}x + {state['c']}"
    return {'equation': equation}

def calculate_discriminant(state: QuadState):
    discriminant = state['b']**2 - (4*state['a']*state['c'])
    return {'discriminant': discriminant}

def check_condition(state: QuadState) -> Literal["real_roots","repeated_roots","no_real_roots"]:
    if state['discriminant'] > 0:      return "real_roots"
    elif state["discriminant"] == 0:   return "repeated_roots"
    else:                              return "no_real_roots"

# Graph: START → show_equation → calculate_discriminant
#         → (conditional) → real_roots / repeated_roots / no_real_roots → END

graph.add_conditional_edges('calculate_discriminant', check_condition)
```

### Workflow Diagram

```mermaid
flowchart LR
    START([START]) --> show_equation[show_equation]
    show_equation --> calculate_discriminant[calculate_discriminant]
    calculate_discriminant --> check_condition{check_condition}
    check_condition -- "D > 0" --> real_roots[real_roots]
    check_condition -- "D == 0" --> repeated_roots[repeated_roots]
    check_condition -- "D < 0" --> no_real_roots[no_real_roots]
    real_roots --> END([END])
    repeated_roots --> END
    no_real_roots --> END
```

### Key Takeaways

| Concept | What it teaches |
|---------|----------------|
| **Conditional edges** | `add_conditional_edges(node, router_fn)` — the router function returns the **name of the next node** |
| **Router return type** | Router must return a `Literal["node1", "node2", ...]` — this tells LangGraph which edge to follow |
| **Branching logic** | A single node can fan out to multiple destination nodes based on data |
| **No edge to END** | The conditional edges replace `add_edge` — each route goes directly to its own node |

**⚠️ Common Bug:** The router function returns a **string** (node name), not a boolean or state update. Mistyped node names cause `ValueError`.

---

## 7️⃣ Review Reply Workflow (`7_review_reply_workflow.ipynb`)

**Goal:** Classify a product review's sentiment (positive/negative), then either send a thank-you note or diagnose the issue and craft a resolution message.

### Code Pattern

```python
class SentimentSchema(BaseModel):
    sentiment: Literal["positive", "negative"] = Field(description="Sentiment of the review")

class DiagnosisSchema(BaseModel):
    issue_type: Literal["UX","Performance","Bug","Support","Other"]
    tone: Literal["angry","frustrated","disappointed","calm"]
    urgency: Literal["low","medium","high"]

structure_model = model.with_structured_output(SentimentSchema)
structure_model2 = model.with_structured_output(DiagnosisSchema)

def find_sentiment(state: ReviewState):
    prompt = f"For the following review find out the sentiment \n {state['review']}"
    sentiment = structure_model.invoke(prompt).sentiment
    return {"sentiment": sentiment}

def check_sentiment(state: ReviewState) -> Literal["positive_response", "run_diagnosis"]:
    if state['sentiment'] == "positive": return "positive_response"
    else:                                return "run_diagnosis"

# Positive path: find_sentiment → positive_response → END
# Negative path: find_sentiment → run_diagnosis → negative_response → END

graph.add_conditional_edges("find_sentiment", check_sentiment)
```

### Workflow Diagram

```mermaid
flowchart LR
    START([START]) --> find_sentiment[find_sentiment]
    find_sentiment --> check_sentiment{check_sentiment}
    check_sentiment -- "positive" --> positive_response[positive_response]
    check_sentiment -- "negative" --> run_diagnosis[run_diagnosis]
    positive_response --> END([END])
    run_diagnosis --> negative_response[negative_response]
    negative_response --> END
```

### Key Takeaways

| Concept | What it teaches |
|---------|----------------|
| **Sentiment routing** | Use an LLM + structured output to classify input, then route with conditional edges |
| **Two schema pattern** | One schema for classification (`SentimentSchema`), another for deeper analysis (`DiagnosisSchema`) |
| **Chained analysis** | Negative reviews go through diagnosis → response generation (2-step chain) |
| **Struct → dict** | `response.model_dump()` converts Pydantic output to a dict for state storage |
| **Multi-path graph** | A single conditional edge sends positive and negative reviews down completely different paths |

**Interview Tip:** This is a classic **real-world customer support workflow** — classify → route → specialized handler. It combines conditional edges, structured output, and prompt chaining in one graph.

---

## 8️⃣ Post Generator — Evaluator-Optimizer (`8_post_generator.ipynb`)

**Goal:** Generate a LinkedIn post on a topic, evaluate it with strict criteria, and iteratively improve it until approved or max iterations reached.

### Code Pattern

```python
class PostEvaluation(BaseModel):
    evaluation: Literal['approved', 'needs_improvement']
    feedback: str

structured_evaluator_llm = evaluator_llm.with_structured_output(PostEvaluation)

class PostState(TypedDict):
    topic: str
    tweet: str
    evaluation: Literal["approved", "needs_improvement"]
    feedback: str
    iteration: int
    max_iteration: int

def generate_Linkedin_Post(state: PostState):
    messages = [SystemMessage(content="..."), HumanMessage(content=f"...{state['topic']}...")]
    response = generator_llm.invoke(messages).content
    return {"tweet": response}

def evaluate_post(state: PostState):
    response = structured_evaluator_llm.invoke(messages)
    return {'evaluation': response.evaluation, "feedback": response.feedback}

def optimised_post(state: PostState):
    response = optimizer_llm.invoke(messages).content
    return {"tweet": response, "iteration": state['iteration'] + 1}

def route_evaluation(state: PostState) -> Literal["approved", "needs_improvement"]:
    if state['evaluation'] == "approved" or state['iteration'] >= state['max_iteration']:
        return "approved"
    return "needs_improvement"

# Graph: START → generate → evaluate
#         → (conditional) → END (if approved) / optimize → evaluate (loop)
graph = StateGraph(PostState)
graph.add_node("generate", generate_Linkedin_Post)
graph.add_node("evaluate", evaluate_post)
graph.add_node("optimize", optimised_post)
graph.add_edge(START, "generate")
graph.add_edge("generate", "evaluate")
graph.add_conditional_edges("evaluate", route_evaluation, {
    'approved': END, 'needs_improvement': "optimize"
})
graph.add_edge("optimize", "evaluate")
workflow = graph.compile()
```

### Workflow Diagram

```mermaid
flowchart LR
    START([START]) --> generate[generate]
    generate --> evaluate[evaluate]
    evaluate --> route{route_evaluation}
    route -- "approved" --> END([END])
    route -- "needs_improvement" --> optimize[optimize]
    optimize --> evaluate
```

### Key Takeaways

| Concept | What it teaches |
|---------|----------------|
| **Evaluator-Optimizer** | Classic loop pattern: Generate → Evaluate → Improve → Re-evaluate |
| **Loop with guard** | `optimize → evaluate` is a cycle; the router breaks it on approval or max iteration |
| **Three LLMs** | Generator, evaluator, and optimizer can be separate models or the same |
| **Structured evaluation** | `with_structured_output(PostEvaluation)` gives typed {evaluation, feedback} |
| **Iteration counter** | `iteration` tracks attempts; `max_iteration` prevents infinite loops |
| **Router as guard** | The router checks both quality (`approved`) and budget (`iteration >= max_iteration`) |
| **Loop-back edge** | `add_edge("optimize", "evaluate")` creates a feedback loop in the graph |

**⚠️ Common Bug:** The `route_evaluation` guard must check `>= max_iteration` (not `==`) to handle edge cases where iteration gets incremented past the limit.

**Interview Tip:** This is the **Evaluator-Optimizer** workflow pattern from the [workflow types](#common-types-of-workflows) section — implemented as a graph with a loop-back edge. It's the most practical pattern for production AI systems where quality control via iterative refinement is needed.

---

## 📊 Workflow Comparison Table

| Feature | BMI Workflow | LLM Q&A | Blog Generator |
|---------|-------------|---------|----------------|
| **Nodes** | 2 (calc + label) | 1 (LLM) | 2 (outline + blog) |
| **LLM used?** | No | Yes | Yes (2 calls) |
| **Pattern** | Sequential logic | Single LLM call | Prompt chaining |
| **State keys** | 4 | 2 | 3 |
| **Key lesson** | Nodes must return state | LLM integration | Chaining outputs |

---

## 🚨 Common Mistakes & Fixes

| Mistake | Why it fails | Fix |
|---------|-------------|-----|
| Forgetting `return state` | Node updates are silently dropped | Always `return state` at end of node |
| Missing state keys | `KeyError` on access | Define all keys in TypedDict |
| Not calling `compile()` | Cannot invoke the graph | Call `workflow = graph.compile()` |
| Wrong edge names | `ValueError` — node not found | Edge names must match `add_node` names |
| Forgetting `load_dotenv()` | LLM auth fails | Call `load_dotenv()` before creating `ChatOpenAI` |

---

## 💡 Interview Quick Notes

- **LangGraph flow:** Define State → Create Nodes → Add Edges → Compile → Invoke
- **Node = Python function** that takes state, does work, returns state
- **State = TypedDict** shared across all nodes
- **Graph execution:** Runs nodes in edge order, passing state through
- **Prompt chaining =** LLM output feeds into next LLM's prompt via state
- **LLM in LangGraph =** Just call `model.invoke()` inside a node — nothing special needed
- **Why LangGraph over LangChain for these?** Even simple workflows benefit from built-in state management vs manual variable passing

---

## 9️⃣ Chatbot Workflow (`9_chatbot.ipynb`)

**Goal:** Build a conversational chatbot that maintains message history across turns using LangGraph state.

### Code Pattern

```python
from langgraph.graph.message import add_messages
from langgraph.checkpoint.memory import MemorySaver

class ChatState(TypedDict):
    messages: Annotated[list[BaseMessage], add_messages]

llm = ChatOpenAI()

def chat_node(state: ChatState):
    messages = state['messages']
    response = llm.invoke(messages)
    return {'messages': [response]}

checkpointer = MemorySaver()
graph = StateGraph(ChatState)
graph.add_node('chat_node', chat_node)
graph.add_edge(START, "chat_node")
graph.add_edge("chat_node", END)

chatbot = graph.compile(checkpointer=checkpointer)

# Invoke with thread_id for conversation tracking
config = {"configurable": {"thread_id": "1"}}
chatbot.invoke({"messages": [HumanMessage(content="hi")]}, config=config)
```

### Key Takeaways

| Concept | What it teaches |
|---------|----------------|
| **add_messages reducer** | `Annotated[list[BaseMessage], add_messages]` — this reducer appends new messages to the existing list automatically |
| **MemorySaver** | In-memory checkpointer that saves state at every super step — keeps conversation history alive within a thread |
| **Thread ID** | `configurable: {"thread_id": "1"}` — each conversation gets a unique thread ID so the checkpointer knows which history to load |
| **LLM with history** | Pass the full `messages` list to `llm.invoke()` — the LLM sees the entire conversation context |
| **Conversation loop** | `while True` loop with `input()` — on each turn, invoke with the same thread_id so the checkpointer appends new messages to existing history |

### How State Grows

```
Turn 1: [HumanMessage("hi")]
         ↓
         LLM responds → state['messages'] = [Human("hi"), AIMessage("Hello!")]
         ↓
Turn 2: [HumanMessage("what is langchain?")]
         ↓
         checkpointer loads history → invokes LLM with [Human("hi"), AI("Hello!"), Human("what is langchain?")]
         ↓
         state['messages'] = [Human("hi"), AI("Hello!"), Human("what is langchain?"), AIMessage("...")]
```

**⚠️ Common Bug:** Forgetting to pass `config` with `thread_id` — without it, every invoke starts a fresh conversation with no history.

**Interview Tip:** This is the foundation of **stateful chatbots**. The `add_messages` reducer is what makes conversation history automatic. Without it, each turn would overwrite the previous messages. The `MemorySaver` + `thread_id` combo is what enables multi-turn conversations — the checkpointer persists state between invocations.

---

## 10️⃣ Persistence (`10_persistence.ipynb`)

**Goal:** Understand LangGraph's persistence layer — how checkpoints and threads enable fault tolerance, conversation history, human-in-the-loop, and time travel.

### Core Concepts

| Concept | What it does |
|---------|-------------|
| **Checkpointer** | Saves the entire state at every **super step** to a database. If the workflow crashes, it resumes from the last saved checkpoint — not from the beginning |
| **Thread ID** | A unique identifier assigned to a workflow run. All checkpoint data is stored under this ID. When resuming, you supply the same thread_id to load the correct state |
| **Super Step** | LangGraph's execution unit (one or more nodes running in parallel). After each super step, the checkpointer persists the current state |

### Benefits of Persistence

| Benefit | What it enables |
|---------|----------------|
| **Short-term memory** | Messages from previous turns are saved and reloaded — the chatbot remembers the conversation (shown in `9_chatbot.ipynb` with `MemorySaver`) |
| **Fault tolerance** | If the workflow crashes mid-execution, next run with the same thread_id resumes from the last checkpoint — no data loss and no restart |
| **Human in the Loop** | The workflow pauses and waits for human approval; the state is saved at that point. After approval, execution continues from the same saved state |
| **Time travel** | You can re-run the workflow from any past checkpoint using the thread_id and checkpoint ID — useful for debugging and replay |

### Checkpointer Flow

```
Super Step 1 → Checkpointer saves state → Super Step 2 → Checkpointer saves state
                                                              ↓
                                                    (crash happens here)
                                                              ↓
                                                    Resume with same thread_id
                                                              ↓
                                                    Checkpointer loads last saved state
                                                              ↓
                                                    Continues from Super Step 2 onward
```

**Interview Tip:** Persistence is what makes LangGraph production-ready. Four things it enables — short-term memory (conversation history), fault tolerance (crash recovery), human-in-the-loop (pause/resume), and time travel (replay from any point). The checkpointer + thread_id pattern is the backbone of all four.
