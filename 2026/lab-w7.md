---
layout: default
title: Home
---

[Home](index) | [Blackboard Page](https://www.ole.bris.ac.uk/ultra/courses/_269234_1/outline) | [Blackboard Forum](https://www.ole.bris.ac.uk/ultra/courses/_269234_1/engagement) | [Unit Catalogue](https://www.bristol.ac.uk/unit-programme-catalogue/UnitDetails.jsa?ayrCode=26%2F27&unitCode=COMS30094)
<br/>
Labs: [W1](lab-w1) | [W2](lab-w2) | [W3](lab-w3) | [W4](lab-w4) | [W5](lab-w5) | [W7](lab-w7) | [W8]()

# Welcome to Intelligent Agents Lab 7!

This lab covers resource allocation, voting rules (first past the post, Condorcet and single transferable vote) and second-price (Vickrey) auctions, using a train dispatcher at Bristol Temple Meads. The instructions are in `lab7-initial.ipynb`.

## Setup

The lab needs only `pygame-ce` and Jupyter. Run these commands from the unzipped lab folder.

```bash
pip install pygame-ce notebook
jupyter notebook lab7-initial.ipynb
```

## Lab Files

**[⬇ Download the Lab 7 starter files](https://www.ole.bris.ac.uk/bbcswebdav/courses/COMS30094_2026_TB-1/labs/lab-w7/lab7-initial.zip)**

The lab contains the following key files:

* `lab7-initial.ipynb`
  The Jupyter notebook containing the lab exercises.

* `pygame_gui.py`
  The station simulation and the Pygame dashboard.

You should complete the exercises in `lab7-initial.ipynb`.

Where the notebook contains:

```python
# CODE HERE

# CODE END
```

write your solution only inside the indicated region.

Each unfinished exercise raises `NotImplementedError` until you replace that line with your code. The simulation window keeps running and shows "not implemented yet" for an unfinished strategy.

Do not modify `pygame_gui.py`.
