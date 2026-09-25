---
layout: default
title: Home
---

[Home](index) \| [Blackboard Page](https://www.ole.bris.ac.uk/ultra/courses/_269234_1/outline) \| [Blackboard Forum](https://www.ole.bris.ac.uk/ultra/courses/_269234_1/engagement) \| [Unit Catalogue](https://www.bristol.ac.uk/unit-programme-catalogue/UnitDetails.jsa?ayrCode=26%2F27&unitCode=COMS30094)
<br/>
Labs: [W1](lab-w1) | [W2]() | [W3]() | [W4]() | [W5]() | [W7]() | [W8]()

# Welcome to Intelligent Agents Lab 1!

In this lab, you will work with an agent situated in a small grid-world environment. The agent must move between locations and, later, make decisions about when and where to drop litter.

As you work on the code, also think about what the implementation tells us about:

* the relationship between an **agent** and its **environment**;
* how an agent chooses actions;
* how **search algorithms** such as BFS, DFS and A* can be used for navigation;
* the difference between an agent's **navigation algorithm** and its **policy**;
* different properties of an agent's environment; and
* different ways in which an intelligent agent can be characterised.

By the end of the lab, you should be able to relate the behaviour you observe in the code to concepts introduced in the lectures.

## Setup

1. Install Anaconda if you do not have it already: https://www.anaconda.com/download
2. Create a new conda environment with `conda create -n agents_env python=3.11`
3. Activate your conda environment with `conda activate agents_env`
4. Install the requirements with `python -m pip install -r requirements.txt`

> **Note:** To deactivate your conda environment, run `conda deactivate`.
> To delete the environment, run `conda remove -n agents_env -a`.

## Lab Files

The lab contains the following key files:

* `Lab1.ipynb`
  The Jupyter notebook containing the lab exercises.

* `empty_world.py`
  Defines the basic grid-world environment.

* `agent.py`
  Defines the basic navigation agent.

* `litter_world.py`
  Extends the basic grid world with litter and cells that can become blocked.

* `litter_agent.py`
  Extends the basic agent with behaviour for interacting with litter.

You should complete the exercises in `Lab1.ipynb`.

Where the notebook contains:

```python
# CODE HERE

# CODE END
```

write your solution only inside the indicated region unless the task explicitly tells you otherwise.

Do not modify the supplied environment or agent classes simply to make a task easier.

## Before You Start: Understand the World

Before writing any code, inspect the environment parameters near the beginning of the notebook.

Identify:

* the width and height of the grid;
* the positions of houses;
* the positions of parks;
* the actions available to a normal agent;
* the additional action available to a litter agent;
* the number of pieces of litter initially carried by an agent; and
* the amount of litter required to make a cell blocked.

#### Think about it

For each of the following, identify an example from the lab:

* **Agent**
* **Environment**
* **State**
* **Action**
* **Goal**

You do not need to modify any code for this task.

## Task 1 — Moving an Agent

### Task 1.1 — Complete the Simulation Loop

The function `simulate(...)` repeatedly allows an agent to interact with the environment.

Complete the indicated section so that, on each simulation step, the agent:

1. chooses its next action;
2. executes that action in the environment; and
3. records its new position in the trail.

The simulation should stop when either:

* the agent reaches its destination; or
* the maximum number of simulation steps is reached.

Run the supplied test after completing the function.

A successful implementation should produce a plotted route showing the agent moving from its starting location towards its goal.

#### Think about it

The basic agent repeatedly follows the cycle:

> **sense → decide → act → sense**

Identify where each part of this cycle appears in the code.

### Task 1.2 — Compare BFS, DFS and A*

The navigation agent can use different search algorithms.

Run the same navigation problem using:

* **BFS**
* **DFS**
* **A***

Keep the starting position and destination the same.

Do **not** implement these algorithms again. Use the implementations already provided.

#### Think about it

Compare the routes produced by the three algorithms:

1. Do all three algorithms reach the goal?
2. Do they produce the same route?
3. Which produces the shortest route in this example?
4. How does the route produced by DFS differ from BFS?
5. What additional information does A* use compared with BFS and DFS?

Also notice the difference between:

* the **path followed by the agent**; and
* the **search used to find that path**.

### Task 1.3 — What Kind of Environment Is This?

Consider the grid world from the perspective of the navigation agent.

For each property below, select the description that best fits the environment used in Task 1.

#### Accessible or inaccessible?

Can the agent obtain the information it needs about the grid when navigating?

#### Deterministic or non-deterministic?

If the agent performs the same valid action in the same state, does it always produce the same result?

#### Static or non-static?

Can the environment change independently while the agent is deciding what to do?

#### Episodic or non-episodic?

Can each movement be treated independently, or can earlier movements affect later decisions?

#### Think about it

Would your classifications change if:

* obstacles could move;
* the agent could only observe neighbouring cells;
* several agents moved at the same time; or
* actions sometimes failed?

### Task 1.4 — What Kind of Agent Is It?

Consider the navigation agent you have just used.

Which of the following descriptions best fit the agent?

* Reactive
* Model-based
* Goal-based
* Utility-based
* BDI

#### Think about it

Look at the implementation and check:

1. Does the agent have an explicit goal?
2. Does it plan a sequence of actions?
3. Does it use information about the structure of the environment?
4. Does it simply react to the current state?
5. Does it contain explicit representations of **beliefs, desires and intentions**?

> **Note:** Having a goal does not automatically make an agent a BDI agent. A BDI architecture explicitly represents beliefs, desires and intentions.

## Task 2 — Designing a Litter-Agent Policy

The first part of the lab focused on **navigation**: finding a way from one location to another.

The litter agent has an additional action:

```text
drop
```

The agent must now make two kinds of decisions:

> "Where should I move?"

and

> "When should I drop the litter I am carrying?"

The second decision is controlled by the agent's **policy**.

### Task 2.1 — Implement the Basic Litter Policy

Complete `litter_policy(...)` as described in the notebook.

The initial policy should cause the agent to drop its litter after its first successful movement when littering is enabled.

Run the supplied experiment several times.

Observe:

* the route followed by the agent;
* where litter is deposited;
* how litter accumulates in cells; and
* what happens when the blocking threshold is reached.

#### Think about it

What is the difference between:

* the agent's **search/navigation algorithm**; and
* the agent's **litter policy**?

For example, the agent may use DFS to decide where to move while using a separate policy to decide when to perform `drop`.

### Task 2.2 — What Kind of Behaviour Is This?

Consider the basic litter policy.

It follows a rule of the form:

> **when a condition occurs → perform an action**

#### Think about it

Which description best fits this behaviour?

* Reactive
* Model-based
* Goal-based
* Utility-based
* BDI

Also consider whether remembering that the first successful movement has already occurred means that the agent is using some internal state.

### Task 2.3 — What Happens Over Time?

Run the litter world for several journeys.

Observe what happens as litter accumulates.

Check:

1. Where is litter repeatedly deposited?
2. Do any cells become blocked?
3. Does blocking a cell affect later routes?
4. Can an action in one journey affect a later journey?
5. Does this suggest that the task is episodic or non-episodic?

The agent's actions can change the environment it encounters later.

### Task 2.4 — Design a Better Policy

Design a better `better_policy(...)` for the litter agent.

Your policy should make a better decision about when or where litter is dropped and reduce the likelihood of blocking useful routes.

Use the existing actions and environment. Do not modify the environment mechanics.

Before implementing the policy, think about:

* Should litter always be dropped immediately?
* Should the current cell be checked before dropping?
* Should the amount of litter already present matter?
* Are some cells better places to drop litter than others?
* Should reaching the destination affect the decision?

Then implement and test your policy.

### Task 2.5 — Compare the Policies

Run your improved policy over several journeys.

Compare it with the original policy.

Check:

1. Does the agent still reach its destinations?
2. Are fewer cells blocked?
3. Is litter distributed differently?
4. Are there situations where the improved policy still fails?

### Task 2.6 — Has the Type of Agent Changed?

Compare the navigation-only agent from Task 1 with the improved litter agent.

#### Think about it

For each description below, decide whether it applies to the litter agent.

##### Reactive

Does the agent respond directly to the current situation?

##### Model-based

Does it use information about the current world, previous actions or accumulated litter?

##### Goal-based

Does it act towards goals such as:

* reaching the destination;
* avoiding blocked cells; or
* disposing of litter?

##### BDI

Does the implementation explicitly contain:

* beliefs;
* desires; and
* intentions?
