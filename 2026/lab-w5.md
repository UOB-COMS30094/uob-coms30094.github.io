---
layout: default
title: Home
---

[Home](index) | [Blackboard Page](https://www.ole.bris.ac.uk/ultra/courses/_269234_1/outline) | [Blackboard Forum](https://www.ole.bris.ac.uk/ultra/courses/_269234_1/engagement) | [Unit Catalogue](https://www.bristol.ac.uk/unit-programme-catalogue/UnitDetails.jsa?ayrCode=26%2F27&unitCode=COMS30094)
<br/>
Labs: [W1](lab-w1) | [W2](lab-w2) | [W3](lab-w3) | [W4](lab-w4) | [W5](lab-w5) | [W7]() | [W8]()

# Welcome to Intelligent Agents Lab 5!

This lab covers tabular Q-learning, joint action learning with agent modelling (JAL-AM) and deep Q-learning (DQN) in a two-agent level-based foraging environment. The instructions are in `lab5-initial.ipynb`.

## Setup

Run these commands from the unzipped lab folder.

macOS:

```bash
bash setup_scripts/setup-mac.sh
source .venv/bin/activate
jupyter notebook lab5-initial.ipynb
```

Windows (PowerShell):

```powershell
powershell -ExecutionPolicy Bypass -File setup_scripts\setup-windows.ps1
.venv\Scripts\jupyter notebook lab5-initial.ipynb
```

## Lab Files

**[⬇ Download the Lab 5 starter files](https://www.ole.bris.ac.uk/bbcswebdav/courses/COMS30094_2026_TB-1/labs/lab-w5/lab5-initial.zip)**

The lab contains the following key files:

* `lab5-initial.ipynb`
  The Jupyter notebook containing the lab exercises.

* `Agents/tabular_q_learner.py`, `Agents/jal_am.py`, `Agents/q_learner.py`
  Base classes for the tabular Q-learner, the JAL-AM learner and the DQN policy.

* `posggym/`
  The level-based foraging environment and the pretrained agents.

* `setup_scripts/`
  Creates the Python environment.

You should complete the exercises in `lab5-initial.ipynb`.

Where the notebook contains:

```python
# CODE HERE

# CODE END
```

write your solution only inside the indicated region.

After each exercise, run the cell below it. Unfinished exercises raise `NotImplementedError` and stop the cells that depend on them.

Do not modify the files in `Agents/` or `posggym/`.
