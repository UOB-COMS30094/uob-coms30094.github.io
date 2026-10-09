---
layout: default
title: Home
---

[Home](index) | [Blackboard Page](https://www.ole.bris.ac.uk/ultra/courses/_269234_1/outline) | [Blackboard Forum](https://www.ole.bris.ac.uk/ultra/courses/_269234_1/engagement) | [Unit Catalogue](https://www.bristol.ac.uk/unit-programme-catalogue/UnitDetails.jsa?ayrCode=26%2F27&unitCode=COMS30094)
<br/>
Labs: [W1](lab-w1) | [W2](lab-w2) | [W3](lab-w3) | [W4]() | [W5]() | [W7]() | [W8]()

# Welcome to Intelligent Agents Lab 3!

In this lab you will work with a small multi-agent **search-and-rescue** simulator.

The main programming task is small. The important part is understanding the coordination decisions being made.

You will explore:

- task dependencies;
- allocation of tasks to agents;
- coalition formation; and
- independent, centralised and decentralised coordination.

The simulator is already supplied. You only need to complete **three functions** near the top of `lab3.py`.

---

## Setup

Use the same Python environment as the previous labs.

Run the simulation:

```bash
python lab3.py
```

Useful controls:

- `M` — change coordination mode and reset
- `SPACE` — pause / resume
- `R` — reset
- `ESC` — quit

---

## Lab Files

[Download the Lab 3 starter files](REPLACE_WITH_STARTER_URL)

The main file is:

- `lab3.py` — agents, tasks, coordination and the Pygame visualisation.

You only need to edit three functions near the top of the file:

```python
task_ready(...)
choose_agent(...)
choose_coalition(...)
```

---

## Before You Start

There are four agents with different capabilities:

```text
A0: medic
A1: engineer, lift
A2: medic, lift
A3: transport
```

There is one simple rescue task and one rescue involving a sequence of dependent tasks:

```text
B1 Clear debris
      ↓
B2 Stabilise casualty
      ↓
B3 Extricate casualty   [2 agents]
      ↓
B4 Evacuate casualty
```

Start by looking at:

```python
Agent
Task
build_agents()
build_tasks()
```

These show the agent state, capabilities, task requirements and dependencies.

Then briefly look at:

```python
_update_ready()
_central_allocate()
_decentralised_allocate()
_execute()
```

These are supplied. You do **not** need to modify them, but they show where your functions are used.

---

## Task 1 — Task Dependencies

Complete:

```python
def task_ready(task, tasks):
```

A task should become ready only when **all of its predecessor tasks are `DONE`**.

For example, `B3` cannot begin until `B2` has finished.

A task with no predecessors should be ready immediately.

Useful code/data:

```python
task.predecessors
tasks[task_id]
all(...)
```

**Think about it:** Why might we prevent an agent from starting a task before its predecessor has completed?

---

## Task 2 — Allocate One Agent

Complete:

```python
def choose_agent(task, agents):
```

Choose an agent that:

1. is idle;
2. has all capabilities required by the task;
3. is closest to the task.

Useful code/data:

```python
agent.idle
agent.capabilities
task.roles
manhattan(...)
```

If two suitable agents have the same distance, choose the lower agent ID.

If no suitable agent is available, return `None`.

**Think about it:** Why is simply choosing the nearest agent not enough?

---

## Task 3 — Form a Coalition

Complete:

```python
def choose_coalition(task, agents):
```

Some tasks require **two agents acting together**.

For each possible group of idle agents:

1. combine their capabilities;
2. check that they cover the task requirements;
3. ensure each selected agent contributes at least one required capability;
4. calculate how long it takes for all agents to arrive.

Useful code/data:

```python
itertools.combinations
agent.capabilities
task.roles
task.required_agents
manhattan(...)
```

Use:

\[
cost(C,t)=\max_{a\in C} distance(a,t)
\]

The maximum is used because the task cannot start until the **last coalition member arrives**.

Choose the feasible coalition with the lowest cost. Break ties using agent IDs.

If no feasible coalition exists, return an empty list.

**Think about it:** Why might coalition cost use the maximum distance rather than the sum of distances?

---

## Compare the Coordination Modes

Once your functions work, press `M` to run the same scenario using:

```text
INDEPENDENT
CENTRALISED
DECENTRALISED
```

Compare:

- tasks completed;
- distance travelled;
- messages;
- mission completion time.

**Think about it:**

- Why can independent agents fail to complete the whole mission?
- What information does the centralised coordinator require?
- Why can decentralised coordination take longer?
- When do we need a coalition rather than ordinary task allocation?

---

## Bonus Tasks

### Bonus 1 — Repeated Allocation

Look at:

```python
step()
_central_allocate()
_execute()
```

Currently the centralised coordinator assigns at most **one ready task per simulation tick**.

Add another independent task in `build_tasks()` so that several tasks can be ready at once.

Then modify `_central_allocate()` so that it can allocate several tasks in the same tick while suitable idle agents are available.

**Question:** Does this reduce the mission completion time? Can assigning everything immediately ever be a bad idea?

### Bonus 2 — Which Task First?

The supplied coordinator considers ready tasks in task-ID order.

Add two ready tasks that require the same type of agent.

Try changing which task is allocated first, for example:

- nearest task first;
- shortest task first;
- dependency-unlocking task first.

**Question:** Does the ordering of allocations change the overall mission time?

---

## Lab Solutions

Lab 3 Solution (will be made available!)
