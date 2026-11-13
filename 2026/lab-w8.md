---
layout: default
title: Home
---

[Home](index) | [Blackboard Page](https://www.ole.bris.ac.uk/ultra/courses/_269234_1/outline) | [Blackboard Forum](https://www.ole.bris.ac.uk/ultra/courses/_269234_1/engagement) | [Unit Catalogue](https://www.bristol.ac.uk/unit-programme-catalogue/UnitDetails.jsa?ayrCode=26%2F27&unitCode=COMS30094)
<br/>
Labs: [W1](lab-w1) | [W2](lab-w2) | [W3](lab-w3) | [W4](lab-w4) | [W5](lab-w5) | [W7](lab-w7) | [W8](lab-w8)

# Welcome to Intelligent Agents Lab 8!

This week, we explore how Large Language Models (LLMs) can act as intelligent agents in a multi-agent environment.

We will use [GovSim](https://github.com/giorgiopiatti/GovSim) (Piatti et al., 2024), a simulation environment in which agents must manage a shared resource while balancing individual needs and collective interests.

[0] Giorgio Piatti, Zhijing Jin, Max Kleiman-Weiner*, Bernhard Schölkopf, Mrinmaya Sachan, Rada Mihalcea. ``[Cooperate or Collapse: Emergence of Sustainable Cooperation in a Society of LLM Agents.](https://arxiv.org/abs/2404.16698)'' Proc. 38th NeurIPS.

You will build LLM-based agents, explore how their values and social norms influence their decisions, and investigate whether cooperation can emerge when agents face competing interests.


## Lab Files

**[⬇ Download the Lab 8 starter files](https://www.ole.bris.ac.uk/bbcswebdav/courses/COMS30094_2026_TB-1/labs/lab-w8/lab8-initial.zip)**

Start with **[lab8.ipynb](lab8.ipynb)**.

The lab downloads a small Qwen model through Ollama, demonstrates text generation, then turns that same model into an agent in GovSim's original fishing environment. Inference uses only your own computer.

| Part | What you do | Coding task |
|---|---|---|
| 1 | Set up Ollama and the model | |
| 2 | Send a prompt, then change its context | 1: build messages and call the LLM |
| 3 | Meet the GovSim fishing world and define a household | 2: write a persona |
| 4 | One agent: observe, decide, act, remember | 3: memory. 4: turn a decision into an action |
| 5 | Five agents given a norm | |
| 6 | Metrics, values, and a run with no norm | 5: compliance and shortfall |
| 7, optional | GovSim's original conversation and memory | |
| 8, optional | A vote to revise the norm |  |
| 9 | Reflection | |

Write your code between `#code here` and `#code end`.

## The three runs

| Run | What the households are told | What is added |
|---|---|---|
| Given a norm | A quota of 10 tons each, for every month | Nothing |
| No norm | No quota and no rule | Nothing |
| Revision | A quota of 10 tons each to start | In month 2 each household proposes a quota for itself, then all vote on the five proposals.|


## Set up


```sh
python3.11 -m venv .venv
```

On macOS/Linux, activate with `source .venv/bin/activate`; on Windows use `.venv\Scripts\Activate.ps1` (create it with `py -3.11 -m venv .venv`).
Then:

```sh
python -m pip install -r requirements-notebook.txt
python -m jupyterlab lab8.ipynb
```

## Optional Section 7
The original GovSim flow runs from section 7 of the notebook.
Set `RUN_ORIGINAL_GOVSIM = True` to download the `mxbai-embed-large` embedding model and run that section.
