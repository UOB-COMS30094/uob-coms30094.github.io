---
layout: default
title: Home
---

[Home](index) | [Blackboard Page](https://www.ole.bris.ac.uk/ultra/courses/_269234_1/outline) | [Blackboard Forum](https://www.ole.bris.ac.uk/ultra/courses/_269234_1/engagement) | [Unit Catalogue](https://www.bristol.ac.uk/unit-programme-catalogue/UnitDetails.jsa?ayrCode=26%2F27&unitCode=COMS30094)
<br/>
Labs: [W1](lab-w1) | [W2](lab-w2) | [W3]() | [W4]() | [W5]() | [W7]() | [W8]()

# Welcome to Intelligent Agents Lab 2!

In Week 1 you worked with a **single autonomous agent** that perceived its environment, reasoned and performed physical actions.

This week several autonomous agents share the same world. They continue moving while also sending and receiving messages.

The key idea is that one agent cannot directly change another agent's goal. It can **REQUEST** something, but the other agent decides whether to **AGREE** or **REFUSE**.

In this lab you will work with:

* three moving agents;
* private inboxes and delayed message delivery;
* `REQUEST`, `AGREE`, `REFUSE` and `INFORM`;
* conversation IDs and a simple interaction protocol;
* commitments with deadlines; and
* directional trust learned from repeated interactions.

The communication language is deliberately small: the aim is to understand **interaction between autonomous agents**, not memorise message syntax.

> **More detail:** `LAB2_HANDOUT.txt`, included in the starter files, gives a fuller explanation of the simulation, controls, protocol, trust model and suggested experiments.

## Setup

Use the same environment as Week 1:

```bash
conda activate agents_env
python -m pip install -r requirements.txt
```

Run the visualisation:

```bash
python run_lab.py
```

Run the checks:

```bash
python tests.py
```

Useful controls:

* `SPACE` — pause/run
* `A` — toggle autonomous requests
* `T` — cycle whose trust history is displayed
* `1` — manually request A0 → A1
* `2` — manually request A0 → A2
* `R` — reset
* `ESC` — quit

## Lab Files

[⬇ Download the Lab 2 starter files](https://www.ole.bris.ac.uk/bbcswebdav/courses/COMS30094_2026_TB-1/labs/lab-w2/lab2-initial.zip)

The main files are:

* `communication_agent.py` — agent communication, protocol and commitments;
* `message_world.py` — delayed message transport;
* `messaging.py` — messages, envelopes and commitments;
* `trust.py` — the trust model;
* `run_lab.py` / `pygame_view.py` — simulation and visualisation;
* `agent.py` / `empty_world.py` — navigation code reused from Week 1;
* `LAB2_HANDOUT.txt` — fuller lab instructions.

Complete the `# CODE HERE` sections in `communication_agent.py`, `message_world.py` and `trust.py`.

## Before You Start: From One Agent to Several

In Week 1 the basic cycle was:

```text
perceive environment → reason → physical action
```

This week an agent also perceives its **inbox** and may perform a **communicative action**:

```text
perceive environment + inbox
            ↓
          reason
            ↓
physical action and/or message
```

Sending a message does **not** directly invoke the receiver. Messages first travel through the world, enter the receiver's inbox, and are processed on a later reasoning step. Agents can therefore keep moving while messages are in transit.

The meeting protocol is:

```text
REQUEST → AGREE → INFORM(done/failed)

       or

REQUEST → REFUSE
```

`AGREE` creates a commitment by the responder. `REFUSE` does not.

#### Think about it

Why is sending a request different from directly assigning another agent a new destination?

---

## Task 1 — Communication and Commitments

### Task 1.1 — Start a Conversation

Complete `request_meeting(...)` in `communication_agent.py`.

Create a `REQUEST` message containing:

* sender and receiver;
* the meeting target;
* a fresh conversation ID.

Store the requester-side state as `REQUEST_SENT`, then send the message through the world.

#### Think about it

After `send_message(...)` returns, has the receiver necessarily processed the message? Why do we need conversation IDs when interactions repeat?

### Task 1.2 — Deliver Messages Asynchronously

In `message_world.py`, complete:

* `send_message(...)` — place an envelope in `world.in_transit` with a future delivery tick;
* `deliver_messages(...)` — move due messages into the receiver's inbox.

Do **not** call the receiver's message handler directly from `send_message(...)`.

#### Think about it

Which better represents independent agents?

```python
receiver._handle_request(message)
```

or:

```python
receiver.inbox.append(message)
```

### Task 1.3 — Agree, Refuse and Create Commitments

Complete `_handle_request(...)`.

The responder should agree only if:

* it is not already busy with another live meeting;
* the target is within `max_request_distance`; and
* its trust in the requester is at least `min_trust_to_accept`.

If it refuses, send `REFUSE` and create no commitment.

If it agrees:

1. create a commitment with the responder as **debtor** and requester as **creditor**;
2. assign a deadline;
3. send `AGREE` using the same conversation ID.

#### Think about it

Why does `AGREE`, rather than `REQUEST`, create the commitment? Why is an honest refusal not a trust failure?

### Task 1.4 — Process the Outcome

Complete `_handle_reply(...)`.

Handle:

* `AGREE` — move towards the meeting target;
* `REFUSE` — mark the conversation refused;
* `INFORM(status="done")` — record a fulfilled commitment;
* `INFORM(status="failed")` — record a violated commitment.

The supplied simulation checks whether commitments are fulfilled or violated as agents continue moving.

#### Think about it

Why should trust be updated when the outcome is known rather than when `AGREE` is received?

---

## Task 2 — Trust and Repeated Interaction

### Task 2.1 — Implement Direct-Experience Trust

In `trust.py`, each agent stores for every partner:

```text
successes = fulfilled commitments
failures  = violated commitments
```

Use:

```text
trust = (successes + 1) / (successes + failures + 2)
```

This gives:

```text
no evidence          → 0.50
1 fulfilled, 0 fail  → 0.67
0 fulfilled, 1 fail  → 0.33
```

A `REFUSE` does not change trust because no promise was made.

Complete `score(...)` and `record(...)`.

#### Think about it

Why can `Trust(A0,A1)` differ from `Trust(A1,A0)`?

### Task 2.2 — Reliability versus Learned Trust

The simulation's default underlying reliabilities are:

```text
A0 = 0.95
A1 = 0.75
A2 = 0.45
```

These are **ground truth for the experiment**. Agents do not read another agent's reliability directly.

Run the simulation for 200+ ticks and compare these values with the trust that agents learn from experience.

#### Think about it

Why might learned trust differ substantially from true reliability after only a few interactions?

### Task 2.3 — Choose Who to Ask

Complete `choose_partner(...)`.

Use trust to prefer the most trusted available partner, but retain a small exploration probability:

1. occasionally choose a random partner;
2. otherwise choose one of the most trusted partners;
3. break ties randomly.

#### Think about it

What could happen if exploration were zero after one unlucky early failure?

### Task 2.4 — Observe the Society Over Time

Let the simulation run with autonomous requests enabled.

Observe:

* different agents initiating conversations;
* messages travelling while agents continue moving;
* multiple conversation IDs appearing in the transcript;
* commitments being fulfilled or violated;
* trust values becoming directional and unequal.

Press `T` to inspect trust histories from different agents' perspectives.

### Task 2.5 — Try a Different Exploration Policy

Set:

```python
exploration = 0.0
```

and compare the behaviour with the default value.

Does an early failure make an agent less likely to be selected again?

### Task 2.6 — Add Another Agent

Add another entry to `AGENT_CONFIGS` in `run_lab.py`, for example:

```python
{"id": 3, "reliability": 0.60}
```

Run the simulation again.

#### Think about it

With \(n\) agents, how many **directed** trust relationships are possible?

---

## Lab Solutions

Lab 2 Solution (Will be made available!)
