---
layout: default
title: Home
---

[Home](index) | [Blackboard Page](https://www.ole.bris.ac.uk/ultra/courses/_269234_1/outline) | [Blackboard Forum](https://www.ole.bris.ac.uk/ultra/courses/_269234_1/engagement) | [Unit Catalogue](https://www.bristol.ac.uk/unit-programme-catalogue/UnitDetails.jsa?ayrCode=26%2F27&unitCode=COMS30094)
<br/>
Labs: [W1](lab-w1) | [W2](lab-w2) | [W3](lab-w3) | [W4](lab-w4) | [W5]() | [W7]() | [W8]()

# Welcome to Intelligent Agents Lab 4!

This lab covers normal-form games, Nash equilibria and Pareto optimality, repeated games and multi-armed bandits, with mixed equilibria as a bonus. The instructions are in `lab4.ipynb`.

## Setup

```bash
conda activate [your_env]
python -m pip install -r requirements.txt
jupyter notebook lab4.ipynb
```

## Lab Files

**[⬇ Download the Lab 4 starter files](https://www.ole.bris.ac.uk/bbcswebdav/courses/COMS30094_2026_TB-1/labs/lab-w4/lab4-initial.zip)**

The lab contains the following key files:

* `lab4.ipynb`
  The Jupyter notebook containing the lab exercises.

* `games.py`
  Defines the four payoff tables, the repeated-game strategies and the fishing-spot bandits.

* `viz.py`
  Prints the payoff tables and plots the bandit learning curves.

* `tests.py`
  Contains the checks you run after each task.

* `requirements.txt`
  Lists the Python packages the lab needs.

You should complete the exercises in `lab4.ipynb`.

Where the notebook contains:

```python
# CODE HERE

# CODE END
```

write your solution only inside the indicated region.

After each task, run the check cell below it, e.g. `tests.check_task1_nash(is_pure_nash)`. Each check prints PASS or FAIL. Unfinished tasks show FAIL but won't stop the rest of the notebook from running.

Do not modify `games.py`, `viz.py` or `tests.py`.

