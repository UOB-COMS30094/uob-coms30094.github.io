# COMS30097 — Intelligent Agents Coursework: Agents in Eregion Works

#### Coursework Files

**[⬇ Starter files](https://www.ole.bris.ac.uk/bbcswebdav/courses/COMS30097_2026_TB-1/coursework/CW-starter.zip)**

### 0: Setup the Environment

Ensure you have uv installed on your machine,

From the project folder, with `uv` installed, set up the environment and optional learning dependencies:

```sh
./setup_uv.sh --learning
source .venv/bin/activate
```

Run the four examples:

```sh
python -m examples.demo --agent random
python -m examples.demo --agent rulebased
python -m examples.demo --agent qlearning --train-episodes 10
```

You can use conda or venv to create your own python virtual environment if you wish. The required packages to run the code are: PyYaml, torch, and numpy

### 1. Introduction

**Eregion Works** is a company in Middle-earth that manufactures a range of products. Manufacturing each product requires completing a task, which may consist of several subtasks.

The company employs autonomous agents to carry out these tasks. It has several offices where agents work, either independently or in cooperation with other agents.

Each task has a deadline. Agents must decide how to complete their assigned tasks while maintaining the offices they share.

Working reduces the cleanliness of an office. Agents can clean offices, but cleaning takes time that could otherwise be spent completing tasks. If an office becomes too dirty, it is temporarily closed, disrupting work and potentially causing task progress to be lost.

The actions of one agent can therefore affect the ability of other agents to complete their tasks.

In this coursework, you will investigate how agents should behave in this environment, implement decision-making rules based on social norms, and develop agents that learn through experience.

You are provided with a Python implementation of the environment and example code demonstrating how agents select actions.

You are **not required to implement the environment from scratch**.

---

## 2. The Environment

### 2.1 Agents and Offices

The environment contains a collection of agents and offices.

Each agent has a unique identifier, a current location and, when assigned, a task to complete.

Each office has a unique identifier, a supported task type and a cleanliness level.

At the start of an episode, every office has its initial (maximum) cleanliness level.

Agents can perform three actions:

| Action | Description |
|---|---|
| `Work(room_id)` | Attempt to perform work in an office |
| `Clean(room_id)` | Clean an office, increasing its cleanliness |
| `Idle()` | Take no productive action |

Each action takes one simulation step.

Movement between offices is abstracted: an agent can choose an action targeting an accessible office without explicitly planning a route.

### 2.2 Tasks

The company generates tasks that are assigned to agents.

Every task consists of a fixed amount of work, defined by `work_required` in the environment configuration.

There are three task modes:

**Individual (`individual`)**

One agent is responsible for completing the task.

**Simultaneous (`simultaneous`)**

Two agents must cooperate to complete the task. Both must work on the task during the same simulation step for the relevant work to progress.

**Sequential (`sequential`)**

Two agents must contribute to the task in a particular order. The first agent must complete its portion before the second agent can begin.

Each task has a deadline. The deadline applies to the entire task, including both participants in a cooperative task.

If the task is not completed before its deadline, it expires.

### 2.3 Cleanliness and Maintenance

Each office has a cleanliness level, represented by `cleanliness`.

The initial and maximum cleanliness of an office is defined by `initial_cleanliness`.

Whenever an agent performs work, the office's cleanliness decreases by `task_damage`.

When an agent cleans an office, cleanliness increases by `clean_increase`, up to its maximum value.

An office becomes inaccessible when its cleanliness falls below `accessible_threshold`.

An inaccessible office cannot be used by agents.

When an office becomes inaccessible:

- Work and cleaning cannot take place in that office.
- Progress on unfinished tasks assigned to that office is lost.
- Deadlines continue to run.
- The office remains inaccessible for `restore_steps` simulation steps.

The company restores the office automatically. Agents do not carry out this restoration.

Once restored, the office becomes accessible again at its initial cleanliness level.

These conditions create a trade-off between completing tasks and maintaining the shared working environment.

### 2.4 Task Generation and Rewards

Tasks are generated throughout an episode.

The environment configuration determines how frequently new tasks are generated, whether they require cooperation, and the number of simulation steps available for completion.

Agents receive rewards for completing tasks. Working, cleaning and attempting to access inaccessible offices can incur costs.

The relevant values are defined in the YAML configuration.

### 2.5 Example Environment

The supplied example configuration contains three agents (`alice`, `bob` and `carol`) and three offices (`office1`, `office2` and `office3`).

Some important configuration parameters are:

| Parameter | Example value | Meaning |
|---|---:|---|
| `horizon` | 240 | Maximum number of simulation steps |
| `work_required` | 4 | Required work for each task |
| `deadline` | 16 | Task deadline in simulation steps |
| `task_damage` | 2 | Cleanliness reduction per accepted work action |
| `clean_increase` | 2 | Cleanliness restored by cleaning |
| `initial_cleanliness` | 20 | Initial and maximum office cleanliness |
| `accessible_threshold` | 1 | Minimum cleanliness required for access |
| `restore_steps` | 48 | Office restoration duration |
| `spawn_probability` | 0.7 | Probability of generating tasks for available agents |
| `cooperation_probability` | 0.5 | Probability that a task requires cooperation |
| `cooperative_mode` | simultaneous | Cooperative task mode |
| `reward` | 2.0 | Completion reward for each participant |

These parameters can be changed through the supplied scenario configuration files.

### 2.6 Simulation

The environment operates in discrete time steps.

At each step, agents receive observations, choose their actions, and the environment processes the actions together.

The environment updates task progress, office cleanliness, deadlines, rewards and office accessibility.

An episode continues until the configured horizon or an applicable termination condition is reached.

---

## 3. Coursework Tasks

You will complete four tasks.

### Task 1 — Normative Specification

**Your first task is to propose a normative specification describing how agents should behave at Eregion Works.**

Consider the responsibilities of agents towards the company, towards other agents and towards the shared offices.

Using concepts studied in the unit, identify and formally specify appropriate social norms.

For each norm, explain:

- The conditions under which the norm applies.
- The agents or other parties involved.
- What constitutes satisfaction or violation.
- Why the norm is appropriate for this environment.

You should also explain how conflicts between norms might arise and how they could be resolved.

**This is a specification task only. You are not required to write code for Task 1.**

You are not given predefined norms. You must develop and justify your own normative specification.

### Task 2 — Rule-Based Agent

Implement an agent that follows the norms specified in Task 1.

You should work primarily in:

`examples/rulebased.py`

The provided code contains a `RuleBasedAgent` class with a `decide()` function.

This function receives an observation of the current environment and returns a decision.

Your task is to replace the example decision-making behaviour with rules derived from your normative specification.

Your implementation should demonstrate how an agent selects actions in different situations and how potentially conflicting rules are handled.

You may create additional helper functions or classes.

**Evaluation**

Evaluate your rule-based agent in all three environment configurations:

1. Individual tasks.
2. Simultaneous cooperative tasks.
3. Sequential cooperative tasks.

For each configuration, compare your agent against the supplied random-action baseline.

Define and calculate suitable metrics, including task performance, office cleanliness and compliance with your proposed norms.

Produce appropriate graphs or tables and discuss your results.

You should explain whether your rules produce the intended behaviour, where they perform well, and where they have limitations.

### Task 3 — Learning Agent

Develop an agent that learns to select actions through interaction with the environment.

You should work primarily in:

`examples/qlearning.py`

The provided example includes the interface for a learning agent.

The `examples/qlearning.py` file contains the `QLearningAgent` class. Currently, the `decide()` method selects an available action randomly. Complete this method so that actions are selected using the Q-values stored in `self.values`.

During training, implement an **ε-greedy policy**, where the agent selects a random available action with probability ε and otherwise selects an action with the highest Q-value. Use:

\[
\epsilon = \max\left(0.05,\ 0.4\left(1-\frac{t}{100000}\right)\right)
\]

where \(t\) is the number of observed transitions (`self.observed`). During evaluation (when not training), the agent should always select an available action with the highest Q-value.

The `observe()` method is also incomplete. This method updates Q-values using experiences obtained from the environment. Complete the calculation of the target value and the Q-value update using `self.discount_factor`, `self.learning_rate`, the observed reward and the estimated value of the next state. Ensure that terminal transitions are handled appropriately.

**Evaluation**

Test your implementation using:

```bash
python -m examples.demo --agent qlearning --train-episodes 10
```

Use more episodes for meaningful training.

Evaluate your learning agent in the same three environment configurations used in Task 2:

1. Individual tasks.
2. Simultaneous cooperative tasks.
3. Sequential cooperative tasks.

Use multiple random seeds (at least five evaluation seeds are recommended) and comparable experimental conditions.

Training and evaluation episodes should be kept separate.

Compare the learning agent against both the random baseline and your rule-based agent.

Use appropriate metrics and visualisations.

In particular, investigate:

- How well does the learning agent perform?
- Were any of your proposed norms are reflected in its behaviour?
- Does it develop useful behaviours not explicitly included in your rule-based agent?
- Why might the rule-based and learning agents behave differently?

You should not assume that behaviour consistent with a norm necessarily means the agent has learned an explicit representation of that norm.

Your analysis should be supported by experimental evidence.

### Task 4 — Discussion and Extensions

Consider how the agents or the environment could be improved using other concepts studied in COMS30094.

For example, you could consider (note that this is not an exhaustive list):

- Allowing agents to exchange information and coordinate their actions, particularly for cooperative tasks.
- Investigating whether agents might avoid cleaning or rely on others to maintain shared offices.
- Introducing mechanisms to monitor compliance and reward or penalise agents for their behaviour.
- Exploring how large language models could support agent reasoning, planning, communication or decision-making.

Discuss at least two relevant extensions. For each extension, explain why it would be useful, how it might work, and what additional challenges it might introduce.

Relate your discussion to the limitations or results of your implemented agents.

**You are not required to implement these extensions.**

---

## 4. Starter Code Guide

You are provided with the source code for the environment.

The `src/` directory contains the simulator. You should read the relevant files to understand how the environment works, but you should **not modify files in `src/`**.

Your implementation should be placed in `examples/` or additional files that you create.

### 4.1 Code Overview

| File | Purpose |
|---|---|
| `src/environment.yaml` | Defines the simulation configuration |
| `src/scenario.py` | Defines configuration structures for agents, rooms, tasks and the world |
| `src/configuration.py` | Loads the YAML configuration |
| `src/state.py` | Represents the current state of the environment |
| `src/tasks.py` | Handles task generation, progress and deadlines |
| `src/actions.py` | Defines available actions and their outcomes |
| `src/observations.py` | Defines the information provided to agents |
| `src/agents.py` | Defines the interfaces for agents and learning agents |
| `src/transitions.py` | Processes actions and updates the environment |
| `src/environment.py` | Provides the main environment interface |
| `src/runner.py` | Runs episodes and invokes agent policies |
| `src/rewards.py` | Defines reward components |
| `src/events.py` | Defines task and room events |
| `src/metrics.py` | Calculates basic performance metrics |
| `examples/rulebased.py` | Your rule-based agent implementation |
| `examples/qlearning.py` | Your learning agent implementation |
| `examples/learning.py` | Example representation of observations for learning |
| `examples/demo.py` | Runs agents and reports simulation results |

### 4.2 Agent Observations

Your agent makes decisions using:

`decide(self, observation: Observation) -> Decision`

The `Observation` object provides information including:

| Property | Description |
|---|---|
| `observation.subject` | Identifier of the agent |
| `observation.agent` | Current state of the agent |
| `observation.task` | Currently assigned task, if any |
| `observation.room` | Current office, if any |
| `observation.state` | Overall environment state |
| `observation.available_actions` | Actions currently available |
| `observation.task_actions` | Convenient tuple containing work, clean and idle options |

You can inspect the following state information:

- `observation.state.rooms` — office states.
- `observation.state.agents` — agent states.
- `observation.state.tasks` — currently active tasks.

For an assigned task, useful attributes include:

`mode`, `participants`, `progress`, `work_required`, `remaining`, `room_id` and `leader`.

For an office, useful attributes include:

`cleanliness`, `initial_cleanliness`, `accessible_threshold` and `accessible`.

These attributes can be used to develop your decision-making policy.

### 4.3 Learning from Experience

Learning agents also use:

`observe(self, experience: Experience) -> None`

This method receives information about the previous observation, the decision taken, the resulting transition, the next observation and the reward.

You can use this information to update your learning policy.

### 4.4 Running and Evaluating Agents

The `rollout()` function in `src/runner.py` executes simulation episodes.

It calls each agent's `decide()` function, processes the agents' actions and provides learning experiences where appropriate.

The environment records task events, room events, action outcomes and rewards.

The `EpisodeMetrics` class in `src/metrics.py` provides basic measures such as:

| Metric | Meaning |
|---|---|
| `steps` | Number of completed simulation steps |
| `tasks_completed` | Completed tasks |
| `tasks_expired` | Tasks that expired |
| `tasks_pending` | Currently unfinished tasks |
| `task_completion_rate` | Proportion of tasks completed |
| `tasks_completed_per_step` | Task completion throughput |
| `agent_returns` | Accumulated reward for each agent |
| `mean_return` | Average return across agents |
| `social_welfare` | Aggregate reward-based system performance |
| `room_availability` | Proportion of office-time that remains accessible |

Additional information can be obtained from simulation events and action outcomes.

You will need to develop any additional metrics required for your evaluation, particularly metrics for norm compliance and performance by task mode.

You may use Python libraries such as Matplotlib or other suitable tools to produce visualisations.

### 4.5 Experimental Configurations

You should evaluate your agents using three configurations of the environment:

| Environment | Task mode | Configuration |
|---|---|---|
| Individual | `individual` | `cooperation_probability: 0.0` |
| Simultaneous | `simultaneous` | `cooperation_probability: 1.0`, `cooperative_mode: simultaneous` |
| Sequential | `sequential` | `cooperation_probability: 1.0`, `cooperative_mode: sequential` |

Use the supplied configuration files for these experiments.

Use the same evaluation configurations and random seeds when comparing different agent policies.

---

## 5. Submission Requirements

### 5.1 Coursework Report

Submit a report of **no more than four pages in ACM conference format**, including all figures, tables and references.

Use the following [Overleaf ACM template](https://www.overleaf.com/latex/templates/acm-conference-proceedings-primary-article-template/wbvnghjbzwpc) with these settings:

```latex
\documentclass[sigconf]{acmart}
\setcopyright{none}
\settopmatter{printacmref=false, printccs=false}
```

Remove any placeholder conference and publication information from the template.

Your report should cover:

1. **Normative specification:** Your proposed norms, their formal representation and justification.
2. **Rule-based agent:** Design, implementation and evaluation across the three environments.
3. **Learning agent:** Q-learning implementation, training and evaluation across the three environments.
4. **Comparison:** Performance differences and which norms are reflected in the learned behaviour.
5. **Discussion:** Limitations and potential extensions using other concepts from the unit.

Use suitable figures and tables to communicate your experimental findings.

The report should be concise and self-contained. More detailed implementation information may be included in your private GitHub repository.

### 5.2 Code and GitHub Repository

Submit the final version of the code along with the report. 

As you make progress on the coursework, maintain your coursework code in a **private GitHub repository**. 

You must:

- Keep the repository private.
- Include a link to your GitHub repository in your report. 
- Give GitHub user `niravajmeri` access to the repository.
- Include a README explaining how to run your agents and reproduce your experiments.
- Include the source code for your rule-based and learning agents.
- Include your evaluation and visualisation code.
- Include the link to your repository in your coursework report.

Do not make your solution repository public.

#### Reproducibility and Code Execution

Your submitted code must be executable and reproducible. Markers will run your code to verify its correctness and reproduce your results.

- Include a `README.md` with clear instructions for training agents and running evaluations
- Evaluation should complete within 10 minutes on a standard CPU-only machine, excluding installation and training.
- Training may take longer, but you must provide instructions to reproduce it and report the approximate training time.
- Save your trained Q-values so that markers can evaluate your learning agent without retraining it.

### 5.3 Code Overview Video

Provide a code overview video of **no more than five minutes**.

The video should briefly explain the code you have written, focusing on:

- Your rule-based agent and its decision-making rules.
- Your learning agent and its learning procedure.
- Your evaluation and visualisation code.
- How to run your implementation.

The video should be a walk through of the relevant source code and not a  demonstration of simulation output.

We are not looking for a fancy edited video. A simple screen recording with voice narration, clearly explaining your code, is sufficient. For example, you can start a Microsoft Teams meeting, share your screen and record your walkthrough. Upload the recording to OneDrive and include a link in your report. Ensure that the markers have permission to access the video.

The video does not carry separate marks. It is intended to help the markers understand and review your implementation.

---

## 6. Assessment and Marking Guidance

Your work will be assessed on the quality of your normative specification, the correctness and effectiveness of your agent implementations, the rigour of your experimental evaluation, and your understanding of wider Intelligent Agents concepts.

There is no single correct normative specification or agent policy.

We are interested in how well you justify your decisions, translate ideas into working implementations, and use evidence to explain the behaviour of your agents.

### Marking Scheme

| Task | Marks | Criteria |
|------|-------|----------|
| **Task 1: Normative Specification** | 20 | Appropriate and well-justified norms; clear formal specification; identification and resolution of conflicts |
| **Task 2: Rule-Based Agent** | 30 | Translation of norms into decision rules; correctness and quality of implementation; experiments and analysis across all three task modes |
| **Task 3: Learning Agent** | 30 | Correct implementation of Q-learning, including action selection and Q-value updates; controlled evaluation and visualisations across all three task modes; comparison of performance and norm-consistent behaviour |
| **Task 4: Discussion** | 20 | Identification of relevant extensions; technically sound proposals; critical discussion of benefits, challenges and limitations |
| **Total** | **100** | |

Report clarity, code quality and reproducibility will be assessed within the relevant criteria.

Marks will reflect the correctness of implementations, quality of reasoning and experimental evidence.

The code overview video is compulsory but carries no separate marks. It is intended to support the review and verification of submitted code.

### Guidance for Different Levels of Work

| Standard | Expected Evidence |
|----------|-------------------|
| **Excellent (70%+)** | Precise, well-justified norms; clear translation into decision rules; correct and effective implementations; rigorous evaluation across all three task modes using multiple seeds; insightful comparisons supported by quantitative evidence; thoughtful discussion of extensions and limitations. |
| **Good (60–69%)** | Appropriate norms and functioning agents; sound implementations; reasonable experiments across all three task modes; clear results and discussion, with some limitations in experimental depth or critical analysis. |
| **Satisfactory (50–59%)** | Basic normative specification and mostly functioning implementations; some meaningful evaluation and comparison, but limited justification, experimental rigour or interpretation. |
| **Basic (40–49%)** | Partial implementations or weak normative specification; incomplete experiments; limited analysis or insufficient evidence supporting conclusions. |
| **Insufficient (below 40%)** | Major missing components, substantially incorrect or non-functioning implementations, little meaningful evaluation, or inadequate demonstration of the intended concepts. |

